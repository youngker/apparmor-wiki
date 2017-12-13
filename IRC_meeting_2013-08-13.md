```
(01:01:39 PM) jjohansen: cboltz, kshitij8, sarnold, sbeattie, kees, jdstrand, mdeslaur: meeting time
(01:01:46 PM) mdeslaur: \o
(01:01:51 PM) jjohansen: oops tyhicks^
(01:01:59 PM) cboltz: I'm here ;-)
(01:02:08 PM) kshitij8: Present for now :)
(01:02:13 PM) tyhicks: Hello
(01:02:23 PM) ***sbeattie waves
(01:02:45 PM) jjohansen: alright lets get started
(01:02:59 PM) jjohansen: kshitij8: your up GSoC status report
(01:04:18 PM) kshitij8: Im working on a profile writing module to preserve comments like we discussed
(01:04:50 PM) kshitij8: After that I'd say i can roll out a few alpha version tools
(01:05:32 PM) kshitij8: cboltz am i over estimating?
(01:06:33 PM) cboltz: I wouldn't say so
(01:06:42 PM) cboltz: the interesting part might be the writing module ;-)
(01:06:48 PM) ***kshitij8 apologizes for any slow responses in advance
(01:07:09 PM) ***jdstrand is here now
(01:07:12 PM) cboltz: please keep in mind that we also discussed that logprof should be able to write to local/* instead to the main profile
(01:07:35 PM) cboltz: (we still have to discuss if we do this with a config option, comment in the profile, whatever)
(01:07:37 PM) kshitij8: Yes.. I've been giving it some thought I'll get back with some doubts on the same 
(01:07:53 PM) jjohansen: kshitij8: how has your progress been on dealing with permission sets? (sorry I haven't looked at the latest code or reviews)
(01:08:37 PM) jjohansen: kshitij8: so for a first pass I wouldn't bother with writing to local/*
(01:09:06 PM) jjohansen: but I certainly think when that is supported it should be a config option
(01:09:13 PM) kshitij8: Well it seems to be in working shape atm. I ptobably will modify it a bit down the line, but that risks breaking other code so not now
(01:09:54 PM) cboltz: jjohansen: maybe, but the code should be written with writing to local/ in mind (so that we don't need a full rewrite later)
(01:10:09 PM) jjohansen: yes definitely
(01:10:12 PM) kshitij8: A commandline switch for local/ feels right to me
(01:10:23 PM) cboltz: I have a feeling that the most important part of writing is having a working two-way diff
(01:10:30 PM) cboltz: so that we know what needs to be added
(01:10:50 PM) cboltz: and what can be removed (because it's obsoleted by another, new rule)
(01:11:15 PM) jjohansen: well I tend to like having a config option that can set an expected default and a command line switch that can override but one thing at a time
(01:12:04 PM) kshitij8: Yes i have some ideas on processing the old files for writing lets see how far i get with them
(01:12:06 PM) sarnold: I'd rather the tool keeps track of which profiles were created on the machine, and which profiles were modified from upstream sources; if upstream, use local/, if a locally created profile in the first place, then don't use local/ (I think it'd be silly for /etc/apparmor.d/usr.bin.foo to contain exactly one line, #include <local/usr.bin.foo> ...)
(01:13:15 PM) cboltz: sarnold: good point - we can probably solve this with a "#modifyme" comment in /etc/apparmor.d/usr.bin.foo (which genprof can add)
(01:13:24 PM) jjohansen: sarnold: the way to keep track of what is locally created is to have policy install into a different location, and be able to reference and compare
(01:13:27 PM) kshitij8: The profiles will need a marker comment to specify origin then?
(01:13:56 PM) sarnold: jjohansen: I love that idea.
(01:14:10 PM) jjohansen: the problem with marker comments is that it only catches what the tool does, not manual edits
(01:14:24 PM) jjohansen: not that marker comments are bad
(01:14:52 PM) kshitij8: And one fine day user will delete/mess that marker ;)
(01:15:39 PM) jdstrand: one could imagine system policy could be installed in /usr with a local/ #include. then only site policy could be in /etc. of course, local/ is handy but doesn't solve everything and you would then need another way to override system policy
(01:15:42 PM) cboltz: kshitij8: that's called "shooting yourself in the foot" - and I doubt there is a solution to avoid someone does it ;-)
(01:16:17 PM) kshitij8: Different locations for profiles? I think I've seen a discussion on that.
(01:16:37 PM) jdstrand: that would be easy enough though-- just put site policy and local overrides in /etc. the boot scripts prefer /etc over /usr
(01:17:17 PM) jdstrand: I'm not keen on actually performing this migration personally though-- just tossing the idea out there ;)
(01:17:43 PM) cboltz: jdstrand: does ubuntu mount /usr early enough?
(01:17:53 PM) jdstrand: heck, you could even have symlinks in /etc to /usr so it looks the same
(01:18:00 PM) cboltz: (I remember discussions to not move cache/ to /var because it's mounted too late)
(01:18:25 PM) jdstrand: well, it doesn't *have* to be /usr
(01:18:30 PM) jjohansen: hrmmm my preference is similar, install to /usr, and then copy to /etc/ if it doesn't exist in /etc/ or the version in /etc/ is the old version
(01:18:39 PM) jjohansen: this keeps all policy in one place
(01:18:55 PM) jdstrand: by the time we load this kind of policy, /var and /usr should be mounted, yes
(01:19:06 PM) jjohansen: jdstrand: nope
(01:19:27 PM) jjohansen: jdstrand: we have a split of early and later policy, and early is in before /usr
(01:19:53 PM) jdstrand: jjohansen: that's what I meant be "this kind of policy"
(01:20:03 PM) jjohansen: oh and that is the other benefit of keeping policy in /etc/ we don't have the late mnt problem
(01:20:26 PM) jdstrand: I am keenly aware of how troubling the apparmor policy load during boot is :)
(01:20:29 PM) jjohansen: jdstrand: but then you have some system policy in /etc/ and other in /usr/
(01:20:37 PM) ***cboltz doesn't really care about late mount - systemd lives on /usr/ nowadays
(01:20:56 PM) jjohansen: our early policy is in the same /etc/ dir, its just marked different
(01:21:20 PM) jjohansen: cboltz: some of us would like to avoid changes that only works with systemd
(01:21:35 PM) cboltz: I understand that ;-)
(01:21:46 PM) jdstrand: note, I was merely tossing an idea out there-- not a proposal :P
(01:21:50 PM) cboltz: it's just that I can be sure that /usr/ is mounted _very_ early on openSUSE ;-)
(01:22:34 PM) jdstrand: well, /usr and /var will be very early on ubuntu too. its just that dhclient can come up before then
(01:23:02 PM) jjohansen: right, and if you would like to distro patch for suse that is fine, the upstream solution for now has to take late mounting of /usr into account
(01:23:35 PM) jdstrand: there are certainly other things, especially if you are trying to do a more comprehensive system confinement rather than targetting certain services
(01:23:48 PM) jjohansen: yes
(01:24:32 PM) jdstrand: so, was this actually on the agenda? I mean, it is an interesting topic, but I doubt anyone will be implementing this right away (unless I missed something :)
(01:24:58 PM) jdstrand: it also seems kinda-distro specific
(01:25:07 PM) jjohansen: hehe no it wasn't on the agenda, we took a tangent from kshitij8 topic
(01:25:10 PM) cboltz: does anybody care about the agenda?
(01:25:14 PM) ***cboltz doesn't ;-)
(01:25:30 PM) jjohansen: not really, its a guideline
(01:26:02 PM) ***kshitij8 doesn't mind discussions
(01:26:12 PM) jjohansen: I think jdstrand point is that if its not something we are going to do right away, then we can delay the discussion, so we can get back to working on all the things we are way behind on
(01:26:54 PM) ***jdstrand doesn't mind discussions-- it's just that I know this is a bike-sheddable moment and thought maybe that would be better served on the mailing list. I'm but one person though...
(01:28:33 PM) kshitij8: So for the alpha release my approach for write tool is basically read old profile if correct rule write it else remove it when the end brace is reached dump everything
(01:29:20 PM) kshitij8: Subject to change due to implementation details ;)
(01:30:08 PM) cboltz: as a first version (to get a working logprof fast) this sounds good
(01:30:50 PM) kshitij8: Meanwhile i can figure out how local/ fits in
(01:30:51 PM) jjohansen: yep, we can discuss profile locations later, I'll even send an email to the list for everyone to ignore
(01:31:25 PM) jjohansen: kshitij8: yep, writing to local/ is orthogonal to the whole /etc/ /usr/ discussion
(01:31:28 PM) jdstrand: heh
(01:31:40 PM) ***kshitij8 reads all ml mails O:) 
(01:31:48 PM) cboltz: kshitij8: for local/, just think of the local/ file as the end brace
(01:31:56 PM) cboltz: in other words: dump everything to local/ ;-)
(01:32:32 PM) kshitij8: In that case the existing profile is in local/ ?
(01:32:57 PM) kshitij8: Or we are just dumping a locally modified profile to local/
(01:33:37 PM) cboltz: yes, the locally modified profile (to be exact: the additions) go to local/
(01:34:51 PM) kshitij8: local/ also has some profiles though
(01:35:38 PM) kshitij8: So what becomes input for writer? The local/ profile or the one in apparmor.d
(01:36:10 PM) cboltz: local doesn't have full profiles, just included files
(01:36:29 PM) cboltz: the profiles in /etc/apparmor.d/ have a #include <local/$profilename>
(01:37:58 PM) kshitij8: Hmm.. The tangent discussion seems to have confused me :p
(01:38:35 PM) cboltz: ;-)
(01:38:52 PM) kshitij8: Anyways im back in sync. Dump to local. Leave original profile as it is?
(01:39:18 PM) cboltz: we'll have a config option for that later ;-)
(01:39:42 PM) cboltz: but it might be a good start for you because it makes it easier to see the profile differences
(01:39:45 PM) kshitij8: Shouldnt be very tricky :)
(01:40:23 PM) cboltz: if you (for now) always write to local/ please also add some comments saying
(01:40:27 PM) cboltz: # lines to remove:
(01:40:37 PM) cboltz: # /home/cb/foo/bar/xy r,
(01:40:38 PM) kshitij8: Using local/ makes seeing difference easy?
(01:40:52 PM) cboltz: depends on the POV ;-)
(01:41:10 PM) cboltz: but for logprof I'd say yes
(01:41:29 PM) kshitij8: That was my concern about removed rules. 
(01:42:35 PM) kshitij8: How does local/ deal with that? I guess we remove in main profile and additions go to local/
(01:42:58 PM) cboltz: no and yes ;-)
(01:43:06 PM) cboltz: do not change anything in the main profile
(01:43:16 PM) cboltz: additions go to local/
(01:43:30 PM) kshitij8: Schrödinger's answer cboltz :p
(01:44:12 PM) kshitij8: But Removed allow or deny rules? 
(01:45:04 PM) cboltz: if logprof removes a rule, it means there is a rule that allows more
(01:45:29 PM) cboltz: deleting the now superfluous rule is a nice cleanup
(01:45:45 PM) cboltz: but it doesn't change anything permission-wise if it is still there
(01:46:55 PM) kshitij8: Okay. Btw we had a point to ask user to produce a clean profile or modify existing profile. We want that?
(01:47:24 PM) cboltz: that should be
(01:47:27 PM) cboltz: a) a config option
(01:47:31 PM) cboltz: b) a commandline parameter
(01:47:51 PM) cboltz: and there are more options ;-)
(01:48:09 PM) kshitij8: C) choice for individual profiles
(01:48:14 PM) cboltz: besides "clean profile" and "modify existing profile", there's also "write to local/"
(01:49:05 PM) kshitij8: And some never ask me again stuff too..
(01:49:13 PM) cboltz: (hmm, or should "write to $whatever" be separated to allow a cleanup in local/?)
(01:49:38 PM) cboltz: users will tell you where a "never ask again" makes sense *g*
(01:49:55 PM) kshitij8: :D
(01:50:19 PM) jjohansen: alright so if there anything else to discuss around GSoC
(01:50:53 PM) kshitij8: Not on my end atm.
(01:50:53 PM) kshitij8: :)
(01:51:15 PM) cboltz: maybe a small thing
(01:51:39 PM) cboltz: kshitij8: IIRC you asked for the remaining parts of older reviews
(01:52:04 PM) cboltz: are you interested in them currently? (I could just attach them to a mail ;-)
(01:52:21 PM) cboltz: or do you prefer to focus on profile writing first?
(01:52:57 PM) kshitij8: Btw I've fallen a bit behind my target schedule. Should get back on track soon.
(01:53:22 PM) kshitij8: I'd prefer a mail in present situation.
(01:53:52 PM) kshitij8: Or we can have it on irc someother day.
(01:54:02 PM) cboltz: ok, I'll send it later
(01:54:22 PM) cboltz: if you think some details need discussion, just ping me on irc whenever you see me ;-)
(01:54:45 PM) kshitij8: And writing is the primary goal atm. It opens doors to bug reports
(01:55:14 PM) kshitij8: Sure :)
(01:55:31 PM) cboltz: BTW: can you add a commandline parameter to set a different profile dir?
(01:55:40 PM) cboltz: I'd prefer not to mess up my system profiles while testing ;-)
(01:55:54 PM) kshitij8: Shouldnt be hard i believe
(01:56:29 PM) kshitij8: I'll let try it out tomorrow.
(01:57:03 PM) kshitij8: You can always abort on seeing the final diff ;)
(01:57:24 PM) cboltz: ;-)
(01:58:30 PM) cboltz: and that's all what I have for GSoC today ;-)
(01:58:41 PM) cboltz: jjohansen: feel free to continue with the next topic
(01:59:48 PM) jjohansen: alright, Intrigeri wanted us to discuss synching and maintenance of apparmor policy between ubuntu and debian
(02:00:17 PM) jjohansen: but he is at debconf and doesn't seem to have made it to the meeting, so we are going to postpone the discussion
(02:00:25 PM) jdstrand: that works
(02:00:50 PM) jdstrand: it would be nice to resuce that delta
(02:00:51 PM) sbeattie: fair enough
(02:00:57 PM) jdstrand: reduce even
(02:01:19 PM) jjohansen: basically he wants to get together with /me jdstrand, maybe kees and anyone else who is interested and come up with a way to improve what is happening with policy
(02:01:22 PM) jjohansen: yep
(02:01:34 PM) jdstrand: yeah, that's cool
(02:02:11 PM) jjohansen: it doesn't even have to be in the next meeting, we will organize something sometime
(02:02:20 PM) jjohansen: moving on 2.8.2 release
(02:02:21 PM) cboltz: sounds interesting - maybe openSUSE could also benefit?
(02:02:31 PM) jjohansen: cboltz: yes likely
(02:02:47 PM) jjohansen: you can join the discussion as well
sbeattie sbeattie_ 
(02:03:00 PM) jjohansen: sarnold, sbeattie: 2.8.2 status?
(02:03:21 PM) mdeslaur left the room (quit: Quit: *** stack smashing detected ***).
(02:04:05 PM) sbeattie: sorry, black hat and my vacation got in the way
sbeattie sbeattie_ 
(02:04:20 PM) mdeslaur [~mdeslaur@166.62.243.58] entered the room.
(02:04:20 PM) sbeattie: Am hoping to prod sarnold through it this week.
(02:04:53 PM) sbeattie: (I think sarnold stepped away for a bit)
(02:05:01 PM) jjohansen: sbeattie: are we still looking at doing a quick turn around to a 2.8.3 after 2.8.2 or just incorporating those things into 2.8.2
(02:06:02 PM) sbeattie: jjohansen: I haven't decided, so far we had the one alias patch that we wanted for 2.8.3 IIRC
(02:06:16 PM) sbeattie: I don't think there was anything else yet.
(02:06:29 PM) sbeattie: but I'm behind on the list a bit, so might be wrong about that.
(02:06:57 PM) jjohansen: did we finish the log review for 2.8.2, I had that down as finishing under 2.8.3
(02:07:06 PM) sbeattie: no, not yet.
(02:07:34 PM) sbeattie: So I guess I'm still leaning towards a 2.8.2 with a not too distant 2.8.3
(02:08:10 PM) sbeattie: Is that reasonable?
(02:08:20 PM) jjohansen: okay, and btw intrigeri is reporting some bugs in the alias patch so that sounds very reasonable to me
(02:08:28 PM) sbeattie: And sorry, this should have gone out a while ago.
(02:08:55 PM) jjohansen: heh it happens, /me is about to apologize for the state of 3.0 :)
(02:08:56 PM) sbeattie: Oh, good to know. Am I missing these because launchpad hates me or are they reported elsewhere?
(02:09:10 PM) ***cboltz prefers to have 2.8.2 soon to get rid of backported patches in the opensuse package
(02:09:12 PM) jjohansen: not in launchpad private mail
(02:09:18 PM) sbeattie: okay, cool.
(02:09:27 PM) sbeattie: cboltz: yeah, understood.
(02:09:33 PM) sbeattie: cboltz: sorry.
(02:09:45 PM) jjohansen: alright, moving onto 3.0
(02:10:12 PM) jjohansen: I am working on getting the next alpha kernel together, it will be up today/tomorrow sometime
(02:10:12 PM) cboltz: one last question for 2.8.2 - can you give me a release date? ;-)
(02:11:18 PM) sbeattie: cboltz: how about 8/23 at the latest, possibly earlier?
(02:11:41 PM) cboltz: possibly earlier sounds good *g,d&r*
(02:12:50 PM) jjohansen: with that said, 3.0 is way behind, and I have started slashing features
(02:13:51 PM) jjohansen: default profile - will not ship as a supported feature, I am just going to leave in the current implementation and we will not support it, its experimental and you can play with it if you want to but it is not the final version and will change
(02:14:17 PM) jjohansen: the alias patch - will likely not make it unless I have time to fix it before feature freeze
(02:14:40 PM) jjohansen: the extended perms beyond what is absolutely required will likely be cut
(02:15:00 PM) jjohansen: there will be no profile instantiations, so no per user profiles, etc
(02:15:15 PM) jjohansen: extended delegation will not make it
(02:15:51 PM) jjohansen: we will retain the delegation of fds from unconfined.  There is one potential feature around delegation to discuss
(02:16:17 PM) jjohansen: whether we add a basic rule to allow a profile to state whether it can receive delegation
(02:16:47 PM) jjohansen: this would allow profiles to lock themselves down and be authorative
(02:17:00 PM) jjohansen: I think its a feature we want
(02:17:11 PM) jdstrand: I do too, it is when
(02:17:48 PM) jdstrand: I feel like that is a nice to have rather than a need to have
(02:18:08 PM) jdstrand: but I'll admit I haven't thought about it a lot
(02:18:25 PM) jdstrand: tyhicks: you've been thinking about fd passing-- thoughts?
(02:18:25 PM) jjohansen: hrmm, I would like it as then it wouldn't require a version bump to add the other delegation bits
(02:18:40 PM) jjohansen: but it is not required
(02:18:49 PM) jjohansen: we can skip it if needed
(02:18:53 PM) tyhicks: hmm
(02:19:07 PM) tyhicks: I don't think it should be a blocker for 3.0
(02:19:15 PM) tyhicks: it is something AA should have
(02:19:35 PM) tyhicks: but it would potentially be holding up features that AA needs to have right now
(02:19:40 PM) jjohansen: I have been working on a larger delegation email, and we can discuss it more in there. I just haven't made getting that out a priority, but it will happen this week
(02:19:52 PM) tyhicks: I think it can be lumped into the future delegation work and we'll still be fine
(02:20:42 PM) jjohansen: everyone fine with further discussion for this happening on ml
(02:20:45 PM) jdstrand: I tend to agree. we could say that because it does help us avoid a version bump, if we happen to have time to get it in, we can, but it shouldn't block
(02:21:01 PM) jdstrand: +1
(02:21:11 PM) tyhicks: that's a good approach, jdstrand 
(02:21:12 PM) jjohansen: alright moving on
(02:21:17 PM) tyhicks: and I'm fine w/ taking this to the ml
(02:21:28 PM) jjohansen: any initscript rework is being cut
(02:22:09 PM) jdstrand: what rework was that?
(02:22:23 PM) jdstrand: well, let me ask a simpler question
(02:22:37 PM) jjohansen: while it would be nice to fix the initscripts to use the newer parser features like having the parser update the cache, use a single parser invocation to load policy etc, that is all just nice to have
(02:22:47 PM) jdstrand: I updated ubuntu's initscript to also use /var/lib/apparmor/profiles and /var/cache/apparmor
(02:23:16 PM) jdstrand: ok, so parser improvements that affect the initscript, not the stuff I poked at
(02:23:31 PM) jjohansen: jdstrand: just basic cleanups like calling the parser once with the profile dir to load, ...
(02:23:45 PM) jjohansen: jdstrand: right, also if anything is done it can go in
(02:23:49 PM) jdstrand: yeah-- that's cool but can wait
(02:23:54 PM) jjohansen: its just the bits that haven't landed
(02:24:08 PM) jjohansen: tyhicks, jdstrand: where do stand on versioning
(02:24:21 PM) jjohansen: its going to come in late for 3.0
(02:24:26 PM) tyhicks: jjohansen: no progress at the moment
(02:24:28 PM) jdstrand: well, we discussed it from ubuntu's perspective
(02:24:55 PM) jdstrand: we are going to minimize v3 rules in /etc for now
(02:24:56 PM) jjohansen: its not required for ubuntu but I think for upstream could be considered required
(02:25:30 PM) jdstrand: I agree
(02:25:46 PM) jjohansen: I think we are looking at ubuntu taking a "beta" version of 3.0, with some 3.0 features landing after
(02:26:00 PM) jdstrand: it would be nice to have in the september timeframe though-- we could maybe get it into 13.10
(02:26:09 PM) jjohansen: sure
(02:27:04 PM) tyhicks: am I currently holding up the versioning work?
(02:27:14 PM) jjohansen: okay so we are going to play it by ear for now, the upstream version of 3.0 may be delayed a few weeks
(02:27:18 PM) jdstrand: jjohansen: yeah-- it wouldn't be the first time. in Ubuntu, we can largely dodge the v3 issue by just not doing them in system policy and reserving those for our app store policy (which is versioned)
(02:27:22 PM) jjohansen: tyhicks: no, I am
(02:27:25 PM) tyhicks: ok
(02:27:38 PM) jdstrand: and then pull in v3 upstream versioning when it makes sense
(02:28:21 PM) jjohansen: tyhicks: well sort of, I'd rather you wait to work on it until I kick out my latest iteration of parser patches, and you have the dbus stuff taken care of
(02:28:22 PM) tyhicks: jjohansen: I'd say it would be safe to put off the delegation email in favor of the versioning email
(02:28:39 PM) jjohansen: we need another versioning email?
(02:29:13 PM) tyhicks: maybe not
(02:29:21 PM) tyhicks: I guess the parser patches were what I was talking about
(02:29:47 PM) jjohansen: ah okay
(02:30:05 PM) jjohansen: that is all I have for 3.0, does anyone have anything else
(02:31:53 PM) ***jdstrand does not
(02:31:56 PM) jjohansen: I'll take that as a no
(02:32:00 PM) jjohansen: okay onto next meeting, does the regular 2nd tuesday schedule work for, thats sept 10, @20:00 UTC
(02:32:41 PM) tyhicks: I'll be available
(02:32:43 PM) sbeattie: jjohansen: should be okay for me
(02:32:50 PM) jdstrand: works for me
sbeattie sbeattie_ 
(02:33:13 PM) cboltz: sounds good
(02:33:34 PM) ***kshitij8 surprisingly made it through the meeting :)
(02:33:37 PM) jjohansen: sbeattie: does that mean your out of town ;)
(02:33:37 PM) jjohansen: alright meeting adjourned, thanks everyone
(02:33:56 PM) kshitij8: Looks good thanks jjohansen :)
(02:34:01 PM) tyhicks: thanks
``` 
