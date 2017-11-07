```
(01:00:08 PM) jjohansen: jdstrand, sbeattie, sarnold, tyhicks, mdeslaur, cboltz: if you are interested it time for the apparmor irc meeting
(01:00:22 PM) sarnold: danke ;)
(01:00:28 PM) sarnold: (here, but divided attention)
(01:00:41 PM) ***tyhicks keeps an eye on the channel
(01:00:46 PM) sbeattie: heya jjohansen 
(01:01:03 PM) ***cboltz is here - and wonders why sarnold speaks german
(01:01:23 PM) sarnold: cboltz: long story, but I had hoped to move to nuernberg back when I worked for SuSE :)
(01:01:26 PM) jjohansen: cboltz: because sarnold is just weird :)
(01:01:35 PM) mdeslaur: hiya
(01:02:21 PM) jjohansen: oh oops left out kees, kees we are going to roll with the apparmor meeting if you can drop in
(01:02:52 PM) jjohansen: Alright the meeting agenda is at http://wiki.apparmor.net/index.php/MeetingAgenda
(01:03:33 PM) jjohansen: its just a rough guideline so feel free to add items at the end
(01:04:15 PM) jjohansen: We haven't had a meeting in a long while, so the first question I would like to resolve is how often should we have them
(01:04:44 PM) jjohansen: every week was to much, and we just didn't have the material to justify it
(01:05:35 PM) cboltz: I'd propose monthly meetings
(01:05:38 PM) mdeslaur: once a month seems more reasonable
(01:05:39 PM) jjohansen: I am leaning toward having a meeting every month or two months
(01:05:40 PM) jdstrand: jjohansen: I vote monthly
(01:05:46 PM) jdstrand: (to start)
(01:05:58 PM) jjohansen: sounds good to /me we can always modify from there
(01:05:59 PM) jdstrand: we can adjust as necessary
(01:06:13 PM) jjohansen: does tuesday this time work for everyone?
(01:06:25 PM) mdeslaur: sure
(01:06:26 PM) jdstrand: that was probably the easiest item on the agenda :)
(01:06:28 PM) sbeattie: +1 on monthly
(01:06:35 PM) jdstrand: +1 tues
(01:06:43 PM) sarnold: +1 for right now :)
(01:06:53 PM) jjohansen: okay tentatively scheduling next one for Oct 25
(01:07:16 PM) jjohansen: gah my calendar foo failed, make that Oct 23
(01:07:52 PM) jjohansen: so the week before UDS for those attending
(01:08:08 PM) sbeattie: sure, that works
(01:08:57 PM) jjohansen: kees: if your here do you want to discuss autotools
(01:09:00 PM) cboltz: I'll be on the way back from the openSUSE conference on Oct 23 (without internet access)
(01:09:23 PM) cboltz: but well, you'll never find a date that works for everybody ;-)
(01:09:41 PM) jjohansen: cboltz: hrmmm, when would work for you? the 30th will be bad for the rest of us
(01:09:56 PM) cboltz: a week earlier should work
(01:10:07 PM) cboltz: Oct 16
(01:10:27 PM) ***sbeattie can also do thursdays at this time
(01:10:30 PM) jjohansen: cboltz: yeah or the 16th, the week after UDS
(01:10:49 PM) ***jdstrand votes for week after UDS instead of week earlier
(01:10:49 PM) sbeattie: err, the 6th you mean?
(01:11:08 PM) jjohansen: bleah yeah
(01:11:17 PM) ***cboltz thinks about sending a calendar to jjohansen
(01:11:49 PM) jjohansen: hehe, /me keeps looking at his calendar widget but has to reset the month every time
(01:12:29 PM) jjohansen: any preference on the meeting being pre or post UDS
(01:12:36 PM) jdstrand: post
(01:12:51 PM) sarnold: either
(01:12:59 PM) jdstrand: I don't think we'll have a lot for pre since it is less than a month away
(01:13:05 PM) sbeattie: no pref here
(01:13:15 PM) jjohansen: alright lets shoot for Nov 6th, that way we can summarize what comes out of UDS etc
(01:14:20 PM) jjohansen: moving on, it seems kees isn't here so I will skip autotools unless someone else wants to jump in and discuss it
(01:14:28 PM) cboltz: speaking about UDS - will there be apparmor-related sessions?
(01:14:51 PM) jjohansen: cboltz: yes, but there hasn't been any planning of them yet
(01:15:40 PM) cboltz: can you send a mail when the planning is done? Preferably earlier than some hours before the session begins ;-)
(01:15:45 PM) jjohansen: more information will hit the list as we have more details
(01:15:56 PM) jjohansen: cboltz: yes, sorry about that
(01:16:47 PM) jjohansen: alright I will take it, no body wants to talk autotools so we will move on
(01:17:18 PM) jjohansen: hrmm, I am going to move testing infrastructure down to release planning, so next up documentation
(01:17:32 PM) jjohansen: Uh its in really bad shape as always
(01:18:00 PM) ***cboltz remembers a, well, slightly outdated ;-) techdoc
(01:18:11 PM) jdstrand: that is definitely out of date
(01:18:11 PM) jjohansen: it would be nice if we could just do a little wiki gardening
(01:18:35 PM) jjohansen: yeah the tech doc is very out of date, but I don't want to update it yet
(01:18:41 PM) ***jdstrand nods
(01:19:00 PM) jjohansen: I would like that to get updated with stuff from the next release
(01:19:22 PM) jdstrand: jjohansen: so, what pruning do you envision? I only ever seem to look at http://wiki.apparmor.net/index.php/Documentation which should not be horribly bad
(01:19:28 PM) jjohansen: I have a lot of newer documentation "written" that can be the base for updating it
(01:19:55 PM) cboltz: what about pointing to the openSUSE documentation?
(01:19:56 PM) jdstrand: I can't speak to 'In Depth' there
(01:20:03 PM) cboltz: http://doc.opensuse.org/documentation/html/openSUSE/opensuse-security/part.apparmor.html
(01:20:16 PM) jdstrand: but the howtos aren't bad
(01:20:26 PM) cboltz: should be nearly up-to-date
(01:20:29 PM) sarnold: one potential problem with the documentation is that there is a wide variety of released features and limitations; is the idea of the wiki's documentation to document Current Upstream, or try to hit something good for most users?
(01:20:59 PM) cboltz: (I gave a special workshop for a SUSE documentation author at LinuxTag ;-)
(01:21:08 PM) jjohansen: cboltz: yes I have been meaning to add that to http://wiki.apparmor.net/index.php/Distro_suse
(01:21:33 PM) jjohansen: not that it couldn't be pointed to else where as well
(01:21:43 PM) cboltz: I'd even add it to the /Documentation page
(01:21:54 PM) jjohansen: sarnold: it would be nice to cover both, and just note which version if its needed
(01:22:03 PM) jjohansen: there isn't a lot of difference there
(01:22:06 PM) cboltz: if you skip the YaST-related sections, the openSUSE documentation works for all distributions 
(01:22:11 PM) jjohansen: yep
(01:23:29 PM) jjohansen: jdstrand: heh you know I am not sure, I suppose the core policy reference manual is the thing that most needs work
(01:23:55 PM) jjohansen: I just find every time I am answering people on #apparmor and point them at the wiki something is out of date
(01:23:56 PM) ***jdstrand just now updated http://wiki.apparmor.net/index.php/Distro_suse to point to http://doc.opensuse.org/documentation/html/openSUSE/opensuse-security/part.apparmor.html
(01:24:10 PM) jjohansen: I realize /me is at fault
(01:24:27 PM) jdstrand: jjohansen: no, you cannot blame yourself for everything :)
(01:24:36 PM) jdstrand: all of us could update the docs
(01:24:38 PM) sbeattie: jjohansen: no more than everyone else
(01:24:40 PM) jjohansen: Basically atm, if you see something take a moment to fix it, I'll try to do the same
(01:24:43 PM) sarnold: jjohansen: perhaps it'd help if Future Directions were clearly split from The Way It Is (Or Was) :)
(01:24:50 PM) ***jdstrand will do that
(01:24:57 PM) jjohansen: sarnold: sure that works too
(01:25:14 PM) ***mdeslaur thinks the quality of AA documentation matches the quality of all other linux documentation
(01:25:50 PM) jjohansen: anyway I don't have anything specific I just wanted to raise it as an issue, to put it in peoples mind
(01:25:52 PM) cboltz: mdeslaur: please don't mention THAT problem ;-)
(01:26:04 PM) mdeslaur: hehe
(01:26:22 PM) jjohansen: ugh, that bad. Its clearly worse than I thought
(01:26:46 PM) jjohansen: Alright moving on top the next release planning
(01:27:43 PM) jjohansen: I was planning for our next release to be next spring and have some alphas and betas
(01:28:09 PM) jjohansen: with a schedule something like
(01:28:11 PM) jjohansen: Alpha 1: week on Oct 24.
(01:28:11 PM) jjohansen: Alpha 2: late november
(01:28:11 PM) jjohansen: Alpha 3/beta 1: late december
(01:28:11 PM) jjohansen: beta2/beta3: in mid january
(01:28:11 PM) jjohansen: beta3/4 start of Feb
(01:28:11 PM) jjohansen: release late Feb or late march.
(01:28:56 PM) jjohansen: sigh didn't get that right either but that is the basic idea
(01:29:16 PM) sbeattie: hrm, is an alpha this month even useful for a release 6 months from now?
(01:29:38 PM) jjohansen: I would like to shoot for the release to be late Feb, but give slop room to push back to march if needed
(01:29:58 PM) sbeattie: that sounds good
(01:30:14 PM) jjohansen: sbeattie: yeah I think so, this is looking like the biggest change set we have ever pushed
(01:30:37 PM) jjohansen: I think having some points along the way is worth having
(01:31:01 PM) jjohansen: I want alpha1 pre UDS so we can have dbus and stacking for people to start playing with
(01:31:19 PM) jjohansen: even if they are just prototype stage
(01:31:21 PM) sbeattie: ah, I see.
(01:31:27 PM) sbeattie: okay. 
(01:31:28 PM) sarnold: that feels ambitious to me
(01:31:37 PM) jjohansen: why yes it is
(01:31:51 PM) sarnold: is the old dbus work "close enough" that it isn't? .. is the old dbus work even worth re-opening?
(01:32:09 PM) jjohansen: however the dbus prototype has been up in the past, and stacking is working its way to actually not blowing up in your face
(01:32:28 PM) jjohansen: sarnold: yes we are reopening it and continuing with it
(01:32:57 PM) jjohansen: just having it up on current apparmor would be sufficient for UDS
(01:33:18 PM) jjohansen: so not as ambitious as it may at first seem
(01:33:20 PM) sarnold: at least in the eyes of others; I'm sure others have more dbus experience than i :)
(01:33:47 PM) jjohansen: well the whole point is to get people playing with it and get feed back on the design
(01:34:12 PM) jjohansen: other teams want to use it, and the earlier they can start the more feedback we get into it
(01:34:25 PM) jjohansen: I expect actual dev on it will continue the full cycle
(01:35:44 PM) jjohansen: So anyway dbus and stacking are just 2 of the features, anyone want to speak to any of the others: profile templating, sandbox, aafs updates, ...
(01:36:09 PM) jjohansen: jdstrand: are we shooting for templating and sandboxing to be tech previews for this next release?
(01:36:44 PM) jdstrand: well, templating already works, we just need the templates
(01:36:49 PM) sarnold: :)
(01:37:13 PM) jdstrand: app sandboxing we are striving for tech previews for dbus, env filtering and X
(01:37:44 PM) jdstrand: we'd like to have that in 13.04, so that sounds like the next apparmor release, yeah
(01:37:46 PM) jjohansen: jdstrand: I am more than happy to have it be something more than a tech preview, just want to get a feel for what we are shooting for, for each component
(01:38:19 PM) jdstrand: (since the release schedule you provided is roughly around the time of Ubuntu's feature freeze)
(01:38:44 PM) jdstrand: prototype < tech preview < production quality
(01:38:44 PM) jjohansen: jdstrand: right, so do you count templating as something that will be more than tech preview, even if we only have a couple of templates
(01:39:32 PM) jdstrand: jjohansen: I'm not really counting the templating in all of this-- IME it will just be a few files and profiling updates
(01:39:53 PM) jjohansen: okay
(01:40:02 PM) jdstrand: we will probably add some things certainly, but I imagine the polishing work that is slated for later will be a lot of this sort of thing
(01:40:59 PM) jjohansen: so the next big concern for the release is I don't see how we will be able to get the tools updated.  I am currently just shooting for the core (parser, lib, kernel) and dbus and X extensions
(01:41:32 PM) jjohansen: this means that we will further break genprof/logprof and maybe some of the other tools
(01:41:52 PM) mdeslaur: features are more important than the tools right now
(01:42:06 PM) jjohansen: I know this is bad, but for the next release I want to declare them deprecated
(01:42:15 PM) mdeslaur: jjohansen: the tools?
(01:42:19 PM) sarnold: it'd be ideal if we could at the least get the tools to not molest the rules; how feasible does that sound?
(01:42:30 PM) jjohansen: mdeslaur: or at least a subset of them
(01:43:05 PM) jjohansen: sarnold: well we have done that for several rules in the past, but I just don't even see that much happening
(01:43:27 PM) sbeattie: and there've been bugs in how well that worked.
(01:43:31 PM) jjohansen: we have an extremely ambitious schedule
(01:44:02 PM) jdstrand: so
(01:44:07 PM) jdstrand: we have the aging tools
(01:44:10 PM) jjohansen: I don't propose removing them, people can still use them but we treat them as deprecated
(01:44:13 PM) jdstrand: we also want a python rewrite
(01:44:15 PM) sarnold: jjohansen: it's clear that none of us know them as well as jesse ever did, agreed on that point.
(01:44:30 PM) mdeslaur: well, once we add new features, we can ask for help to get people working on the tools
(01:44:34 PM) sarnold: .. and they've grown well beyond perl's usual grasp.
(01:44:37 PM) jjohansen: hrmmm, I got to know genprof pretty well though I have lost some of it
(01:44:54 PM) jdstrand: it seems like fiddling with the aging tools is wasted effort since we will be doing the python rewrite and address various deficiencies
(01:44:56 PM) jjohansen: yes, their perl is a problem
(01:45:38 PM) jdstrand: I sorta feel like, yes deprecate the exisitng tools, then come back to them
(01:45:42 PM) jdstrand: and do them right
(01:45:55 PM) jjohansen: yeah we work on fixing it in the next release
(01:45:58 PM) ***cboltz thinks interesting[tm] code can be written in any language
(01:46:02 PM) mdeslaur: ok, we should specify that the _perl_ tools are deprecated
(01:46:10 PM) jdstrand: (not that they weren't right at one time, they just aren't now and the bandaids we would add wouldn't particularly help)
(01:46:11 PM) sbeattie: yes, the existing tools have some structural problems, that make it difficult to extend
(01:46:16 PM) ***kees is late, distracted by EFI booting horror
(01:46:36 PM) jjohansen: cboltz: its not pearl that I have a problem with, its specifically that perl that I have a problem with
(01:46:55 PM) cboltz: I know ;-)
(01:47:18 PM) cboltz: I've read some of the code and fully understand why you don't like it ;-)
(01:48:27 PM) jjohansen: Alright so we will deprecate any tools that aren't working, but we should try to keep the simple tools, enforce/complain etc working, and I suppose aa-status too
(01:48:55 PM) cboltz: logprof/genprof should also work
(01:49:12 PM) jjohansen: cboltz: I just don't see a way to achieve it
(01:49:32 PM) cboltz: no need to support newly introduced rules (mount etc) as long as they don't break or remove such rules
(01:49:42 PM) sarnold: cboltz: funny, those are the ones specifically we want to distance ourselves from. (gently)
(01:49:44 PM) jjohansen: I would be delighted if they would, but I know I won't have the time to fix them
(01:50:15 PM) sarnold: cboltz: as sbeattie points out, there are often bugs in the "do no harm" updates we've tried to do to those in the past.
(01:50:29 PM) jjohansen: right but even supporting that kind of thing will be very hard with, the env filtering
(01:50:40 PM) jjohansen: extended x transitions, conditionals, ...
(01:50:40 PM) cboltz: is it that hard to add a "keep any line you don't understand" to logprof?
(01:50:46 PM) jdstrand: jjohansen: I think aa-complain/aa-enforce/aa-disable are all easy to keep. aa-status may not be, but I think a lot of people probably use it, so probably worth the effort (it is also python now)
(01:51:01 PM) cboltz: no need to parse that line somehow - just save it in a "buffer" and re-add it to the profile later
(01:51:17 PM) sarnold: cboltz: perhaps if they are all done as _comments_ or osmething similarly goofy, it might not be too hard
(01:51:23 PM) jdstrand: jjohansen: we could revisit on aa-status in the future if needed
(01:51:29 PM) jjohansen: yes
(01:51:45 PM) cboltz: then just handle env rules as comments internally ;-)
(01:52:06 PM) jjohansen: alright, how is this we will try to make sure genprof/logprof don't break things but its not release critical if we don't get there ...
(01:52:38 PM) jjohansen: other wise we need someone to step up and commit to the work
(01:53:18 PM) jdstrand: seems reasonable
(01:53:51 PM) jdstrand: also, people could wait to integrate the newer apparmor into their environments until the new tools are in place
(01:54:04 PM) jjohansen: yes
(01:54:07 PM) jdstrand: (ie, if they *must* have aa-logprof)
(01:54:26 PM) jdstrand: we want tools, just have to get the other stuff in place first...
(01:54:47 PM) cboltz: you can probably expand that to "if they *must* have aa-logprof _and_ env rules
(01:55:11 PM) cboltz: having one of them shouldn't be a problem, only having both might be interesting[tm]
(01:56:01 PM) jjohansen: cboltz: the problem its not just env rules, its dbus, X, other ipc, ...
(01:56:29 PM) jjohansen: which brings us to the other issue
(01:56:33 PM) jjohansen: version tagging
(01:56:38 PM) cboltz: s/env/newly introduced rules/ then
(01:56:46 PM) jjohansen: sure
(01:57:27 PM) jjohansen: The userspace should be backwards compatible
(01:57:56 PM) jjohansen: Basically we are in the situation shown in the policy version Matrix
(01:58:09 PM) jjohansen: in the release agenda
(01:58:34 PM) jjohansen: new userspace + old kernel and old policy works
(01:59:12 PM) jjohansen: however there are a whole lot of failure cases
(01:59:38 PM) ***mdeslaur has to go...bye!
(01:59:48 PM) jjohansen: bye mdeslaur
(01:59:58 PM) sbeattie: mdeslaur: thanks!
(02:00:01 PM) mdeslaur: I'll read scrollback when I return
(02:00:44 PM) jjohansen: we can either version tag new policy which will basically get the Fail(5) case working or we can just require policy upgrade 
(02:00:49 PM) jdstrand: jjohansen: I don't understand why old parser fails on 'new policy without new rules'
(02:01:31 PM) jjohansen: jdstrand: because there are currently some subtle semantic changes, I don't have a solution for
(02:01:50 PM) jjohansen: not saying it can't be fixed
(02:02:23 PM) cboltz: can you give an example?
(02:02:34 PM) jdstrand: jjohansen: maybe a better question is what is the difference between 'old policy' and 'new policy without new rules'?
(02:02:36 PM) sarnold: at some point, older parsers just won't be able to load policy authored under newer parsers / kernels. we're used to this from other systems, too, mpm-threaded directives don't work on old prefork-only apache..
(02:02:38 PM) jjohansen: there needs to be more work, on it but there can be failures around unix domain sockets using the alternate namespace and netlink rules
(02:03:20 PM) jdstrand: if we are talking about the binary caches, then ok, I get that
(02:03:39 PM) jjohansen: jdstrand: no, the rule results in a slightly different intent
(02:03:40 PM) jjohansen: eg.
(02:03:55 PM) jjohansen:   network,
(02:04:12 PM) jjohansen: hrmmm no that isn't a good example
(02:04:25 PM) jjohansen: example is lets say you lack a network rule
(02:04:36 PM) jdstrand: oh wait, I missed the 'old kernel' vs 'new kernel' line
(02:04:46 PM) jjohansen: unix domain and netlink still get a pass
(02:04:51 PM) ***jdstrand tries to digest the matrix
(02:05:02 PM) jjohansen: they aren't mediated, even though the policy compiles them as being mediated
(02:05:24 PM) jjohansen: so take it as the kernel is NOT applying the intent of the policy
(02:05:52 PM) jdstrand: ok, I get that
(02:06:00 PM) jdstrand: that is mighty subtle
(02:06:10 PM) jdstrand: maybe I am being dense
(02:06:26 PM) jjohansen: yes, and its not likely to affect a lot of applications
(02:06:42 PM) sarnold: jjohansen: if someone really intends on running significantly older userspace tools than the kernel, not everything will work. so long as the kernel doesn't blow up, I just don't see a big problem?
(02:06:57 PM) jjohansen: as that example only applies to unix domain sockets in the alternate namespace and netlink sockets
(02:07:06 PM) jdstrand: I think I feel the same as seth
(02:07:13 PM) sarnold: .. document the new versions in Documentation/Changes once in a while and hope the users read it once in a while? :)
(02:07:35 PM) cboltz: you really expect users to read documentation? ;-)
(02:07:54 PM) sarnold: cboltz: I'm proud of our users :)
(02:08:08 PM) jjohansen: I am not opposed to forcing new policy, a version tag will break old tools just like new policy rules.  What the version tag gains us is breaking old policy when there are subtle semantic differences to worry about
(02:08:22 PM) jdstrand: iiuc, someone writes policy for a new kernel and userspace. then takes that policy to an older parser and kernel and it behaves differently
(02:08:22 PM) sarnold: cboltz: failing that, they can always get their linux from a distribution vendor who updates userspace tools to match kernel tools periodically. :D
(02:08:25 PM) jjohansen: and allowing the newer tools to work with old policy unchanged
(02:08:40 PM) jdstrand: s/writes policy/writes and tests policy/
(02:08:59 PM) sarnold: jjohansen: _that_ is slightly more compelling to me.
(02:09:56 PM) jjohansen: I am indifferent, but I don't deal with the policy end of things much. I know cboltz has expressed a desire for the version tag
(02:10:46 PM) cboltz: yes, I would already have introduced it when directories had to be marked as /path__/__
(02:10:48 PM) sarnold: I'm not opposed to a policy version tag -- it's something SELinux has done for ages and lets them say "this parser works with policy versions 17-21", and it's nice and explicit that way. But it feels like it'd add another hyper-column, "with policy tags" and "without policy tags", and I'm afraid _that_ matrix would overwhelm all of us.
(02:10:56 PM) cboltz: (and that change broke some profiles for sure)
(02:11:18 PM) jdstrand: so, as an integrator of apparmor for a distro, honestly, Fail(5) is ok by me. usually a change in apparmor userspace happens to coincide with an update of the applicate being profiled, so policy tends to change anyway
(02:11:23 PM) sarnold: cboltz: would you have fixed those policies to use trailing / by now? :)
(02:11:37 PM) jdstrand: s/applicate/application/
(02:11:51 PM) jjohansen: jdstrand: you fix your profiles, but what of user profiles?
(02:12:05 PM) jdstrand: that is just part of upgrading your OS
(02:12:34 PM) jjohansen: well yes, profiles do tend to break on upgrade, regardless
(02:12:53 PM) jdstrand: ie, even if we handle Fail(5), /usr/bin/foo most often needs other policy updates unrelated to the apparmor failure we are trying to address
(02:13:38 PM) jdstrand: my point is not to say that we shouldn't do it-- just that I don't think it is super important. I'd probably abstain from voting on this
(02:13:41 PM) jjohansen: yes and mixing updated includes and old policy makes me shudder
(02:13:57 PM) cboltz: ideally we would have an aa-upgrade tool that updates the profiles to the latest syntax version
(02:14:08 PM) cboltz: (but I also know this won't be too easy to write)
(02:14:18 PM) sarnold: cboltz: having written two of those already, it wasn't much fun
(02:14:26 PM) sarnold: .. and those were _easier_ transitions, by far :)
(02:14:45 PM) jjohansen: hrmm, well actually I do think this transition is fairly easy
(02:15:00 PM) ***jdstrand clarifies to say that he doesn't feel fixing Fail(5) is super-important. there might be other reasons to have the version
(02:15:50 PM) jjohansen: so any votes for?
(02:16:09 PM) jjohansen: that is votes for an explicit version tag?
(02:16:19 PM) jdstrand: (+|-) 0
(02:16:32 PM) cboltz: I'd like to have the version tag
(02:17:17 PM) sarnold: abstain; I don't think it buys us enough to justify the complexity, but when one of our best users is asking for it, maybe I'm missing something. :)
(02:17:21 PM) cboltz: (even if the parser and tools ignore it for now - see it as some type of comment/documentation)
(02:18:16 PM) jjohansen: any votes against?
(02:18:43 PM) jdstrand: jjohansen: I noticed you did not vote
(02:18:45 PM) sbeattie: I also don't have strong feelings one way or the other.
(02:19:15 PM) jjohansen: hehe no, like I said I am ambivalent
(02:19:52 PM) jjohansen: okay, we have a single +1, so I will consider it a low priority item that we will try to have
(02:20:04 PM) jjohansen: if nothing else we can tag profiles with a comment
(02:20:16 PM) jjohansen: for documentation purposes
(02:20:39 PM) jdstrand: jjohansen: well, what does a commentless profile indicate?
(02:21:12 PM) cboltz: jdstrand: that's the problem ;-)
(02:21:15 PM) jdstrand: yes
(02:21:20 PM) jjohansen: hehe
(02:21:24 PM) cboltz: I'd do something like
(02:21:36 PM) jdstrand: cause people won't know to comment and possibly degrade to a lower policy
(02:21:38 PM) cboltz:   /bin/foo (version=2) {
(02:21:50 PM) cboltz: even if the parser just ignores it for now
(02:22:25 PM) jjohansen: right, I wasn't saying we would use it in comments, but at the least we could document the new policy is updated for the new version with a comment
(02:22:39 PM) jdstrand: I guess the parser could complain if there is no comment and report the highest version it supports
(02:23:02 PM) jjohansen: cboltz: interesting, so you would accept back versioning to keep old profiles working vs require versioning going forward?
(02:23:25 PM) jdstrand: well, comment or now, if we take this decision it means that apparmor profiles, distro profiles and user profiles will need to change to take advantage of the new policy, no?
(02:23:28 PM) jjohansen: ie. policy without a tag is accepted as current, and if you want old behavior add the tag
(02:23:34 PM) jdstrand: s/or now/or not/
(02:23:54 PM) jjohansen: jdstrand: not necessarily
(02:24:10 PM) jdstrand: jjohansen: if that is the case, then that seems reasonable. I was thinking defaulting to earliest version
(02:24:10 PM) jjohansen: any use of new rules would automatically indicate new policy
(02:24:37 PM) cboltz: the most safe solution is probably
(02:24:43 PM) jjohansen: jdstrand: so was I but requiring an older version is an intriguing way to go
(02:24:49 PM) cboltz: - profiles without version are "old"
(02:25:01 PM) cboltz: - only profiles with a version are handled in the new way
(02:25:23 PM) cboltz: - let the parser complain if "old" profiles (without a version) contain new rules
(02:25:29 PM) jjohansen: cboltz: well it would be easy to do a profile migration tool that just adds the old tag
(02:25:58 PM) jdstrand: cboltz: you voted +1. can you explain why you want this?
(02:26:33 PM) cboltz: to avoid that profiles behave different after upgrading apparmor
(02:26:52 PM) cboltz: and allow and deny exactly what they did before
(02:27:36 PM) jjohansen: would not a migration tool provide that?
(02:27:37 PM) cboltz: that's it ;-)
(02:27:53 PM) jjohansen: ie just something that adds the tag
(02:27:54 PM) cboltz: a migration tool would help
(02:28:08 PM) cboltz: but I'm not sure if it can be fully automated
(02:28:10 PM) jdstrand: I admit, it sounds nice, but boy it seems like a lot of complexity is being added
(02:28:49 PM) jdstrand: and it is hard to see the immediate benefit since the profiles should only be getting more strict
(02:29:32 PM) cboltz: a typical user will call this "apparmor broke my application" ;-)
(02:29:35 PM) jdstrand: well, I think jjohansen is right. add it to the todo list, but low priority
(02:29:42 PM) jjohansen: hrmm, about we table it for now. I will poke and revisit when we have a more concrete idea of the cost
(02:30:22 PM) jdstrand: cboltz: yeah, but how often are they upgrading apparmor? I would imagine by *far* are OS upgrades where their application changed outside of apparmor
(02:30:42 PM) jdstrand: (ie, apparmor already broke them)
(02:31:34 PM) jdstrand: table: +1
(02:31:52 PM) cboltz: jdstrand: you might be right ;-)
(02:32:12 PM) cboltz: (but I don't have statistics on it)
(02:33:01 PM) jdstrand: neither do I-- just the ubuntu bugs vs apparmor upstream bugs. anyhoo. I don't disagree it could be valuable
(02:33:53 PM) jjohansen: alright so I was currently planning to bump the major version of apparmor for the next version as it seems there is finally a large feature jump coming and it can also serve as the version tag for the policy version if needed
(02:34:01 PM) jjohansen: any objections?
(02:34:17 PM) cboltz: no
(02:34:23 PM) jdstrand: no
(02:34:31 PM) sbeattie: not from me
(02:34:36 PM) cboltz: (we'll see if AppArmor 3.0 or PostfixAdmin 3.0 comes earlier ;-)
(02:34:46 PM) jdstrand: heh
(02:34:47 PM) jjohansen: hehe
(02:35:47 PM) jjohansen: Okay my last item was the compatibility patches in the kernel, I think with the potential lack of tool changes etc, we need to carry for a couple of more releases
(02:35:53 PM) jjohansen: seem reasonable?
(02:37:06 PM) jjohansen: okay I'll take that as a yes :)
(02:37:39 PM) jjohansen: cboltz you and sarnold added a couple items, care to take 
(02:37:41 PM) jjohansen: where to package extra profiles? (/etc/apparmor/profiles/extras/ isn't the best place)
(02:38:11 PM) cboltz: jjohansen: we invented those items while you were away last week ;-)
(02:38:30 PM) cboltz: for the extra profiles, what about /usr/share/apparmor/ ?
(02:38:44 PM) cboltz: it already exists and contains easyprof sniplets
(02:38:58 PM) jjohansen: cboltz: I know, its punishment for my ISP being incompetent
(02:39:15 PM) jjohansen: :)
(02:39:19 PM) cboltz: and it would be a much better place than /etc/ (which should only contain config files)
(02:40:07 PM) jjohansen: I am okay with that, the current location of /etc/apparmor/extras/ or what ever it is, is just bad
(02:40:14 PM) jdstrand: just so I'm clear, these are profiles/apparmor/profiles/extras/*
(02:40:31 PM) cboltz: exactly
(02:40:55 PM) sbeattie: cboltz: in ubuntu, we ship them in /usr/share/doc/apparmor-profiles/extras/, /usr/share/doc/[package] being the typical place in debian-land of shipping example configurations.
(02:41:37 PM) ***jdstrand nods
(02:41:57 PM) cboltz: opensuse has only /usr/share/doc/apparmor/ (no separate directory for -profiles)
(02:41:57 PM) jjohansen: oh right, /me forgets all the diffs there
(02:42:05 PM) cboltz: but the idea to put them in the doc dir isn't too bad
(02:42:14 PM) jdstrand: /usr/share/apparmor seems reasonable as an upstream
(02:42:40 PM) jdstrand: if they are examples, doc/ tends to make sense. that is how we have thought of them in Ubuntu
(02:42:44 PM) cboltz: yes, but if all distributions ignore the upstream path, it doesn't make much sense ;-)
(02:42:46 PM) sbeattie: so I don't think there's any opposition to moving the default to somewhere under /usr/share/, so long as distros can finetune where they actually land.
(02:43:00 PM) jdstrand: if they are more than that, then /usr/share/apparmor also seems ok
(02:43:13 PM) cboltz: keep in mind that genprof uses them as template
(02:43:22 PM) cboltz: so we have at least to tell it the path ;-)
(02:44:03 PM) ***cboltz hopes that this wasn't an indirect ubuntu bugreport ;-)
(02:44:59 PM) cboltz: hmm, proposal to avoid each distribution has to change the genprof config:
(02:45:04 PM) jdstrand: I was not aware the aa-genprof used them as templates. seems that /usr/share/apparmor is the way to go then
(02:45:17 PM) cboltz: - put the extra profiles in /usr/share/apparmor/extra-profiles/
(02:45:26 PM) cboltz: - add a symlink to it in the doc directory
(02:45:37 PM) jjohansen: jdstrand: yes if it finds them it can propose that a profile already exists do you want to use it as a base
(02:45:45 PM) jdstrand: (ie, they aren't examples any more)
(02:45:55 PM) cboltz: jdstrand: well, it uses them if it can find them ;-)
(02:46:17 PM) jjohansen: cboltz: see ubuntu doesn't have that because it can't find them :)
(02:46:22 PM) sbeattie: hehe
(02:46:26 PM) cboltz: lol
(02:49:00 PM) jjohansen: so cboltz proposal wfm, jdstrand?
(02:49:13 PM) sbeattie: +1 from me
(02:51:44 PM) jdstrand: the proposal is /usr/share/apparmor?
(02:51:51 PM) jdstrand: /usr/share/apparmor/extra-profiles/?
(02:51:59 PM) jjohansen: and symlink docs if desired
(02:52:28 PM) jdstrand: I think they should live in /usr/share/apparmor/extra-profiles/. the symlink from doc is dstro specific?
(02:53:03 PM) sbeattie: yeah, /usr/share/apparmor/extra-profiles/
(02:53:10 PM) cboltz: the docdir is distro-specific, so the symlink is too
(02:53:17 PM) jdstrand: yes, then I agree
(02:53:23 PM) jjohansen: okay next
(02:53:36 PM) jjohansen: do we want a special location for auto-generated sniplets? (for example the samba on openSUSE, which currently lives in local/)
(02:53:47 PM) cboltz: a last word on extra profiles:
(02:54:09 PM) cboltz: I'll add a symlink in /etc/apparmor/profiles/ so people who know the old location can find them
(02:54:33 PM) cboltz: does it make sense to create this symlink in the Makefile or should I do it in the .spec?
(02:54:54 PM) sbeattie: probably the .spec
(02:55:06 PM) jdstrand: the symlink feels weird to me, so I am leaning towards the .spec :P
(02:55:23 PM) jdstrand: of course, I am not handling said migration :)
(02:55:35 PM) cboltz: ;-)
(02:56:30 PM) jjohansen: I think doing it in the spec is fine
(02:56:59 PM) cboltz: ok
(02:57:19 PM) jjohansen: so back to autogenerated snippets? Don't we already do this some with libvirt
(02:57:46 PM) cboltz: on openSUSE we do it for the samba shares
(02:58:04 PM) jjohansen: oh right!
(02:58:40 PM) cboltz: there are two reasons why I brought up this question:
(02:58:49 PM) cboltz: a) there might be name conflicts in local/
(02:59:12 PM) cboltz: b) auto-generated sniplets should be separated from manually modified files
(03:00:04 PM) jdstrand: cboltz: so, openSUSE there is a tool or packaging that generates these sniplets?
(03:00:27 PM) cboltz: it's a small script in the samba package
(03:00:51 PM) sbeattie: jdstrand: does libvirt do anything with local/ ? I know it puts its snippets in /etc/apparmor.d/libvirt/ but does it let the user add anything via local/?
(03:01:02 PM) cboltz: I can ask the samba team to upstream it if you want
(03:01:38 PM) jjohansen: I like upstreaming :)
(03:02:05 PM) cboltz: that means we should decide first if we want a special directory for autogenerated sniplets
(03:02:12 PM) cboltz: and if yes, how to name it
(03:04:04 PM) sbeattie: cboltz: are these snippets that are pulled into a single profile via an include rule, or are these multiple generated profiles?
(03:04:36 PM) cboltz: at the moment it's a single file that is included in the /usr/sbin/smbd profile
(03:04:43 PM) sbeattie: in the libvirt case, we have dynamically generated profiles.
(03:05:40 PM) jdstrand: ok, sorry. phone
(03:05:43 PM) ***jdstrand is back
(03:06:22 PM) cboltz: how/by what are the libvirt profiles loaded?
(03:06:23 PM) jdstrand: so, the way libvirt works is that libvirtd generates a profile dynamically and puts it in /etc/apparmor.d/libvirt
(03:06:37 PM) jdstrand: then it loads it and does a change_profile
(03:06:59 PM) sbeattie: however, for mysql and now dhcp, we have .d/ directories that are included in the respective top level profiles, such that other packages can drop snippets into place to extend the policy to cover the extra functionality introduced by the separate package.
(03:07:03 PM) jdstrand: (actually, libvirtd uses aa-virt-helper from libvirt, but that is neither here nor there)
(03:07:14 PM) jdstrand: there is also apache.d
(03:07:30 PM) jdstrand: apache2.d
(03:07:51 PM) sbeattie: oh apache2.d/, maybe that's what I'm thinking of, I don't think we got around to the mysql.d/ one yet.
(03:08:16 PM) cboltz: sbeattie: the *.d directories sound like a good idea
(03:08:25 PM) jdstrand: I wasn't aware of mysql.d. but yes, dhcp.d (or something) now
(03:08:27 PM) cboltz: but are overkill for the samba case
(03:08:40 PM) jdstrand: so I like these .d style directories in general
(03:09:06 PM) jdstrand: ie, the concept is nice-- admins, distros, etc can just drop stuff in there
(03:09:08 PM) sbeattie: jdstrand: I do, too, I just don't like them at the top level under /etc/apparmor.d/
(03:09:17 PM) jdstrand: but they have traditionally been problematic with the tools
(03:09:59 PM) cboltz: BTW: if a package drops a file in a *.d directory, does it also reload the profile?
(03:10:16 PM) jdstrand: cboltz: in Ubuntu, yes
(03:10:22 PM) sbeattie: cboltz: yes, it's supposed to, as part of the postinstall.
(03:10:27 PM) jdstrand: but it depends on how it is being added
(03:10:57 PM) jdstrand: eg, for dhcp.d, then it needs to somehow be reloaded in the packaging of the package doing the dropping
(03:11:13 PM) jdstrand: libvirt handles it all itself-- nothing is actually dropped in there by other packages
(03:11:33 PM) cboltz: a pointer to the postinstall script would be nice ;-)
(03:11:35 PM) sbeattie: jdstrand: right, the libvirt usage feels different than these other uses.
(03:11:50 PM) jdstrand: right, I think it is
(03:11:53 PM) cboltz: (some openSUSE packages contain a profile, but AFAIK none of them cares about loading it)
(03:12:03 PM) jdstrand: but, there is no reason why it couldn't live in the same place as the others
(03:12:29 PM) jjohansen: cboltz: I think we need an rpm equiv of dh_apparmor
(03:12:51 PM) sbeattie: jdstrand: do you never see additional packages adding things to the default libvirt policies?
(03:13:33 PM) jjohansen: well if we hit that its not like libvirt couldn't also do a .d dir and include that
(03:13:34 PM) cboltz: jjohansen: a good start would be to show me what dh_apparmor does (as in: a link to its source code) ;-)
(03:14:18 PM) jjohansen: cboltz: right, I'll dig it out but basically its a debian packaging helper to do the right thing when dropping in profiles
(03:14:52 PM) cboltz: it shouldn't be too hard to convert it to a rpm macro
(03:14:55 PM) sbeattie: cboltz: debhelper works a bit differently than rpm packaging, in that it autogenerates snippets of shell code used in the various install phases; likely you'd be adding a new rpm macro or two
(03:15:44 PM) jdstrand: sbeattie: re libvirt policies> no
(03:16:23 PM) jdstrand: libvirt does also ships abstractions/libvirt-qemu
(03:16:40 PM) jdstrand: and is also template based
(03:16:50 PM) jdstrand: /etc/apparmor.d/libvirt/TEMPLATE
(03:17:06 PM) jdstrand: so people have lots of places to fiddle with it
(03:17:06 PM) sbeattie: anyway, *I*'d like to see an /etc/apparmor.d/snippets/ directory that contains subdirectories for individual applications, though a better name would be appreciated.
(03:17:24 PM) sbeattie: I'm ambivalent as to whether libvirt dynamic profiles should live there.
(03:17:54 PM) cboltz: does /etc/apparmor.d/autogenerated/ sound too boring?
(03:18:21 PM) jdstrand: I would like them to live somewhere else too. there have been a couple of times that we have had to update some tool to ignore some .d dir
(03:18:33 PM) jdstrand: cboltz: I don't think they are always autogenerated
(03:18:35 PM) jjohansen: yeah
(03:19:10 PM) cboltz: jdstrand: one of my goals is to have the autogenerated sniplets (or profiles) in a separate directory
(03:19:13 PM) sbeattie: right, in many instances, they're not autogenerated
(03:19:30 PM) cboltz: so that admins know they should not change them and/or the change will be overwritten
(03:19:40 PM) jdstrand: hmm
(03:20:02 PM) jdstrand: those should live outside of /etc, no?
(03:20:31 PM) cboltz: if you read the FHS very strict, they should go to /var/
(03:20:40 PM) sarnold: jdstrand: .. but they may reflect site-local configuration made in /etc/ elsewhere -- smbd or ntpd or whatever
(03:20:51 PM) cboltz: but that means we depend on having /var/ mounted when loading the profiles
(03:20:59 PM) cboltz: I don't want to depend on that 
(03:21:05 PM) jdstrand: yeah, it is tricky. they should also be available during early boot so /var is out
(03:21:24 PM) cboltz: (do I need to mention the default _cache_ directory?)
(03:21:53 PM) jdstrand: no you don't. it is definitely a blemish we are painfully aware of :)
(03:22:49 PM) jdstrand: I guess what I am saying is that I like sbeattie's idea. it sounds like cboltz' goal is maybe different. if cboltz' are truly non-admin tweakable, maybe they should be in their own directory
(03:23:14 PM) jdstrand: snippets/autogenerated
(03:23:20 PM) jdstrand: snippets/dhcpd.d
(03:23:24 PM) jdstrand: snippets/foo.d
(03:23:39 PM) jdstrand: snippets/autogenerated/smbd.d
(03:23:53 PM) jdstrand: s/snippets/<some other name>/
(03:23:59 PM) ***sbeattie was hoping someone (e.g. sarnold) would have a better name than snippets. :)
(03:24:11 PM) sarnold: sbeattie: sorry, "autogenerated" as good as I could come up with
(03:24:20 PM) sarnold: sbeattie: besides, I already came up with "chunks". :)
(03:24:48 PM) sbeattie: heh, yeah, "program-chunks" is a mistake we don't need to repeat. :)
(03:24:54 PM) jdstrand: sarnold: ah, so that was you :)
(03:24:58 PM) sarnold: jdstrand: yup!
(03:25:02 PM) ***sarnold takes a bow
(03:25:09 PM) jdstrand: hunks?
(03:25:10 PM) jdstrand: :P
(03:25:12 PM) sarnold: haha
(03:25:19 PM) sarnold: someone has to provide the negative example to today's youth
(03:25:20 PM) ***sbeattie guarantees he's partially to blame for that.
(03:25:34 PM) jjohansen: maybe s/snippets/extensions/
(03:25:39 PM) sarnold: jjohansen: ooh
(03:25:47 PM) jdstrand: that is pretty nice
(03:25:59 PM) sarnold: sbeattie: (it _did_ make sense at the time, right? we werem
(03:26:04 PM) sarnold: t weren't nuts?)
(03:26:20 PM) sarnold: cboltz: does 'extensions' make enough sense to non-native? :)
(03:26:28 PM) jjohansen: sarnold: what when weren't we nuts
(03:26:31 PM) cboltz: I'm afraid extensions might be mis-understood
(03:26:45 PM) jdstrand: fyi, it is *far* from perfect, but for the autogenerated libvirt profiles we have:
(03:26:50 PM) jdstrand: # DO NOT EDIT THIS FILE DIRECTLY. IT IS MANAGED BY LIBVIRT.
(03:26:54 PM) cboltz: typically I think of some *.so or plugin when I hear "extensions"
(03:27:39 PM) cboltz: jdstrand: the samba sniplet has a similar comment (but not uppercase ;-)
(03:27:51 PM) jdstrand: I don't think autogenerated is correct in general. I think the idea of autogenerated file is fine
(03:27:58 PM) jdstrand: cboltz: clearly I was more emphatic
(03:28:09 PM) jdstrand: cboltz: maybe that is all you need to do to solve the bug?
(03:28:18 PM) jdstrand: :)
(03:28:29 PM) cboltz: well, it isn't a real bug
(03:28:32 PM) cboltz: it works at the moment
(03:28:53 PM) cboltz: my idea was to have some type of policy for this and similar cases
(03:29:10 PM) cboltz: and to have it before more profiles contain autogenerated sniplets
(03:29:17 PM) cboltz: (which means changes are harder to do)
(03:29:29 PM) jdstrand: I like the idea of a dedicated directory with .d subdirectories
(03:29:53 PM) ***jdstrand is going to have to go soon
(03:30:35 PM) sbeattie: having the dedicated dir structure makes it easier for future tools to do the right thing (whatever that is) when dealing with them.
(03:30:36 PM) sarnold: jdstrand: indeed, something to contain the contagion makes sense, but we'r already pretty deep, /etc/apparmor.d/piles.d/samba.d/homes
(03:30:56 PM) ***sbeattie also needs to vanish very soon
(03:30:59 PM) jdstrand: I don't mind the depth personally
(03:31:25 PM) jjohansen: its mostly hidden so it not too bad
(03:31:30 PM) sbeattie: ... so long as the hierarchical structure makes sense.
(03:31:35 PM) jdstrand: /etc/apparmor.d/.d/smb.d/h <- that's shorter :P
(03:31:45 PM) sarnold: perhaps I need more shell variables, $EAP, etc.. :)
(03:31:50 PM) jdstrand: with a hidden dir for extra points!
(03:32:03 PM) sbeattie: sarnold: lol
(03:32:06 PM) sarnold: jdstrand: now all it needs is a .d as a file extension :)
(03:32:17 PM) jdstrand: hehe
(03:32:25 PM) cboltz: you'll get more bonus points with a dir named "..something" ;-)
(03:32:50 PM) sarnold: "apps"? "applications"?
(03:33:03 PM) sarnold: (we're hip! we're cool!)
(03:33:27 PM) sarnold: cripes. a plain '.d' is growing on me.
(03:33:40 PM) jjohansen: other potential words, supplements, atoms, particles, fragments, parts, scraps
(03:33:46 PM) jdstrand: I agree with jjohansen-- this is mostly for distros. the depth isn't a big deal. users will tend to the /etc/apparmor.d/profile or local/. if they are cool, they'll dive into /etc/apparmor.d/clutter.d/
(03:33:53 PM) ***jjohansen is having fun with the thesaurus
(03:34:05 PM) jdstrand: stuff.d
(03:34:07 PM) sarnold: bosons! mesons! muons! the possibilities are endless ;)
(03:34:20 PM) ***sbeattie really must go now
(03:34:25 PM) jjohansen: hehe yes
(03:34:42 PM) sarnold: 'atoms' has a ring to it.
(03:34:48 PM) jdstrand: ifyourcoolyoullusethis.d
(03:35:03 PM) sarnold: but i'm afraid their use as interned datastructures in other languages would be nothing but confusing :/
(03:35:25 PM) jdstrand: policy-extensions.d
(03:36:08 PM) sarnold: definitely clear
(03:36:16 PM) jdstrand: jjohansen: your losing your audience. I think there is agreement on a single dir with sub .d dirs
(03:36:20 PM) sarnold: but I'm afraid I'd need a symlink for profile-extensions.d :)
(03:36:22 PM) jdstrand: s/your/you're/
(03:36:40 PM) jdstrand: jjohansen: maybe table the name for next time?
(03:36:45 PM) jjohansen: yes
(03:37:02 PM) jjohansen: thanks everyone, sorry it went so long
(03:37:28 PM) jjohansen: this and remaining items are postponed to the Nov 6 meeting
(03:37:30 PM) jdstrand: thanks jjohansen!
(03:37:32 PM) cboltz: we should just take all name proposals, print them on paper (one per sheet), mix them and blindly take one ;-)
(03:37:46 PM) sarnold: thanks jj :)
(03:37:58 PM) ***jdstrand thinks it is funny that we say we don't have enough to talk about for frequent meetings and then have a mrathon one each time ;)
(03:38:11 PM) jjohansen: hehe yeah
(03:38:21 PM) jjohansen: the agenda really didn't look long
(03:39:09 PM) jdstrand: :)
(03:39:17 PM) cboltz: that's the problem - with a longer agenda, you'll get shorter meetings because everybody hurries up ;-)
(03:40:13 PM) jjohansen: right so I'll stick a long list of names in the agenda for next time :)
(03:42:47 PM) cboltz: ;-)
(03:43:03 PM) cboltz: BTW: the patch for moving the extra profiles is nearly finished
(03:43:18 PM) cboltz: you'll be surprised in how many places this directory name is mentioned...
(03:43:48 PM) ***jjohansen has an inkling
(03:44:31 PM) cboltz: oh, and I found something really outdated in the README for the extra profiles
(03:44:43 PM) cboltz: it still contains the mailinglist @forge.novell.com
(03:45:00 PM) sarnold: i bet the manpages mention the novell bugzilla all over the place too
(03:45:32 PM) cboltz: maybe, but at least bugzilla still exists ;-) 
(03:45:43 PM) sarnold: haha
(03:45:54 PM) cboltz: (and apparmor-related bugs are assigned to me very quickly)
(03:46:05 PM) sarnold: and poor sherry arnold can't figure out why she keeps getting bugs assigned to her...
(03:48:56 PM) xubuntuwwwbbb33 [~xubuntuwb@197.7.34.210] entered the room.
(03:48:57 PM) cboltz: sarnold: is this a real problem?
(03:49:35 PM) cboltz: I'd guess most people @SUSE know that I'm the default victim^Wassignee for apparmor bugs
(03:50:14 PM) sarnold: cboltz: I rather hope not. back then, though, she had to send me a fair amount of mail that was delivered to her (sarnold@novell.com) because that's how people knew me... they didn't expect 'srarnold'.
(03:50:52 PM) cboltz: I can imagine that...
(03:51:39 PM) xubuntuwwwbbb33 left the room (quit: ).
(03:53:44 PM) sarnold: cboltz: this security guide, was that entirely you or did you steal stuff from jana too?
(03:53:53 PM) sarnold: this is surprisingly comprehensive. :)
(03:54:12 PM) sarnold: It reminds me a lot of the SuSE manual that came with my 5.2 box. :)
(03:54:23 PM) cboltz: I did not write it, I just gave a presentation to one of the SUSE doc writers
(03:54:36 PM) cboltz: so that he knows what to write
(03:54:45 PM) sarnold: aha. :)
(03:55:09 PM) sarnold: I liked working with jana, she sometimes asked questions that demonstrated we hadn't thought things through all the way yet. :)
(03:56:06 PM) cboltz: I don't know Jana in person, but nevertheless she'll probably remember my name
(03:56:31 PM) cboltz: http://blog.cboltz.de/archives/13-404-mal-anders.html
(03:56:47 PM) cboltz: have a look at the picture of the printed manual there
(03:57:03 PM) sarnold: with the paperclips? :)
(03:57:12 PM) cboltz: I sent her this picture with the notice "every paperclip is a bug in the shell chapter"
(03:57:17 PM) sarnold: haha
(03:57:21 PM) cboltz: no answer
(03:57:29 PM) cboltz: some days later, I entered the detailed bugreport
(03:57:34 PM) cboltz: and her first comment was
(03:57:44 PM) cboltz:     @bugreporter: please do not ever touch a paperclip again ... :)
(03:57:55 PM) sarnold: :D
(03:58:42 PM) cboltz: BTW: it was Frank Sundermayer who updated the security guide
(03:58:52 PM) sarnold: oh! okay
(03:59:05 PM) sarnold: I never met him, but the name is familiar
(04:01:25 PM) sarnold: cboltz: BBfH?
(04:01:47 PM) cboltz: Bastard Bugreporter from Hell ;-)
(04:01:59 PM) cboltz: I think I can claim this title after > 1000 bugreports
(04:02:28 PM) sarnold: hahaha
(04:04:12 PM) sarnold: heh, nice aa-notify problem
(04:06:05 PM) cboltz: what's wrong with it?
(04:06:13 PM) sarnold: cboltz: thank you so much for including the vi 'db dw de ..', 'cb cw ce ..', 'yb yw ye ..' slide :)
(04:06:24 PM) sarnold: cboltz: I'm going through your 1001 bugs slidedeck
(04:06:29 PM) sarnold: this is awesome ;)
(04:08:08 PM) cboltz: :-)
(04:08:33 PM) cboltz: can you imagine some people's faces when I gave this talk at the openSUSE conference
(04:08:39 PM) cboltz: and mentioned one of their bugs?
(04:09:41 PM) sarnold: some of these are pretty ugly ;)
(04:14:14 PM) cboltz: oh yes, developers sometimes (unintentionally - at least I hope so) come up with very funny ideas ;-)
(04:16:28 PM) sarnold: wow. slide 45. wow.
(04:18:42 PM) cboltz: you shoud have seen Marcus' face ;-)
(04:18:48 PM) sarnold: :)
(04:23:11 PM) cboltz: sarnold: can you tell me what the code in rule #1 does?
(04:23:33 PM) cboltz: (without copying and executing it ;-)
(04:25:31 PM) sarnold: cboltz: well, I _hope_ it prints itself out. :)
(04:25:42 PM) sarnold: cboltz: I've always had a fondness for programs that print themselves.
(04:25:50 PM) cboltz: no, it doesn't
(04:26:02 PM) sarnold: a large, banner-style Viel Spass!
(04:26:03 PM) sarnold: ?
(04:26:12 PM) cboltz: no
(04:26:15 PM) cboltz: much simpler
(04:28:20 PM) sarnold: heh, I've stared at it a while and my eyes hurt :)
(04:28:23 PM) sarnold: what's it do? :)
(04:28:54 PM) cboltz: it prints "I'm a geeko! :-)"
(04:28:58 PM) sarnold: haha
```
