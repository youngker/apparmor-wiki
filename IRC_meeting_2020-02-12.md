 ```
(10:02:27 AM) jjohansen: Talkless, sbeattie, sarnold, cboltz, jdstrand: meeting time
(10:02:41 AM) cboltz: hi everybody
(10:02:56 AM) jjohansen: hey cboltz
(10:04:43 AM) jjohansen: cboltz: its going to take a few minutes for some of the other people to show
(10:05:28 AM) cboltz: no worries, I still have some (unrelated) mails to read ;-)
(10:09:50 AM) Talkless: hi
(10:09:58 AM) jjohansen: hey Talkless
(10:16:24 AM) jjohansen: well hopefully people will trickle in now
(10:16:28 AM) ***sbeattie waves
(10:16:32 AM) jjohansen: lets get started
(10:17:37 AM) jjohansen: do we have any more input on https://gitlab.com/apparmor/apparmor/-/merge_requests/212
(10:18:01 AM) jjohansen: if we are going to make this change I would like to get it in so we can have it for 3.0
(10:19:03 AM) sbeattie: ugh sorry, I've not been able to dig deep into that one. 
(10:19:37 AM) Talkless: that was quite some time we discussed about it in some previous meeting :>
(10:19:48 AM) jjohansen: indeed
(10:19:53 AM) cboltz: the test results show that this MR will add some permissions, and I'm still not sure if we want that
(10:20:06 AM) cboltz: (and even if, someone will have to update the tests)
(10:20:18 AM) jjohansen: it does indeed, and that is my hesitation as well
(10:20:31 AM) jjohansen: on the other hand the rule set is cleaner
(10:21:27 AM) jjohansen: right, the issue really is we have let this sit too long, we need to figure out what we are doing, so we can move it forward or close it
(10:21:41 AM) cboltz: oh, I can make it even more clean and shorter ;-)   -   file,
(10:22:13 AM) jjohansen: or you know just remove/disable apparmor ;-)
(10:22:19 AM) Talkless: just skimmed through it agai, yea it's cleaner.
(10:22:37 AM) Talkless: cboltz: tests do show that? cool.
(10:22:58 AM) cboltz: we don't have tests that guarantee to detect it
(10:23:29 AM) cboltz: it's more like somewhat random test failures in the tools that check abstractions for permissions for a given path, and compare it to the expected permissions (as hardcoded in the test)
(10:24:02 AM) Talkless: cboltz: what is added speciffically?
(10:24:37 AM) cboltz: I think the added tunables/usrmerge explains it best
(10:24:47 AM) cboltz: @{bin}={/usr,}/{,s}bin
(10:24:48 AM) cboltz: @{lib}={/usr,}/lib{,32,64,/@{multiarch}}
(10:24:50 AM) cboltz: @{run}={,/var}/run
(10:24:56 AM) cboltz: @{run} is just a simplification
(10:25:10 AM) cboltz: but @{bin} and @{lib} allow several alternative paths
(10:25:28 AM) jjohansen: yeah, I think we could split @{run} out and just merge that change now
(10:25:50 AM) jjohansen: @{bin} treats everything as merged bin
(10:26:01 AM) Talkless: how alternative paths are a problem? If application originally is "expected" to run /usr/bin/foo, it could run /sbin/foo, and that's an issue?
(10:26:47 AM) cboltz: it could allow additional and unwanted paths in exec rules
(10:26:50 AM) Talkless: these are kinda admin-managed paths, right..?
(10:26:52 AM) jjohansen: I don't know that its an issue, its a matter of how tight do we want the default abstractions to be
(10:27:07 AM) jjohansen: Talkless: yes
(10:28:13 AM) cboltz: IIRC we always hoped to get the abstractions more tight, and this MR would be a big step into the opposite direction
(10:28:46 AM) Talkless: ..and reverting it will break bunch of profilles all over the interwebs and repositories and whatever..
(10:29:09 AM) Talkless: I mean, if this is merged, and someone actually finds security issue wrt to it.
(10:29:19 AM) jjohansen: right
(10:30:19 AM) Talkless: maybe renaming @{lib_maby_unsecure} would help? :D
(10:30:26 AM) Talkless: renaming to*
(10:30:34 AM) cboltz: lol
(10:30:51 AM) peetaur left the room (quit: Ping timeout: 480 seconds).
(10:31:18 AM) Talkless: maybe*
(10:31:25 AM) jjohansen: okay, well think about it, if you have an opinion please add a comment to the MR
(10:31:28 AM) sdeziel: the @{run} is non controversial so I'd like to see it merged along with the first commit defining other vars. Then I'd let profile authors use those less secure ones if they want to
(10:32:06 AM) cboltz: agreed on @{run}
(10:32:09 AM) Talkless: sdeziel: yeah good idea, leave tunable declaration, but not use them. Use only @{run}
(10:32:15 AM) jjohansen: sdeziel: that is probably the best first step
(10:32:18 AM) Talkless: do not use them in abstractions*
(10:32:27 AM) cboltz: but I'm not a fan of providing variables we consider possibly insecure
(10:32:42 AM) sdeziel: cboltz: I'd add a big fat warning along with them
(10:33:08 AM) Talkless: cboltz: someone will still write that lib,32,64,multiarch,whateer somewere :)
(10:33:25 AM) Talkless: if profile is tested on multiple distros
(10:35:07 AM) cboltz: if that's needed for cross-distro compability, then I'm fine with it - but I'd prefer not to use it blindly and "just in case"
(10:35:35 AM) peetaur [~peter@p57AAA3B5.dip0.t-ipconnect.de] entered the room.
(10:36:35 AM) jjohansen: so I have asked nl6720 to separate out the @{run}, @{lib}, and @{bin}
(10:36:37 AM) jjohansen: changes
(10:37:03 AM) jjohansen: that gives us a path to start moving this forward again
(10:37:22 AM) cboltz: well, first sort out @{run} (which should be an easy merge then)
(10:37:27 AM) jjohansen: yep
(10:37:45 AM) Talkless: ..if author is not fed up with us :)
(10:37:59 AM) cboltz: for @{bin} I wonder if we should make multiple variables out of it, depending on the original paths
(10:38:35 AM) cboltz: for example, if something lived in /usr/sbin/ forever, I could maybe agree to also allow /usr/bin/ - but also allowing /bin and /sbin doesn't make sense
(10:38:49 AM) sdeziel: cboltz: having @{sbin} and @{bin} would feel better IMHO
(10:38:57 AM) ottok: Hi! In issues milestones on AppArmor there is currently 2.12. Do you plan to create a "3.0" and mark the issues you want to have done by 3.0 release?
(10:39:15 AM) ottok: https://gitlab.com/apparmor/apparmor/-/milestones
(10:39:47 AM) sbeattie: ottok: we haven't used gitlab milestones much, but we can, sure.
(10:39:55 AM) cboltz: sdeziel: ideally @{usr-bin}, @{usr-sbin}, @{bin} and @{sbin} - where the last two would allow /bin and /usr/bin (and similar for sbin)
(10:40:04 AM) cboltz: needless to say that this is also the most complex way ;-)
(10:40:23 AM) cboltz: but, well, splitting @{lib} could become even more funny...
(10:42:01 AM) cboltz: anyway - let's get @{run} first to get things moving
(10:42:08 AM) Talkless: yes
(10:43:00 AM) jjohansen: ottok: I'll work on getting a milestone setup
(10:43:19 AM) jjohansen: lets move on
(10:43:28 AM) sbeattie: also, given the delay on this, it'd be nice if one of us could shepherd it through so it's not necessarily all on nl6720 to land
(10:44:11 AM) jjohansen: right, I am willing to split out the @{run} changes and commit it
(10:45:39 AM) jjohansen: I am even willing to split the other pieces but I can't update his merge request and would have to open a new one so I want to give him a chance at that
(10:46:03 AM) Talkless: we can ask hims is he interested to do even more work on it
(10:46:32 AM) jjohansen: a potential path forward depending on how nl6720 reponds is to take his patches and then do split patches on top of it
(10:47:18 AM) jjohansen: Talkless: I already have asked if he will do the split if not, I'll setup an MR with his patches as the base and split patches on top
(10:47:48 AM) jjohansen: I will take @{run} work and get it merged so that he can see some of his work going in
(10:48:01 AM) jjohansen: that might encourage him to do the other part of the split
(10:48:40 AM) jjohansen: lets move on
(10:50:04 AM) jjohansen: does anyone have anything else they would like to bring up?
(10:50:24 AM) cboltz: well, our meeting agenda has some things ;-)
(10:50:44 AM) Talkless: what's up with that usr etc?
(10:51:06 AM) cboltz: https://en.opensuse.org/openSUSE:Packaging_UsrEtc
(10:51:11 AM) jjohansen: cboltz: yes, but I purposely switched the order, so new things could be discussed before we run out of time and start bikeshedding
(10:51:31 AM) cboltz: ;-)
(10:52:19 AM) jjohansen: Talkless: the usr etc change is largely done, but I was thinking I would like to see it moved behind a variable so a site admin can easily change it
(10:52:27 AM) cboltz: Talkless: basically openSUSE wants to have the "default config" in /usr/etc/ and an (long-term goal) nearly empty /etc/ with only files the admin changes
(10:52:43 AM) cboltz: yes, a variable might make sense
(10:53:02 AM) jjohansen: suse isn't the only ones, there are several projects shooting for RO root images
(10:53:08 AM) cboltz: but I'd propose to only use it for read access (and maybe lock)
(10:53:23 AM) cboltz: and use /etc/whatever for write accesses
(10:53:56 AM) cboltz: (actually - would it make sense to enforce this via tests, or would that limit us too much?)
(10:54:47 AM) jjohansen: I think it might limit us too much, as I don't think everyone has settled on the same RO root solution
(10:55:28 AM) cboltz: I'd still expect that writes should go to /etc, but not /usr/etc/
(10:56:10 AM) cboltz: so my proposed test would basically be     grep '@{etc}.*  .*w.*,     and error out if it finds something
(10:56:19 AM) jjohansen: well eg. you could have /etc/ be an overlayfs mount over something and they both look like /etc/
(10:56:31 AM) jjohansen: it just depends on if you are in the right mount ns
(10:56:47 AM) cboltz: writes to /etc/ would obviously be allowed by the test
(10:56:56 AM) cboltz: I just want to prevent writes to /usr/etc/
(10:57:30 AM) jjohansen: yeah I get that
(10:57:54 AM) jjohansen: I am not opposed to splitting ro and rw etc
(10:58:29 AM) jjohansen: but lets call the var @{etc_ro} or something
(10:58:39 AM) cboltz: yes, good idea
(10:58:47 AM) Talkless: I don't get it, what does variable expanding to path has to do with acutall permissions? :/
(10:59:30 AM) sarnold: wait didn't we just have a meeting? :) oof :)
(10:59:31 AM) Talkless: oh you mean @{etc_ro} should mean /usr/etc, and @{etc_rw} to /etc/ ?
(10:59:43 AM) Talkless: hi sarnold, kinda :)
(10:59:45 AM) cboltz: exactly
(11:00:10 AM) cboltz: well, @{etc_ro} should be /etc/ and /usr/etc/
(11:00:17 AM) Talkless: oh right
(11:00:58 AM) jjohansen: Talkless: no @{etc_ro} would include both but with just read permission
(11:01:43 AM) jjohansen: sarnold: yes, but we only dealt with the web page yesterday, we delayed the rest of the meeting due to not getting the announcement email out
(11:01:58 AM) sarnold: aha :)
(11:02:05 AM) Talkless: jjohansen: you mean we can write acutall permission ,full line, into variable? 
(11:02:10 AM) Talkless: full rule
(11:02:27 AM) jjohansen: hrmmm, actually no, not yet ...
(11:02:57 AM) jjohansen: I do have a wip change that will allow variable expanse to do some interesting things
(11:03:28 AM) cboltz: sounds scary ;-)
(11:03:34 AM) Talkless: so I guess no point on having @{etc_x} ?
(11:03:36 AM) Talkless: yet.
(11:03:38 AM) jjohansen: I have wanted for a new file rule class around fstype
(11:03:38 AM) Talkless: or is it?
(11:04:05 AM) jjohansen: basically
(11:04:05 AM) jjohansen:   fstype=proc <pid>/* r,
(11:04:15 AM) jjohansen: can then be written as
(11:04:34 AM) jjohansen: @{proc}/@{pid}/* r,
(11:04:44 AM) cboltz: Talkless: If wee agree that rules using @{etc_ro} may only have rk permissions, I'll happily write a test to enforce it ;-)
(11:04:52 AM) jjohansen: and the fstype conditional can be expanded from the var etc
(11:05:06 AM) jjohansen: cboltz: yeah that is good for me
(11:05:25 AM) jjohansen: the magic var expansion isn't going to hit for a while
(11:05:53 AM) jjohansen: its not a priority for 3.0
(11:05:53 AM) Talkless: cboltz: I still don't get it, you will still need to write full path, like @{etc_ro}/foo/zoo.cfg rk,
(11:06:34 AM) cboltz: your example would allow /etc/foo/zoo.cfg rk, and /usr/etc/foo/zoo.cfg rk,
(11:06:39 AM) Talkless: even if jjohansen implementas that magic stuff, not sure how it will help
(11:06:41 AM) jjohansen: Talkless: yes, but @{etc_ro}  now covers /{etc, }
(11:06:57 AM) jdstrand: cboltz: in our policy and abstractions, I like your idea of @{etc_ro}. I like the name cause a profiler should really question a rule like: @{etc_ro}/foo w, :)
(11:07:12 AM) jjohansen: and a site admin can change that in a single place instead of touching all policy
(11:08:19 AM) ***jdstrand nods
(11:08:59 AM) jjohansen: can we move on?
(11:09:09 AM) jdstrand: I like that. I too have seen asks for ro root images and concur no one seems to be standardizing on things
(11:09:25 AM) jdstrand: so having that flexibility: +1
(11:09:42 AM) jdstrand: jjohansen: I can :)
(11:09:47 AM) jjohansen: :)
(11:09:54 AM) jjohansen: alright policy reorg
(11:10:10 AM) jjohansen: I took a first stab at reworking the policy repository
(11:10:11 AM) jjohansen: https://gitlab.com/apparmor/apparmor-profiles/-/merge_requests/40
(11:10:38 AM) jjohansen: there is a lot of work to do still, and several unanswered questions
(11:10:53 AM) jjohansen: like how we are going to fold in the profiles from the main repo
(11:10:53 AM) ***jdstrand had looked at that and likes the idea, but hasn't had time to review it yet (sorry)
(11:11:01 AM) jjohansen: np
(11:11:15 AM) jjohansen: its just a wp and won't be part of the 3.0 release
(11:11:27 AM) jjohansen: there are a few goals
(11:11:39 AM) jjohansen: get all the policy in one place
(11:12:10 AM) jjohansen: make it easier to track diffs between distros and get things folded back into the reference policy when appropriated
(11:12:14 AM) Talkless: well, I was asking about something similar in https://lists.ubuntu.com/archives/apparmor/2018-June/011689.html , I didn't looked ant that MR  too deeply, but it seems it similar.
(11:12:24 AM) jjohansen: and have policy releases separate from the code releases
(11:12:37 AM) jjohansen: Talkless: yep
(11:12:55 AM) Talkless: basically, profiles versioned by AA version.
(11:14:05 AM) Talkless: guestion is, how long a branch will be supported, how far back backprots should go?
(11:14:06 AM) jjohansen: so I don't want to spend a lot of time on this, but I want to make sure people are thinking about it, so we can move this forward
(11:14:09 AM) Talkless: case-by-case?
(11:15:14 AM) Talkless: some fox for application might be relevant only for latest openSUSE/Sid/Ubuntu release, as old AppArmor releases means older distribution version AND so older application version, which does not hit a DENIED.
(11:15:14 AM) jjohansen: Talkless: that has not been decided yet, there will certainly need to be some case-by-case changes, but what the general policy is still needs to be decided
(11:15:33 AM) jjohansen: and that is going to have to be driven largely by the needs of the different distros
(11:15:50 AM) Talkless: I guess anyone can backport to older release if feel needed.
(11:16:28 AM) jjohansen: letting distros branch will also help wtih them being able to differ if changes don't suit their needs
(11:16:48 AM) Talkless: well that can be fixed using more tunables in profiles
(11:17:01 AM) Talkless: so disdtribution can deploy tunables/firefox.d/ubuntu
(11:17:14 AM) jjohansen: yes
(11:17:23 AM) Talkless: or tunables/firefox.d/openSUSE file with tunables being appended
(11:17:39 AM) Talkless: I guess branching is fore more serious derivations
(11:17:46 AM) jjohansen: right
(11:17:59 AM) jdstrand: this isn't terribly different from how Debian and Ubuntu have shared debian/ packaging
(11:18:00 AM) jjohansen: say there is a backport that debian can't accept
(11:18:06 AM) jdstrand: it works well
(11:18:36 AM) jdstrand: distros can branch (if desired) or not. master can pull in changes, distros can cherrypick, etc
(11:18:44 AM) jjohansen: right, I think it certainly better than what we are currently doing
(11:18:47 AM) Talkless: I see, right.
(11:19:49 AM) jjohansen: the big questions are really around how we merge the profiles that are currently in the apparmor release and the extra profiles
(11:20:01 AM) jjohansen: and the organization of all this in the repo
(11:20:22 AM) jjohansen: also if we should start tracking profiles that are shipped in packages
(11:20:44 AM) jjohansen: that aren't part of the release and how best to integrate that if we do it
(11:21:30 AM) jjohansen: I will try to get a proposal together to discuss at next months meeting
(11:21:43 AM) jjohansen: but if you have ideas please add some comments to the MR
(11:21:44 AM) Talkless: Yeah these are huge quesitons
(11:22:02 AM) jdstrand: jjohansen: otoh (well, I've thought about it a teeny bit more than that, but not much ;) it feels like we should slurp everything in from the distros' latest version, provided the profile is worthy of shipping
(11:22:14 AM) Talkless: I feel the dilemma, should new profile be proposed to upstream project, to apparmor repo, or ... 
(11:22:35 AM) Talkless: (I mean personal dilemma, if I would want to create new profile)
(11:22:44 AM) jdstrand: jjohansen: for those that we don't pull in, we can perhaps clean them up and add them or ignore them for now
(11:23:05 AM) jdstrand: jjohansen: the distros can, at their discretion, branch and add the profiles we didn't slurp in
(11:23:41 AM) cboltz: Talkless: the answer is "it depends" - and to be more exact, it depends on how much the upstream project cares about having AppArmor profiles
(11:23:46 AM) jjohansen: Talkless: I think new profiles should be proposed to the repo, the question becomes how do organize the repo. eg. how do indicate certain profiles are considered in dev/unstable ...
(11:24:19 AM) jjohansen: I think we want to slurp in as much as possible
(11:24:44 AM) jjohansen: its a matter of organizing and messaging that, upstream doesn't consider this ready yet for some of them
(11:24:53 AM) jdstrand: Talkless: re upstream vs apparmor. that is going to depend. I think both probably makes sense. having it included in the apparmor repo will give some assuredness that it is ok
(11:25:07 AM) Talkless: I see. I had feeling what it would be nice if upstream projects would collect their profiles, reducing load form AA team, but I guess that's too naive, there's not much who knows AA profile writing...
(11:25:14 AM) jdstrand: jjohansen: agreed
(11:26:17 AM) jjohansen: well even if its upstream its nice for us to be able to track what they are doing, which will help us catch changes we make affecting an upstream
(11:26:18 AM) Talkless: I did experiment, managed to merge AA profile for some niche chat application, but any new updates are hard to push through, just lack of people to review https://github.com/qTox/qTox/pull/5855
(11:26:37 AM) jjohansen: right
(11:26:41 AM) Talkless: It does look more realistically to "slurp" to AA repo...
(11:28:19 AM) jjohansen: alright lets move on
(11:28:47 AM) jjohansen: 3.0 is a wip, I hope to drop a beta in a couple of weeks
(11:29:13 AM) Talkless: jjohansen: does it mean Debian will have that tasty networking (and more?) at last? :)
(11:29:18 AM) jjohansen: so if you have something you want to get in, please submit your merge requests soon
(11:29:27 AM) jdstrand: jjohansen: is the beta something we should upload to Ubuntu 20.04?
(11:29:44 AM) jjohansen: Talkless: yes, this should enable basic networking in debian on newer kernels
(11:30:04 AM) Talkless: yay
(11:30:04 AM) jjohansen: jdstrand: that is something we are going to have to decide soon
(11:30:17 AM) Talkless: jjohansen: any more highlights?
(11:30:24 AM) Talkless: is there a "todo" changelog?
(11:30:29 AM) Talkless: WIP changelog*
(11:30:52 AM) jjohansen: uhmmm, I have a todo list locally for what I am trying to get in
(11:31:04 AM) jjohansen: highlights with be ABI rules
(11:31:12 AM) jjohansen:   ABI rules
(11:31:17 AM) jdstrand: jjohansen: I don't know the timing. but let's say for a second the stars align and the beta is ready and packaging done to upload for feature freeze. later uploads likely wouldn't need FFes. even if we missed FF, an earlier FFe is always better. it would not be ideal to ship a beta that isn't ready though (obviously)
(11:31:17 AM) Talkless: versioning?
(11:31:21 AM) jjohansen:   overlay support for policy and config
(11:32:09 AM) jjohansen: jdstrand: right, atm we can't make the call. We need to wait and see
(11:32:54 AM) jjohansen: Talkless: no versioning yet, though ABI rules provide a type of versioning its not what you are looking for
(11:33:00 AM) jjohansen: uhmm lets see what else
(11:33:03 AM) Talkless: oh ok.
(11:33:12 AM) jjohansen: v8 network
(11:33:25 AM) jjohansen: xattr attachment conditional
(11:33:29 AM) Talkless: v8?
(11:33:33 AM) jdstrand: jjohansen: it sounds like best case is probably an early FFe...
(11:33:37 AM) jjohansen: kill, unconfined flags
(11:33:52 AM) jjohansen: access prefix
(11:33:56 AM) jdstrand: jjohansen: if so, we can defer that (Ubuntu-specific) decision for the moment
(11:34:24 AM) jjohansen: and maybe increased xindex range, and nnp override rules
(11:34:31 AM) jjohansen: jdstrand: yes
(11:34:38 AM) ***jdstrand salivates over nnp overrides
(11:34:55 AM) jjohansen: there are also some lib improvements, and lots of bug fixes
(11:35:38 AM) jjohansen: lets move on
(11:35:40 AM) ***Talkless salivates over all that completely unknown stuff :D
(11:35:50 AM) jjohansen: :-)
(11:36:12 AM) jdstrand: jjohansen: distros aren't immediately required to jump to v8 network, correct?
(11:36:56 AM) ***jdstrand needs to think through the policy changes wrt the 20.04 kernel
(11:37:05 AM) jjohansen: jdstrand: distros who want to carry compatibility patches will be able to have both v7 and v8 at the same time, v8 is what is upstream
(11:37:06 AM) jdstrand: I don't need to do that here of course
(11:37:21 AM) jjohansen: this just opens up access to the v8 kernel rules
(11:37:23 AM) jdstrand: jjohansen: right, that is what I remembered
(11:37:28 AM) jdstrand: ok, cool
(11:38:45 AM) jjohansen: apparmor.net is live, currently its lets encrypt cert has not been issued so no https yet
(11:38:57 AM) jjohansen: but www.apparmor.net does have a cert and is working
(11:39:14 AM) jjohansen: apparmor.dev and www.apparmor.dev are also waiting on certs
(11:39:41 AM) jjohansen: I don't have any visibility on how long this will take, gitlab just reports
(11:39:50 AM) jjohansen: GitLab is obtaining a Let's Encrypt SSL certificate for this domain. This process can take some time. Please try again later. 
(11:39:59 AM) tyhicks: maybe someone can jiggle the mouse
(11:40:08 AM) Talkless: nice site
(11:40:09 AM) jjohansen: hahaha
(11:40:20 AM) Talkless: didn't knew there was apparmor_status
(11:40:29 AM) Talkless: (also it needs sudo on Sid)
(11:40:47 AM) Talkless: I was using aa-status
(11:40:50 AM) Talkless: is there a difference?
(11:41:28 AM) cboltz: yes, the name is longer ;-)
(11:41:30 AM) cboltz: lrwxrwxrwx 1 root root 9 10. Feb 08:35 /usr/sbin/apparmor_status -> aa-status*
(11:42:10 AM) jjohansen: once the certs are available, we will start publicizing, make announcement to the list and on twitter
(11:42:22 AM) jjohansen: a big thanks once again to ottok for doing this
(11:42:35 AM) sbeattie: +1
(11:42:41 AM) Talkless: Well, that site suggest to use apparmor_status, and later aa-status to list profiles, but that's the same executable? :>
(11:42:55 AM) jjohansen: Talkless: aa-status needs access to privileged resources
(11:42:59 AM) jdstrand: yes, thanks! :)
(11:43:18 AM) Talkless: well apparmor_status is not available for me without sudo
(11:43:28 AM) jjohansen: Talkless: yeah it should be, apparmor_status is the legacy name, before we went to trying to be consistent with aa-
(11:43:51 AM) Talkless: ...due to paths of course.
(11:43:54 AM) jjohansen: Talkless: interesting, I assume its only in sbin
(11:43:54 AM) Talkless: PATH's
(11:45:21 AM) jjohansen: Does anyone have anything else that needs to be brought up?
(11:45:24 AM) tyhicks: does anyone mind if I enable 'issues' (bug tracking) for the apparmor.net repo?
(11:45:53 AM) jjohansen: tyhicks: no, please do
(11:45:55 AM) Talkless: jjohansen: sbin is not in PATH for user
(11:45:55 AM) sbeattie: tyhicks: no, please go ahead
(11:46:06 AM) Talkless: in Sid at least
(11:46:22 AM) tyhicks: done
(11:47:56 AM) Talkless: jjohansen: https://paste.debian.net/1130361/
(11:48:44 AM) jjohansen: yeah, that makes sense
(11:49:11 AM) jjohansen: Talkless: just use aa-status, we will get the docs updated to not mention apparmor_status
(11:49:38 AM) jjohansen: this is going to be important because after 3.0 I am going to start opening the tools to be non-privileged
(11:49:56 AM) jjohansen: and they will report status that is available to the user
(11:51:03 AM) jjohansen: thank you everyone, next meeting is Tuesday March 11, at 18:00
(11:51:09 AM) jjohansen: meeting adjourned
 ```