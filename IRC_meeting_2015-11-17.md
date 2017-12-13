```
(12:02:01 PM) jjohansen: meeting time
(12:02:27 PM) tyhicks: hello
(12:02:31 PM) mdeslaur: \o
(12:03:00 PM) sarnold: woo
(12:03:14 PM) jjohansen: alright lets get started, current meeting agenda is http://wiki.apparmor.net/index.php/MeetingAgenda#Next_Meeting:_IRC_Meeting.2C_20:00_UTC.2C_Nov_17_2015
(12:03:19 PM) jdstrand: hey
(12:03:43 PM) ***sbeattie waves
(12:03:55 PM) jjohansen: please feel free to add to it, if you don't have edit permissions, there will be opportunity at the end of the meeting to add stuff
(12:04:11 PM) jjohansen: First up 2.11
(12:04:39 PM) jjohansen: so it seems to me we are looking at our next release to be sometime early next year
(12:05:09 PM) jjohansen: that will work well for ubuntu, what about the other distros
(12:05:27 PM) jjohansen: cboltz: how does that line up with what suse needs?
(12:05:58 PM) cboltz: you have probably heard about openSUSE Leap 42.1 which was released two weeks ago
(12:06:15 PM) cboltz: the next release will be in a year (if you look at Leap)
(12:06:25 PM) cboltz: or tomorrow (if you look at Tumbleweed) ;-)
(12:07:07 PM) sarnold: like, tomorrow tomorrow? or "every day is a release" tomorrow? :)
(12:07:32 PM) cboltz: ideally every day (Tumbleweed is the rolling release)
(12:07:35 PM) jjohansen: sarnold: tumbleweed is a rolling release
(12:07:37 PM) sbeattie: i didn't follow the Leap announcement closely, did non-SLEs-tracking releases get killed?
(12:08:24 PM) sarnold: okay, good, I thought I might have missed something else along the way, hehe
(12:08:24 PM) cboltz: Leap uses SLE 12 SP 1 as base and has lots of additional packages, mostly taken from Tumbleweed
(12:08:55 PM) cboltz: it also has some packages that are newer than what SLE has - for example AppArmor 2.10 ;-)  (SLE still has 2.8.x)
(12:09:27 PM) jjohansen: yikes
(12:09:53 PM) cboltz: well, as long as I don't have to maintain the SLE package... *eg*
(12:10:03 PM) jjohansen: :)
(12:10:19 PM) jjohansen: okay so a release early next year won't be an issue for suse
(12:10:26 PM) ***cboltz also noticed someone complaining about AppArmor 2.7 in Ubuntu 12.something LTS in a bugreport today
(12:10:27 PM) jjohansen: anyone know the debian schedule?
(12:10:46 PM) jjohansen: cboltz: hehe yeah, I cringe at that as well :)
(12:12:39 PM) jjohansen: well, any other input on the schedule?
(12:13:34 PM) jjohansen: if not we will follow what works for ubuntu which would be entering beta in late january
(12:13:35 PM) sbeattie: jjohansen: I would guess debian's next release in first half of 2017, based on recent pattern
(12:13:46 PM) tyhicks: https://wiki.debian.org/DebianStretch#Before_the_release
(12:13:58 PM) jjohansen: yeah
(12:14:13 PM) tyhicks: looks like the start of 2016 will be fine
(12:14:25 PM) jjohansen: yep
(12:15:00 PM) jjohansen: alright, we have a plan for 2.11
(12:15:10 PM) jjohansen: next up 2.9.3
(12:15:15 PM) jjohansen: and 2.10.1
(12:15:27 PM) jjohansen: 2.9.3 is past due
(12:15:29 PM) ***sbeattie groans
(12:15:53 PM) jjohansen: and really is just waiting on us to get around to cutting a release
(12:16:16 PM) jjohansen: I don't have any specific patches targeted for 2.9.3
(12:16:20 PM) sbeattie: do we feel a 2.10.1 is warranted?
(12:16:31 PM) cboltz: there is still a list of patches I'd like to have in 2.9.3 (see ML some months ago)
(12:17:07 PM) cboltz: the interesting question is - 2.10.1 or 2.11?
(12:17:17 PM) cboltz: we still didn't split off a 2.10 branch
(12:17:19 PM) jjohansen: cboltz: ack, weren't all of those patches acked? Its just a matter of pulling them back to 2.9.3?
(12:17:46 PM) jjohansen: but we did do a 2.10 release, and there are new feature developments
(12:17:51 PM) cboltz: (which makes sense IMHO - we had lots of changes that improve stuff, but hopefully nothing that breaks it ;-)
(12:17:54 PM) jjohansen: eg. parallel builds
(12:18:19 PM) tyhicks: the next topic (parser ABI bump) may warrant a 2.10.1 release
(12:18:20 PM) cboltz: jjohansen: there is still a list of maybe 10 patches that were only acked for trunk, but not for 2.9
(12:18:45 PM) jjohansen: cboltz: can you dig that list up, so we can get them in
(12:19:09 PM) cboltz: yes, I'll do that after the meeting
(12:19:23 PM) jjohansen: right, as tyhicks said we have an issue with some cache time stamp fixes
(12:19:52 PM) jjohansen: basically we didn't bump the compiler abi in 2.10 - which would force a policy recompile
(12:20:09 PM) jjohansen: but we had cache timestamp bugs that were fixed
(12:20:41 PM) jjohansen: this means people had stale policy that wasn't being detected as such, when a policy update was made
(12:21:01 PM) jjohansen: because the pre 2.10 compiler had given the cache files the wrong time stamp
(12:21:14 PM) sbeattie: well, that's the hypothesis anyway. 
(12:21:17 PM) jjohansen: bumping the compiler abi - would force a recompile
(12:21:18 PM) tyhicks: the post 2.10 compiler did, too
(12:21:51 PM) jjohansen: err aren't the fixes in 2.10? Its just respecting the timestamps that were already set
(12:21:53 PM) ***cboltz deletes the cache in %post since quite a while in the openSUSE packages
(12:22:23 PM) tyhicks: jjohansen: let me double check
(12:22:47 PM) jjohansen: jdstrand: the one caveat, is that if ubuntu phone picks this up. It will force policy recompiles
(12:22:48 PM) MoC [~u@5P6AABA4A.tor-irc.dnsbl.oftc.net] entered the room.
(12:22:55 PM) tyhicks: https://bugs.launchpad.net/apparmor/+bug/1484178 was fixed post 2.10
(12:23:08 PM) jjohansen: whether, now or in 6 months
(12:23:35 PM) jjohansen: tyhicks: ah, so that should be part of a 2.10.1 release and I think it warrants an abi bump
(12:24:11 PM) jjohansen: other wise we will run into more cache out of sync issues
(12:24:47 PM) jjohansen: opinions
(12:24:53 PM) jjohansen: ?
(12:25:08 PM) sarnold: seems like a good idea. sorry phone, sometimes updates will take forever.
(12:25:47 PM) cboltz: ;-)
(12:26:04 PM) sarnold: oh hey, if we coordinate with the parallel compiles patch, we might be able to hide some of it.. ;)
(12:26:17 PM) tyhicks: even if the phone didn't pick up this particular 2.10.1 bump, it'll pick up the same bump that will happen in 2.11
(12:26:58 PM) tyhicks: (as we stand today, 2.10.1 and 2.11 should have the same parser ABI if we go through with the ABI bump proposal)
(12:27:14 PM) jjohansen: yes
(12:27:33 PM) sbeattie: jjohansen: which abi are you proposing to bump? we have several.
(12:27:39 PM) jjohansen: we only bump the compiler abi for bugs where we want to force a recompile
sbeattie sbeattie_ 
(12:28:18 PM) jjohansen: sbeattie: specifically the compiler abi. In the cache file there are 3 version numbers/abis stored policy, compiler, kernel
(12:28:53 PM) jjohansen: the compiler abi, is only bumped when there are compiler issues that were fixed and we want to force a cache recompile
(12:29:05 PM) jjohansen: it does not affect policy version, or feature set
(12:29:43 PM) tyhicks: we're talking about what's defined at the top of parser/parser_common.c in the parser_abi_version variable
(12:29:56 PM) sbeattie: okay, thanks
(12:30:41 PM) ydev left the room (quit: Remote host closed the connection).
(12:30:43 PM) tyhicks:  * parser_abi_version:
(12:30:43 PM) tyhicks:  *   should be bumped when a compiler error or some other event happens
(12:30:43 PM) tyhicks:  *   and policy cache needs to be forced to be recomputed, when the
(12:30:44 PM) tyhicks:  *   policy_version or kernel version has not changed.
(12:31:19 PM) jjohansen: should probably update that to say kernel feature set has not changed
(12:31:44 PM) jjohansen: as the actual kernel version doesn't have anything todo with it
(12:32:39 PM) tyhicks: this bump seems like the right thing to do based on that comment in the code
(12:33:08 PM) tyhicks: I don't know exactly what it means for phone update times
(12:33:33 PM) tyhicks: do we have any real numbers handy?
(12:34:47 PM) sbeattie: nothing recent, but not much has changed. it'll be slow.
(12:34:55 PM) jjohansen: if the compile is done on the phone, a BQ aquaris with lots of policy installed, say 100+ apps. takes about 20 minutes to compile
(12:35:01 PM) jjohansen: it is really quite bad
(12:35:06 PM) tyhicks: oof
(12:35:35 PM) jjohansen: that being said, I don't expect to drop 2.10.1 on the phone images
(12:35:44 PM) sarnold: (by comparison, android's "optimizing apps" stage of updates is about that painful too. hooray parity.)
(12:36:24 PM) tyhicks: and if 2.10.1 or 2.11 did drop on the phone images, we could always patch parser_abi_version back to 1 in the overlay ppa
(12:36:24 PM) jjohansen: sarnold: but at least it tells you its optimizing instead of just staying on the boot logo for ever
(12:36:33 PM) jjohansen: yes
(12:36:42 PM) sarnold: jjohansen: true
(12:37:05 PM) jjohansen: also, the parallel compile patch (if it ever gets reviewed) should help build times a fair bit
(12:37:10 PM) Blub\w: not really sure comparing policy compilation and "app optimization" makes much sense :-P
(12:37:14 PM) jdstrand: I think what we would do is if the phone images needed a newer apparmor, we would backport the patch to the overlay
(12:37:44 PM) jdstrand: then they get 2.10.1 or 2.11 as part of the upgrade of the base image, which can be expected to get a policy recompile
(12:37:47 PM) sarnold: Blub\w: I assumed it was a euphamism on their part :)
(12:37:54 PM) jjohansen: Blub\w: true, but to the end user its the same type of experience of waiting for the phone to update and be ready to be used
(12:38:02 PM) jdstrand: ie, the 15.04 overaly stays the same; when they move to 16.04, they get the new apparmor
(12:38:11 PM) tyhicks: jdstrand: I agree
(12:38:26 PM) Blub\w: true
(12:38:40 PM) tyhicks: we should do the right thing in upstream AppArmor (bump the ABI) and deal with it ourselves in the overlay ppa
(12:38:43 PM) jjohansen: jdstrand: its not the base image that is the problem, its the installed apps
(12:39:01 PM) jjohansen: tyhicks: +1
(12:39:54 PM) tyhicks: we may be able to even add some intelligent logic to the overlay ppa to not invalidate cache files that seemingly have valid timestamps
(12:40:29 PM) tyhicks: yes, there's some gray area there on what is valid and what isn't
(12:40:53 PM) jjohansen: what this really cries out for is the policy hash as part of the cache
(12:41:09 PM) jjohansen: but ... that isn't happening yet
(12:41:20 PM) tyhicks: and that lessens the usefulness of the cache
(12:41:41 PM) jjohansen: tyhicks: the hash?
(12:41:55 PM) tyhicks: don't we avoid reading the profiles and abstractions today by only looking at the inode timestamps?
(12:42:03 PM) jjohansen: no
(12:42:07 PM) tyhicks: oh
(12:42:20 PM) jjohansen: we read and parse the files, so the front end
(12:42:24 PM) tyhicks: well, I'm all for the hash now :)
(12:42:27 PM) sarnold: we do find timestamps on all included what'sits, so it parses quite a bit..
(12:42:33 PM) jjohansen: we don't expand variables, or do the actual compile
(12:42:36 PM) tyhicks: oh, duh
(12:42:46 PM) tyhicks: don't mind me
(12:42:50 PM) sarnold: not the full thing, of course, but still, it does involve a bit of open/read/close work
(12:42:56 PM) jjohansen: yep
(12:43:25 PM) jjohansen: a fast hash would add very little overhead to that
(12:43:32 PM) tyhicks: agreed
(12:43:49 PM) tyhicks: did I do some work on that??
(12:44:08 PM) tyhicks: no
(12:44:22 PM) sarnold: someone looked for fast hashes, hehe
(12:44:26 PM) tyhicks: I'm thinking of the multiple caches work that never got completed
(12:44:39 PM) tyhicks: I looked into fast hashing for that
(12:44:39 PM) jjohansen: there are things we could do to reduce how many files we open, like caching includes, and moving to passing all the profiles to a single invocation of the parser install of execing the parser for each profile
(12:45:34 PM) jjohansen: tyhicks: so murmurhash is a fairly good fast hash, and sha on archs to support it in the cpu
(12:45:41 PM) jjohansen: but that is a discussion for another time
(12:45:48 PM) tyhicks: yes, I've derailed us
(12:45:52 PM) jjohansen: lets bump the abi and move on :)
(12:45:57 PM) tyhicks: agreed
(12:46:14 PM) jjohansen: next item up dropping python 2 support for 2.11
(12:46:15 PM) Blub\w: thought sha1 is on the agneda anyway ;-)
(12:46:26 PM) jjohansen: Blub\w: sort of, different area
(12:46:30 PM) sarnold: Blub\w: that's a different hash, hehe
(12:46:30 PM) Blub\w: right
(12:48:13 PM) jjohansen: anyone want to run with this? I am ambivalent, I know having python 2 support causes some issues, but it is nice being able to pull the current release back into LTS style releases
sbeattie sbeattie_ 
(12:48:24 PM) jjohansen: cboltz, sbeattie: ?
(12:49:27 PM) cboltz: 2.10 accidently is py2 only (at least some of the rule classes - str (py3) vs. unicode (py3))
(12:49:45 PM) cboltz: it took quite some time for someone to notice, but we finally have a bugreport about it
(12:50:12 PM) jjohansen: cboltz: py3 only or py2 only?
(12:50:15 PM) cboltz: and from the report it seems the distribution (scientific linux IIRC) doesn't include py3 yet :-/
(12:50:35 PM) cboltz: err, 2.10 is py3 only
(12:50:54 PM) jjohansen: which would be an indication that it isn
(12:51:02 PM) jjohansen: t quite time to drop py2
(12:51:41 PM) jjohansen: that being said, I avoid the python bits so I'd rather others speak up about it
(12:51:50 PM) cboltz: fixing that str vs. unicode is possible with some ugly code (to be hidden in a helper function)
(12:51:50 PM) tyhicks: seems to me like an indication that we've already dropped py2 support :)
(12:51:59 PM) tyhicks: why not just run with it?
(12:52:48 PM) sarnold: I think it's fair game to drop py2 at this point, but I'm also not directly affected much..
(12:52:50 PM) cboltz: well, we have one bugreport about it (I could also say "only one" ;-)
(12:54:00 PM) cboltz: staying compatible to py2 means we'll have to introduce a bit of ugly code
(12:54:08 PM) tyhicks: would be interesting to know the distro
(12:54:19 PM) sarnold: tyhicks: iirc scientific linux, a centos rebuild
(12:54:24 PM) sarnold: or rhel rebuild?
(12:54:36 PM) cboltz: https://bugs.launchpad.net/apparmor/+bug/1513880
(12:54:42 PM) sarnold: either way, python3 isn't coming to them very soon, and they've got selinux anyway, so it's unlikely to be a large demand
(12:55:00 PM) tyhicks: RHEL based
(12:55:26 PM) cboltz: right, I was already surprised that someone one a RHEL-based system uses AppArmor instead of SELinux ;-)
(12:55:42 PM) jjohansen: oh there are a few
(12:56:19 PM) tyhicks: but are they using their own custom kernel?
(12:56:31 PM) jjohansen: they used to
(12:56:54 PM) jjohansen: I heard that RH builds apparmor in now and you just have to select it
(12:57:06 PM) sarnold: this affects cboltz more than the rest of us -- i'd be fine with whatever he'd prefer
(12:57:07 PM) jjohansen: I haven't tried though
(12:57:36 PM) tyhicks: I'm fine deferring to cboltz, as well
(12:57:52 PM) cboltz: the problem is that I'm always testing with py3, so py2 issues are mostly unnoticed
(12:57:55 PM) tyhicks: I just don't want him having to juggle py2 support for one person
(12:57:56 PM) sarnold: if he's tired of seeing the ugly py2 and py3 compat code and wants to drop it, I can certainly understand :) if he wants to keep it around for a bit more for the rhel and rhel-rebuilds, that's fine too :)
(12:58:09 PM) cboltz: (unless sbeattie detects them when running the patches through his test setup)
sbeattie sbeattie_ 
(12:58:22 PM) jjohansen: sbeattie: ?
(12:58:22 PM) sbeattie: the other option would be to note that python2 is deprecated in 2.11 and kill it in 2.12 or whatever 2.11+1 is
(12:58:46 PM) cboltz: a deprecated note isn't too helpful IMHO
(12:59:05 PM) jjohansen: meh, it lets people know its coming
(12:59:08 PM) cboltz: it would mean I have to fix the py2 issues now (which introduces some ugly code)
(12:59:19 PM) cboltz: and then revert that next year
(12:59:27 PM) tyhicks: those py2 fixes would probably need to go into 2.10.1, too
(12:59:27 PM) cboltz: that sounds like wasting time to me ;-)
(12:59:33 PM) tyhicks: +1
(01:00:15 PM) jjohansen: well that sounds like we dropped py2 support in 2.10 to me
(01:00:30 PM) cboltz: accidently, but yes
(01:00:30 PM) jjohansen: lets make sure we add it to the release notes on the wiki
(01:00:40 PM) jjohansen: and move on
(01:00:41 PM) sbeattie: I'm okay with dropping it officially in 2.11, if that's the plan. It simplifies other things, too.
(01:01:18 PM) jjohansen: okay next up is the cross-distro repository
(01:01:23 PM) jjohansen: who wants to take this one?
(01:01:26 PM) cboltz: sbeattie: what about 2.10(.1)?
(01:01:42 PM) cboltz: do we fix py2-related bugs in it?
(01:02:16 PM) cboltz: or do we drop py2 support for 2.10.x too?
(01:02:57 PM) ***cboltz is not too keen to fix a py2 bug for 2.10.1 if we drop py2 support for 2.11
(01:03:37 PM) sarnold: I'd say "chef's choice", if there are bugs you'd rather fix elsehwere, that's fine by me..
(01:04:21 PM) sbeattie: I'd prefer to see them addressed on a best effort basis, but understand the desire not to.
(01:04:52 PM) cboltz: the py2 bug we know basically means we need a patch like
(01:05:04 PM) cboltz: -if type(foo) == str:
(01:05:28 PM) cboltz: +if type(foo) == str or (py2 and type(foo) == unicode):
(01:06:00 PM) cboltz: nothing horrible, "just" ugly
(01:06:15 PM) cboltz: (in practise, it would be a is_str_in_this_py_version() function)
(01:06:36 PM) cboltz: (to avoid we have to import sys (for sys.version) everywhere)
sbeattie sbeattie_ 
(01:08:35 PM) jjohansen: cboltz: I think I agree with what sbeattie said, lets treat it as a best effort. We note that it is broken in 2.10 and we are planning to remove support in 2.11, if someone submits patches to fix it we will review them and likely take them
(01:08:49 PM) jjohansen: if not, I guess it will stay broken in 2.10
(01:09:27 PM) cboltz: sounds like the community version ("feel free to fix it") of dropping it in 2.10 ;-)
(01:09:51 PM) jjohansen: well, that is the way open source projects go
(01:10:15 PM) jjohansen: I don't have the time to fix, and I know it is likely others don't
(01:10:36 PM) jjohansen: if someone steps up to fix it, we will do our best to support that
(01:11:19 PM) jjohansen: we can dictate that needs to be fixed all we want, but if someone doesn't have the time or desire to do it, it won't happen
(01:11:53 PM) jjohansen: I think we have dwelt on this long enough
(01:12:02 PM) cboltz: so the summary is: py2 support is broken in 2.10, and dropped in 2.11
(01:12:09 PM) ydev [~ydev@c-71-198-46-216.hsd1.ca.comcast.net] entered the room.
(01:12:27 PM) cboltz: (unless someone steps up and sends patches)
(01:12:35 PM) jjohansen: yes, I think that is a fair summary
(01:13:04 PM) cboltz: that means we should change the shebang lines to py3 (IMHO even in 2.10.1)
(01:13:34 PM) cboltz: and make sure that everything prefers py3 during build and install
(01:13:48 PM) cboltz: (libapparmor bindings and utils/Makefile)
(01:13:52 PM) jjohansen: I would rather leave that work for 2.11
(01:13:56 PM) sarnold: meh, sounds like work. maybe 2.11.
(01:14:29 PM) cboltz: I understand why you don't want it in 2.10.1 - but I also don't like the idea to default to a known-broken state
(01:15:45 PM) sarnold: someone may care enough to fix it
(01:16:25 PM) jjohansen: cboltz: ok, please feel free to submit a patch, we can note in it, that it will need to be reverted when the py2 breakage is fixed
(01:16:52 PM) cboltz: ok
(01:17:17 PM) cboltz: (I'll probably need someone to change the defaults for the libapparmor bindings)
(01:18:07 PM) jjohansen: does anyone want to own the cross-distro profile repository subject or should we postpone it to the next meeting
(01:18:42 PM) tyhicks: I don't
(01:19:17 PM) cboltz: we have some people who discussed it here (hi MoC and nicoo !), so we should at least have a quick look at it
(01:19:39 PM) cboltz: let me try a quick summary:
(01:19:52 PM) nicoo: Damn, forgot about the AA meeting  >_<'
(01:20:17 PM) cboltz: - first step: collect the profiles in all distributions in one repo (with per-distribution branches)
(01:20:45 PM) cboltz:   that's basically lp:apparmor-profiles, but without the "packaged in $pkg" placeholders
(01:20:52 PM) nicoo: cboltz: What did I discuss?
(01:22:09 PM) cboltz: the cross-distribution profile repo (or did you miss that informal part at debconf?)
(01:22:48 PM) cboltz: - there will be a file with metadata for each profile (in which package, complain/enforce/disabled)
(01:23:05 PM) nicoo: I was indeed there
(01:23:21 PM) cboltz: - step 2 is merging those profiles, with the goal to have one profile for all distributions
(01:23:54 PM) cboltz:   (with tunables to cover distro differences - only if really needed)
(01:24:03 PM) Blub\w: sounds like it needs quite a bunch of additional tunables
(01:24:23 PM) Blub\w: different paths / fs layout and whatnot
(01:24:48 PM) cboltz: not necessarily - we already handle things like /lib/ vs /lib64/ in the profiles, and allowing both doesn't hurt because typically only one of them exists
(01:25:29 PM) cboltz: an example where a tunable might make sense is chromium vs. chromium-browser
(01:25:40 PM) cboltz: but even that could be solved by a profile that covers both
(01:25:58 PM) cboltz: so to sum it up - tunables should be the exception
(01:26:52 PM) cboltz: - the repo will have quite some git branches, and merging them around might become funny
(01:27:48 PM) jjohansen: it would help if profiles transition to using a profile name separate from the attachment.
(01:27:48 PM) jjohansen: ie. profile firefox /usr/lib/firefox/firefox{,*[^s][^h]}
(01:27:48 PM) jjohansen: instead of just /usr/lib/firefox/firefox{,*[^s][^h]}
(01:27:54 PM) sarnold: yes
(01:28:01 PM) sarnold: yes yes yes a thosand times yes :)
(01:28:25 PM) cboltz: - the repo doesn't affect where/how to package profiles - they can be in the package they belong to or in a lots-of-profiles package
(01:28:31 PM) cboltz: jjohansen: agreed
(01:28:51 PM) nicoo: cboltz: Also, renaming home.d/ubuntu into home.d/vendor would be nice
(01:29:01 PM) cboltz: right, that's another thing we discussed
(01:29:13 PM) nicoo: Not absolutely necessary, but confusing for users  :V
(01:30:08 PM) cboltz: I think that's enough for the short version
(01:30:14 PM) cboltz: the long version is https://gobby.debian.org/export/debconf15/bof/AppArmor
(01:30:39 PM) cboltz: (or via gobby.debian.org if you prefer a gobby client)
(01:31:32 PM) cboltz: jjohansen, sarnold, sbeattie: what do you think about all this?
(01:31:39 PM) cboltz: does it sound sane or insane? ;-)
(01:31:53 PM) sarnold: sounds like a good idea to me
(01:31:57 PM) sarnold: what we've got now isn't ideal
(01:32:26 PM) jjohansen: well, in principle I like the idea of converging profiles more, I'm still not sure its sane for the different layouts etc
(01:32:42 PM) jjohansen: but I am willing to see what can be done
(01:32:45 PM) sarnold: i'm not sold on the idea of the metadata files, but let me read this bof notes later when i'm not hungry, hehe
(01:32:47 PM) nicoo: jjohansen: That's what vendor tunables are for
(01:33:25 PM) cboltz: sarnold: the metadata is helpful to track where a profile comes from, and where to submit it back after fixing it
(01:33:26 PM) jjohansen: nicoo: sure, I'm just not convinced it will work out as well as is hoped, but it is worth trying
(01:33:38 PM) nicoo: cboltz: Do we have evil plans to pester upstream into trying to get network/dbus/... stuff included in mainline kernel?
(01:34:17 PM) jjohansen: nicoo: hehe, sorry I keep planning on pushing those up, and keep getting delayed
(01:34:21 PM) jjohansen: we are getting closer
(01:34:25 PM) nicoo: ò_ó
(01:34:31 PM) nicoo: Awesome news  :D
(01:34:33 PM) nicoo: \o/
(01:34:37 PM) nicoo: jjohansen: Beware, though
(01:34:48 PM) nicoo: Now I know you are the one I need to pester  :>
(01:35:10 PM) nicoo: cboltz, jjohansen : Should/Could we have an AA meetup at 32C3?
(01:36:24 PM) cboltz: if jjohansen, sarnold and/or sbeattie come to 32C3, that would be a good reason for me to go there ;-)
(01:36:34 PM) jjohansen: nicoo: uh, no what I meant to say is someone else is supposed to being working on pushing those up :)
(01:36:45 PM) jjohansen: 32C3?
(01:36:59 PM) nicoo: The congress from CCC, in Hamburg
(01:37:03 PM) sarnold: cboltz: it's unlikely we'll make it to 32c3, the timing is all around pretty poor..
(01:37:09 PM) nicoo: Between Xmas and New Year's eve
(01:37:38 PM) jjohansen: ah, yeah. There is no chance of me making that
(01:37:50 PM) nicoo: :V
(01:37:57 PM) sarnold: move germany closer :)
(01:38:38 PM) cboltz: what about ubucon Europe? (somewhen in 2016 in Essen, Germany)
(01:39:06 PM) jjohansen: well that would help, but I am on vacation during that and will likely not even have internet accessable
(01:39:08 PM) nicoo: cboltz: Essen might be doable for me.  On the other hand, I would be going to Ubucon just for AppArmor
(01:39:49 PM) jjohansen: heh, oops the vacation comment applies to 32c3
(01:40:21 PM) jjohansen: ubucon might be more doable
(01:41:10 PM) cboltz: tell your boss that you already missed a big AppArmor meeting at debconf, and are not allowed to miss the next one at ubucon ;-)
(01:41:45 PM) cboltz: (the openSUSE conference might be another option, but that's a bit harder to explain to Canonical)
(01:42:17 PM) jjohansen: true
(01:44:20 PM) cboltz: back to the profile repo:
(01:45:07 PM) cboltz: is there anything you want to have changed, or should we start to implement it soon?
(01:45:38 PM) sarnold: if you've got cycles and interest t start working on it, please do :) whatever feedback is unlikely to change it to omuch
(01:46:11 PM) cboltz: the most interesting question is where to host it - launchpad?
(01:46:26 PM) cboltz: git instead of bzr would be nice
(01:46:49 PM) cboltz: (even if that means I finally have to learn more about git ;-)
(01:47:04 PM) jjohansen: cboltz: I am more than happy to have it in git
(01:47:29 PM) sarnold: same here
(01:47:45 PM) sbeattie: well, everything will be moving to git eventually, so having the profiles repo be th first to convert would be a good way to start that process
(01:48:03 PM) sarnold: launchpad sounds like a good first option, it's integrated into everything else we do ATM..
(01:48:16 PM) nicoo: +1 for git
(01:48:42 PM) ***nicoo doesn't have an opinion about Launchpad.
(01:49:04 PM) cboltz: https://help.launchpad.net/Code/Git lists what is supported and what not (aka "planned")
(01:49:27 PM) sarnold: oh, nice
(01:49:42 PM) cboltz: github might have some more features, the question is if we need them
(01:51:34 PM) Blub\w: my time for today is running out, so maybe after you're through with the meeting agenda someone could give me some pointers as to how to debug that issue that comes up with postfix
(01:51:37 PM) nicoo: Not sure.  Webhooks would be nice, at least to be able to trigger distro-specific automation whenever changes get merged
(01:51:53 PM) ***Blub\w shamelessly uses the fact that there are now people active here to point to https://bugs.launchpad.net/ubuntu/+source/lxc/+bug/1446906
(01:52:09 PM) Blub\w: gotta read the replies another time though
(01:52:53 PM) jjohansen: Blub\w: I will add a comment to the bug
(01:53:10 PM) nicoo: Blub\w: Could you setup auditd and collect all the AppArmor DENIAL messages?
(01:53:34 PM) sbeattie: nicoo: my understanding is that webhooks are undergoing beta testing right now.
(01:54:07 PM) nicoo: 'kay
(01:54:27 PM) Blub\w: jjohansen: great, thanks
(01:54:53 PM) Blub\w: nicoo: can do that some time this week, anything in particular though? the test I setup doesn't have any profiles loaded other than the one I aa-exec postfix with
(01:55:22 PM) Blub\w: off for tonight now, bye
(01:55:24 PM) nicoo: That sounds good (means we should only get Postfix-related messages
(01:55:26 PM) nicoo: )
(01:55:34 PM) nicoo: Good night, Blub\w
(01:56:19 PM) ***cboltz sees denials for profile="lxc-container-default" in Blub\w's dmesg - and they look related to postfix
(01:57:06 PM) jjohansen: sure its the disconnected path issue, I have added a comment and an example of what needs to be done
(01:57:11 PM) jjohansen: lets get back on topic
(01:57:18 PM) nicoo: cboltz: Oh, somehow missed them  :V
(01:58:47 PM) jjohansen: cboltz: are you done with cross-distro profile repository?
(01:59:40 PM) cboltz: we still need to decide on where to store that repo, but that can probably wait until the next meeting
(02:00:27 PM) cboltz: so IMHO we can continue with the next topic
(02:00:38 PM) jjohansen: I think I would default to launchpad just because everything else is there
(02:00:49 PM) jjohansen: alright moving on
(02:01:15 PM) jjohansen: systemd, has been the source of more than a few bug/support issues lately
(02:01:23 PM) jjohansen: specifically stop behavior
(02:01:59 PM) jjohansen: I'd like to propose we make stop dumb, and have it just echo out instrutions/warning on how to teardown policy if really needed
(02:02:11 PM) sarnold: +1
(02:02:50 PM) cboltz: I'm afraid it's the only sane and secure way
(02:03:20 PM) cboltz: so also +1 from me (unless someone gets ExecRestart= into systemd)
(02:03:23 PM) nicoo: jjohansen: A warning might also be the source of bug reports, but OTOH silently doing nothing wouldn't be good (and automagic teardown would be scary)
(02:04:10 PM) nicoo: cboltz: Would it really be only a matter of getting an ExecRestart parameter in systemd?
(02:04:17 PM) cboltz: nicoo: just to clarify - "stop behaviour" also means "restart behaviour" because systemd translates restart to stop ; start
(02:04:25 PM) jjohansen: nicoo: the problem is we are already dealing with bug issues from stop, where policy isn't working/things aren't confined. So we will get those reports either way. I'd just rather default to the more secure failure
(02:04:26 PM) nicoo: cboltz: Ah, ok
(02:04:27 PM) sarnold: nicoo: that'd be tough, lennart's said before he hates the idea
(02:04:33 PM) cboltz: so it's probably worse than it looks on the first view
(02:05:27 PM) nicoo: sarnold: Well, it makes the semantics more complicated, that's true  :V
(02:06:17 PM) jjohansen: sure, but there was a reason that it exists
(02:06:39 PM) jjohansen: anyways, I think we have enough of a consensus to push for the warning message change
(02:06:49 PM) cboltz: nicoo: last time I looked at the systemd.* manpages, I had some doubt systemd is really about simplification ;-)
(02:07:00 PM) jjohansen: apparmor isn't a userspace daemon, and the semantics of stop have serious implications
(02:07:10 PM) nicoo: cboltz: I tend to agree with you on that
(02:07:25 PM) nicoo: jjohansen: Agreed
(02:08:43 PM) jjohansen: on to restart, currently restart is a shell script, and I'd like to move it into the parser. I've already started the work, and when combined with the parallel build, we should be able drop all scripting around apparmor start/restart
(02:08:54 PM) jjohansen: any objections?
(02:09:05 PM) cboltz: sounds good
(02:09:29 PM) cboltz: what about unloading all null-* profiles (the init script does this nowadays)
(02:09:55 PM) nicoo: Stupid question, but what are the null-* profiles?
(02:10:06 PM) cboltz: the perfect solution would be "unload all _unused_ null-* profiles"
(02:10:25 PM) cboltz: nicoo: profiles used to track exec in complain mode
(02:10:30 PM) jjohansen: the null-* profiles are learning profiles that get autogenerated in complain mode
(02:10:36 PM) nicoo: Ah, ok
(02:10:55 PM) jjohansen: cboltz: I am all for a little script to unload them
(02:11:20 PM) jjohansen: fixing the kernel to auto-remove them when no longer referenced is still on my to do list
(02:11:52 PM) jjohansen: I don't think restart should necessarily unload them though, it should be a deliberate user action
(02:12:48 PM) cboltz: well, if you reload all profiles, it's very unlikely that you still need the null-* profiles at that moment
(02:13:00 PM) cboltz: and a new exec in complain mode will create a new null-* profile anyway
(02:14:19 PM) cboltz: can we restrict the removal from userspace to unused profiles? or is the "unused" information only available in the kernel?
(02:14:31 PM) jjohansen: sure, but the reload would also strip them from tasks that are currently using them
(02:14:51 PM) jjohansen: the unused info is currently not available
(02:16:08 PM) cboltz: just FYI: there's a debian bugreport about aa-status taking forever
(02:16:27 PM) cboltz: the bugreporter killed it while checking a 6-level-deep null-* chain ;-)
(02:17:03 PM) cboltz: (in other words: removing those profiles is needed in one way or another)
(02:17:07 PM) sarnold: .. was it only six? or were only six levels visible to the exception handling?
(02:17:34 PM) cboltz: it was already shortened, so it might indeed be more than six
(02:18:08 PM) jjohansen: likely
(02:18:23 PM) jjohansen: and yes there are all kinds of improvements needed around learning mode
(02:18:47 PM) jjohansen: that is going to take new kernel code
(02:18:50 PM) jjohansen: patches welcome
(02:19:52 PM) cboltz: you probably don't want kernel patches from me ;-)
(02:20:45 PM) sarnold: .. and these patches would be brutal to write :) you don't want to try..
(02:21:01 PM) sarnold: I asked jjohansen about it yesterday, it sounded well outside my abilities :(
(02:21:19 PM) jjohansen: sarnold: well I don't know about brutal, there is the already existing label code example to follow :)
(02:21:40 PM) jjohansen: but they aren't trivial, it involves RCU and locking
(02:22:26 PM) sarnold: a usersapce app to remove the null-xxx profiles would be a thousand times easier :)
(02:22:31 PM) jjohansen: yes
(02:22:39 PM) jjohansen: alright lets move on
(02:22:45 PM) jjohansen: cache behavior
(02:22:50 PM) jjohansen: we have 2 topics
(02:23:25 PM) jjohansen: first up the compiler has lots of knobs to control its behavior. This used to be necessary, before a the timestamp work was added
(02:23:34 PM) jjohansen: should we clean this up a bit
(02:23:42 PM) sarnold: ideally, yes
(02:23:48 PM) jjohansen: or should we just fix the bugs in the existing flags
(02:23:57 PM) sarnold: they were all added to hack things around the weak cache
(02:24:11 PM) jjohansen: eg. like the one around warning about updating the cache on profile removal I submitted a patch for the other day
(02:24:14 PM) sarnold: and supportuing them all has made it harder to improve the quality of the cache along the way
(02:24:18 PM) jjohansen: there are several similar bugs
(02:24:55 PM) jjohansen: that is how I feel
(02:25:09 PM) ***MoC is here now
(02:25:12 PM) MoC: sorry!
(02:25:12 PM) sarnold: hey MoC 
(02:25:22 PM) cboltz: hi MoC 
(02:25:23 PM) jjohansen: speak up, or I am going to start ripping out flags and cleaning the code up a bit
(02:25:27 PM) jjohansen: hi MoC
(02:25:31 PM) sarnold: jjohansen: these little cleanups feel alright for 2.10.x. but 2.11 ought to just be cleaned as we can.
(02:26:04 PM) jjohansen: sarnold: ack, I will commit them to both, until I have replacement code in place
(02:26:21 PM) jjohansen: alright, lets move on to the other hashing topic
(02:26:53 PM) jjohansen: I sent an email out yesterday "apparmor policy sha-1 behavior"
(02:27:20 PM) nicoo: jjohansen: I'm not (yet) on the upstream ML.  What was it about?
(02:27:46 PM) jjohansen: basically we can stick with the current scheme of how the kernel does sha1 hashes of loaded policy
(02:27:56 PM) sbeattie: nicoo: https://lists.ubuntu.com/archives/apparmor/2015-November/008794.html
(02:27:58 PM) jjohansen: which requires a userspace aa-sha1sum tool
(02:28:36 PM) jjohansen: or we can change to a scheme where the in kernel sum would match up with sha1sum of the policy file cache
(02:28:37 PM) nicoo: Thanks
(02:28:55 PM) jjohansen: each choice has advantages and disadvantages
(02:28:58 PM) sarnold: we kind of already require an aa-sha1sum tool -- to do cache inspection. whether or not it also does sha1 feels like a separate issue to me
(02:29:17 PM) jjohansen: sort of
(02:29:21 PM) sarnold: but using simple standard tools instead of making our own feels like a big win
(02:29:37 PM) sarnold: aa-cache-ls vs aa-sha1sum :) heh
(02:29:41 PM) jjohansen: if I spit out an aa-sha1sum tool you won't need sha1sum
(02:30:00 PM) nicoo: sarnold: On the other hand, a SHA1 implementation isn't big
(02:30:11 PM) jjohansen: and yes, in either case we will want a tool on top to compare loaded policy and what is in cache
(02:30:12 PM) nicoo: and the current behaviour seems more correct
sbeattie sbeattie_ 
(02:30:56 PM) jjohansen: sbeattie, jdstrand, tyhicks^ any preference?
(02:31:57 PM) cboltz: nicoo: actually we have a aa-sha1 already (not in bzr yet) - it's 112 lines of bash code ;-)
(02:32:09 PM) sarnold: cboltz: none of us want that version though :)
(02:32:27 PM) cboltz: well, it works and is much better than nothing ;-)
(02:32:29 PM) jdstrand: I don't feel like I have enough of an understanding to express a strong opinion
(02:32:30 PM) jjohansen: it is better than nothing
(02:32:37 PM) Stallmanu [~Stallmanu@CPE00fc8d4ac3c3-CM00fc8d4ac3c0.cpe.net.cable.rogers.com] entered the room.
(02:33:44 PM) cboltz: I think the interesting difference is sha1 per profile vs. sha1 per cache file
(02:34:02 PM) jjohansen: yes
(02:34:22 PM) sarnold: I think I vaguely prefer per-profile but can't articulate why.
(02:34:30 PM) jjohansen: though, a replace can make that not even the case
(02:34:34 PM) cboltz: in the kernel, it's all about profiles, so having a sha1 per profile (as we have now) would fit better
(02:34:42 PM) cboltz: even if it's a bit harder for userspace
(02:35:20 PM) jjohansen: ie. if going with per cache file, it is possible a profile replacement won't generate a cache file or will create a new cache file that is separate from a grouped cache file
(02:35:23 PM) cboltz: (but probably easier for the kernel - with a sha1 per cache file, the kernel needs to remember from which group of profiles (= cache file) a profile was loaded)
(02:36:01 PM) jjohansen: meh, it doesn't really remember. It takes the sha1 on policy load and only stores that
(02:36:23 PM) jjohansen: the sha1 per cache file gives you the relationship of which profiles were loaded together
(02:36:41 PM) cboltz: ok, just storing the sha1 makes things easier ;-)
(02:36:50 PM) jjohansen: the per profile, an easier match to profile state
(02:37:06 PM) jjohansen: either case you really need a userspace tool
(02:37:10 PM) sbeattie: right, easier to compare how two sets of profiles differ
(02:37:18 PM) jjohansen: to extact what profiles are part of the cache
(02:37:53 PM) jjohansen: so I think there is a slightly leaning towards keeping the current state
(02:38:08 PM) nicoo: It looks to me as if we all agree on current per-profile behaviour, yes
(02:38:24 PM) jjohansen: In that case I will add it to my weekend work queue beat a tool into shape for 2.11
(02:38:58 PM) jjohansen: alright does anybody have anything else they would like to bring up
(02:39:16 PM) jdstrand: does sha1 per profile have any impact on profile composition?
(02:39:17 PM) nicoo: jjohansen: I can try helping with the userspace tool, though I'm probably not knowledgeable enough on the AA internals for that.
(02:39:18 PM) cboltz: I'd accept the bash aa-sha1 (with my fixes included) ;-)
(02:40:34 PM) sarnold: nicoo: converting from bash to C ought to be sufficient :)
(02:40:56 PM) jjohansen: jdstrand: depends on what you mean by profile composition. It does mean the sha1 calculation has to be a little smarter when parts of the cache file are shared, but as for delegation and stacking based composition no
(02:41:00 PM) nicoo: sarnold: Fair enough, I ought to be able to do that.
(02:42:08 PM) jjohansen: nicoo: if you want to run with it, by all means do. I can point you at the bash util and cboltz request of changes
(02:42:22 PM) sarnold: nicoo: https://lists.ubuntu.com/archives/apparmor/2015-September/008552.html
(02:42:26 PM) nicoo: jjohansen: Please do so after the meeting
(02:42:41 PM) jjohansen: I can also work with you on the library interface bits for dealing with cache files
(02:42:41 PM) nicoo: >_<'
(02:42:45 PM) nicoo: sarnold: Thanks
(02:42:55 PM) nicoo: jjohansen: That would be probably very helpful
(02:42:56 PM) jjohansen: I expect there is more work at the library level we will want
(02:43:08 PM) sarnold: yeah
(02:44:02 PM) cboltz: why keep a simple bash script, when you can also have C and use some libraries? *SCNR*
(02:44:35 PM) jjohansen: hehehe :)
(02:44:41 PM) darix: cboltz: naw. you write a dbus service withs with a C++ Qt app for the user side
(02:44:56 PM) nicoo: darix: Vala rather than C++
(02:45:09 PM) darix: nicoo: that makes for too short code ;)
(02:45:10 PM) nicoo: Ok, perhaps we should stop the troll here.
(02:45:19 PM) jjohansen: :)
(02:45:20 PM) jjohansen: okay I think that is it then, unless anyone has another topic they want to bring up
(02:45:29 PM) darix: did you know that the facebook IOS app is 18000 objective C classes?
(02:45:46 PM) cboltz: my usual reminder - pending patches
(02:45:56 PM) nicoo: darix: Is there a AppArmor hat for each of them?
(02:46:02 PM) cboltz: sbeattie, I'm looking at you ;-)
(02:46:19 PM) darix:  http://www.darkcoding.net/software/facebooks-code-quality-problem/
(02:46:27 PM) cboltz: will you review my pending patches, or should I commit them as acked-by <timeout> tomorrow?
(02:47:06 PM) sbeattie: cboltz: I'm planning to look at them this week
(02:47:07 PM) jjohansen: cboltz: I will see what I can do today, no promise to get to all of them
(02:47:11 PM) jdstrand: jjohansen: it was the (future) cache file sharing I was thinking about
(02:47:58 PM) jjohansen: jdstrand: okay, yes future cache sharing makes both cases more difficult, in different ways
(02:48:19 PM) jjohansen: per profile needs to include a sum of the shared pieces
(02:48:19 PM) cboltz: sbeattie: ok, let's do the usual mode - for every two patches you review, you get an additional day
(02:48:39 PM) jjohansen: cache level needs to deal with unsharing inconsistencies
(02:49:09 PM) jjohansen: so for the next meeting because the Nov meet is late and we are going to lose a good chunk of Dec I am inclined to say let go with Jan 12
(02:49:32 PM) darix: jjohansen: i would have had time on december 25th
(02:50:01 PM) jjohansen: darix: oh that is a good day, I won't be able to make it! :)
(02:50:16 PM) jdstrand: the 12th sounds fine
(02:50:17 PM) sarnold: jan 12 wfm
(02:50:23 PM) nicoo: wfm?
(02:50:26 PM) nicoo: Ah, works for me
(02:50:40 PM) tyhicks: that'll do for me
(02:51:08 PM) cboltz: jan 12 sounds good
(02:51:13 PM) nicoo: gOOD FOR ME
(02:51:21 PM) nicoo: 20h UTC again?
(02:51:31 PM) jjohansen: yes, 20:00 UTC
(02:51:44 PM) jjohansen: or at least that has been the standard time
(02:52:05 PM) jjohansen: basically 2nd tuesday of the month 20:00 UTC is what we try for
(02:52:05 PM) nicoo: Wait, actually that conflicts with some thing (on the other hand, I'm definitely not crucial for the meeting...)
(02:52:46 PM) jjohansen: nicoo: well feel free to propose another time
(02:53:01 PM) jjohansen: doesn't have to be now it can be done on the mailing list
(02:53:12 PM) nicoo: Wednesday 13 Jan., 20:00 ?  :3
(02:53:21 PM) jjohansen: that wfm
(02:53:39 PM) cboltz: wfm
(02:53:48 PM) sarnold: also wfm
(02:54:48 PM) nicoo: jdstrand, tyhicks ^
(02:56:12 PM) tyhicks: should be fine
(02:56:22 PM) nicoo: Ok, great
(02:56:38 PM) ***nicoo got to go if they want to get some sleep at a reasonable time.
(02:56:43 PM) nicoo: Bye!  o/
(02:56:50 PM) jdstrand: wed is fine
(02:56:55 PM) jjohansen: alright lets set it for Wed Jan 13 20:00 UTC, please let yell before then if its an issue
(02:57:08 PM) jjohansen: thanks everyone, meeting adjourned
``` 
