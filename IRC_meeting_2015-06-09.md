```
(01:00:52 PM) jjohansen: sarnold, sbeattie, jdstrand, tyhicks, mdeslaur, cboltz: meeting time
(01:00:55 PM) tyhicks: hello
(01:01:01 PM) sarnold: \o
(01:01:17 PM) cboltz: hello
(01:01:17 PM) mdeslaur: hi jjohansen
(01:02:26 PM) jjohansen: alright lets get started, the meeting agenda such as it is http://wiki.apparmor.net/index.php/MeetingAgenda#Next_Meeting:_IRC_Meeting.2C_20:00_UTC.2C_May_09_2015
(01:02:49 PM) jjohansen: First up 2.10
(01:03:06 PM) jjohansen: we have a few outstanding patches
(01:03:27 PM) darix: hmm
(01:03:29 PM) jjohansen: I have to rev my vars in profile name patch
(01:03:30 PM) darix: minor nitpick
(01:03:32 PM) darix: it is june
(01:03:47 PM) jjohansen: darix: haha, so it is :)
(01:03:51 PM) jjohansen: and welcome
(01:04:09 PM) jjohansen: there fixed
(01:04:16 PM) jjohansen: http://wiki.apparmor.net/index.php/MeetingAgenda#Next_Meeting:_IRC_Meeting.2C_20:00_UTC.2C_June_09_2015
(01:04:24 PM) darix: danke
(01:04:48 PM) jjohansen: cboltz: do you have patches to rev, or are they all in review state?
(01:05:22 PM) cboltz: just those I already sent (number 44..48)
(01:05:30 PM) jjohansen: oh I suppose I also have a cboltz patch to update still
(01:05:40 PM) jjohansen: cboltz: ack
(01:05:52 PM) jjohansen: and tyhicks, has 1 patch to rev
(01:06:09 PM) tyhicks: looking at that now
(01:06:12 PM) jjohansen: so I think that is a backlog we can get through this week
(01:06:14 PM) cboltz: I also have several TODO notes, but nothing that _must_ go into 2.10
(01:06:29 PM) jjohansen: tyhicks: ack, thanks
(01:07:28 PM) jjohansen: so currently tyhick's policy load api is the hard requirement for 2.10
(01:07:55 PM) jjohansen: another subject came up this week, which is finally fixing caching to use a hash
(01:07:55 PM) darix: where is that explained? (for people trying to catch up)
(01:08:20 PM) jjohansen: darix: hrmmm, I think that would be in the last couple of meeting logs
(01:08:26 PM) cboltz: darix: short version:
(01:08:30 PM) jjohansen: http://wiki.apparmor.net/index.php/Irclog.12.05.2015
(01:08:41 PM) cboltz: - current code checks for "cache file is never than profile + used abstractions"
(01:09:06 PM) cboltz: (which can break when installing a package that was built before the cache file got created
(01:09:23 PM) cboltz: - planned code: store a hash of all involved files in the cache file
(01:09:34 PM) jjohansen: oh, heh I interpreted darix question as the hard requ for 2.10 release. The policy cache api is so we can start building systemd support off of it
(01:09:53 PM) darix: i referred to the policy api thing yes
(01:09:56 PM) jjohansen: the cache discussion is a bit, here in back scroll and on the mailing list
(01:10:26 PM) micahg left the room (quit: Ping timeout: 480 seconds).
(01:10:43 PM) jjohansen: darix: short of it, we want to have native cache loading in systemd, we need an api to do it, and we don't want to wait for the next release
(01:10:52 PM) tyhicks: darix: I pulled the code out of the parser that performed policy cache loads and placed it into libapparmor
(01:11:02 PM) darix: ic
(01:11:12 PM) darix: thanks
(01:11:52 PM) jjohansen: tyhicks: do you think you will have time to get a revision out this week? Our would you like me to take a stab at it
(01:12:59 PM) MoC [~u@0SGAABA1I.tor-irc.dnsbl.oftc.net] entered the room.
(01:13:16 PM) jjohansen: while waiting back to the other issue, which is caching
(01:13:32 PM) tyhicks: jjohansen: I think I can do it this week
(01:13:38 PM) jjohansen: thanks
(01:14:06 PM) jjohansen: so there are cases where the cache mtime timestamp check can fail, people are hitting this in the wild
(01:14:30 PM) jjohansen: I have pushed out a patch that narrows the window, but doesn't completely fix the issue
(01:14:50 PM) jjohansen: the only way to do that is by supporting a hash check
(01:15:09 PM) jjohansen: the question is whether 2.10 should wait for this
(01:15:57 PM) jjohansen: I futzed with hashing the cache yesterday, and came up with something that is a transparent change
(01:16:10 PM) jjohansen: it won't affect the load api
(01:16:46 PM) sarnold: what would you thuink about holding off on it until we've gotten the multiple cache support written?
(01:16:53 PM) jjohansen: we could ship the patch to the lib without actually supporting hashing of policy in 2.10
(01:17:08 PM) ***jdstrand is here now
(01:17:16 PM) jjohansen: sarnold: well that is part of the question
(01:17:43 PM) jjohansen: how critical is supporting this, from an Ubuntu, Debian, Suse pov
sbeattie sbeattie_ 
(01:18:06 PM) tyhicks: if it doesn't affect the API, lets get 2.10 out the door since there's already a ton of changes waiting to be released
(01:18:10 PM) jjohansen: sbeattie: took a look and thinks this isn't a blocker for ubuntu atm
(01:18:20 PM) cboltz: I let %post drop the cache, so I have a workaround in the package
(01:18:30 PM) jdstrand: if snappy is ok with the mtime fix, then I think we can afford to wait
(01:18:40 PM) cboltz: (there are some packages which install their own profile, which don't do that and might be hit)
(01:18:41 PM) jjohansen: tyhicks: what I have doesn't atm, sorry I should have posted it out already
(01:18:59 PM) jdstrand: since that is the only thing I know of that was hitting it before (which I found surprising considering the patch)
(01:18:59 PM) jjohansen: jdstrand: that is partly the question, is it really fine with it?
(01:19:08 PM) jjohansen: jdstrand: nope
(01:19:26 PM) darix: cboltz: apparmor macros might help. just saying ;)
(01:19:36 PM) jdstrand: I just mean I hadn't seen bugs until that one
(01:19:36 PM) darix: %apparmor_reload
(01:19:41 PM) darix: could then drop caches
(01:19:56 PM) jjohansen: local changes can cause it, anything that computes/creates new local files and drops them into an included dir
(01:20:01 PM) jdstrand: jjohansen: let me see what they say about it
(01:20:08 PM) jjohansen: so say it is possible for lxc or libvirt
(01:20:21 PM) jdstrand: hmm, mvo is gone. guess we have to wait
(01:20:58 PM) cboltz: darix: agreed, but I couldn't decide about which macros would be useful/wanted/needed yet (we should discuss this later ;-)
(01:21:00 PM) jjohansen: cboltz: I know you have been asking for it, has this been an issue for you?
(01:21:34 PM) cboltz: as I said, I have a workaround in %post now - I just delete the cache on package updates
(01:21:38 PM) cboltz: not nice, but works
(01:21:45 PM) cboltz: (and yes, there was a bugreport about it)
(01:22:00 PM) jjohansen: ack, thanks
(01:22:11 PM) jjohansen: so suse is working around it as well
(01:22:45 PM) jjohansen: my inclination is unless someone screams this is a hard requirement, to not delay 2.10 for it
(01:22:59 PM) sbeattie: that's my inclination as well
(01:22:59 PM) tyhicks: I agree considering that workarounds are already in place
(01:23:49 PM) tyhicks: I expect that the hashing patch set will be simple enough for a distro patch, if needed before an upstream release can be cut
(01:24:20 PM) jjohansen: cboltz: that doesn't mean it won't land soon, I will kick out a first cut based on yesterdays futzing
(01:24:47 PM) cboltz: no need to worry - the workaround will work for some more weeks ;-)
(01:25:09 PM) jjohansen: ok, anyone have anything else for 2.10?
(01:25:17 PM) tyhicks: not from me
(01:25:32 PM) sbeattie: nope
(01:25:40 PM) jjohansen: alright 2.11 planning
(01:26:07 PM) jjohansen: so I think the window we are looking at is somewhere between October (4-4.5 months) or January (6-7 months) 
(01:26:26 PM) jjohansen: October is a short time frame to get something together for
(01:26:51 PM) tyhicks: what features are we planning?
(01:26:56 PM) jjohansen: especially if we are trying to hit the ubuntu feature freeze of Aug 22
(01:27:23 PM) jjohansen: tyhicks: well that will partly depend on the time we have
(01:27:42 PM) tyhicks: multiple cache dirs and in-file policy cache hashes seem doable
(01:28:12 PM) jjohansen: the things I have looming large, are stacking and ns issues, overlayfs, multiple-caches and policy hashing
(01:28:31 PM) sarnold: that seem slike a lot to bite off
(01:28:47 PM) jjohansen: sarnold: no kidding
(01:29:13 PM) sarnold: multiple caches and policy hashing is the only one of those that feels reasonable for an october timeframe
(01:29:19 PM) jjohansen: anything else on peoples radar? Or that they would really like to see in 2.11
(01:29:42 PM) sarnold: _one_ of the others for a january timeframe. maybe.
(01:30:00 PM) tyhicks: I think we could do multiple caches and policy hashing in the next couple months
(01:30:16 PM) tyhicks: and then stacking, ns, and overlayfs by january
(01:30:21 PM) jjohansen: darix, cboltz: do you have a better estimate of the next suse release than 2015?
(01:30:50 PM) jjohansen: tyhicks: I'm fine with that
(01:30:55 PM) darix: this is currently in open discussion
(01:31:22 PM) cboltz: not really, and with the option to base it on SLE sources, things got even more interesting ;-)
(01:31:36 PM) jjohansen: atm I am inclined to shoot for a smaller 2.11 release in the fall
(01:31:54 PM) jjohansen: cboltz: oh fun :)
(01:32:47 PM) jdstrand: so, stacking/ns is a priority for april
(01:33:13 PM) jjohansen: yes, and previewing it in october would be good
(01:33:37 PM) jjohansen: but I don't think its possible to get all the pieces in place by then
(01:33:47 PM) jdstrand: ideally jj would focus on that and any other stuff would be picked up by others, imho
(01:33:52 PM) jjohansen: there are userspace updates needed, in addition to the kernel
(01:33:54 PM) tyhicks: agreed
(01:34:05 PM) jdstrand: obviously, it would be collaborative
(01:34:23 PM) jdstrand: but I think jj is the only one that has a handle on the stacking/ns issues
(01:34:54 PM) tyhicks: in fact, it is probably best if jj gets his policy hashing patch out to the list and someone else runs with it
(01:36:00 PM) jjohansen: sure, I plan to kick that stuff out today
(01:36:37 PM) jjohansen: cboltz: did you have anything to add to 2.11?
(01:36:57 PM) jjohansen: err the 2.11 timeframe/feature discussion
(01:37:19 PM) cboltz: hopefully more rule classes in the tools ;-) - but that shouldn't be too surprising
(01:37:54 PM) cboltz: (any volunteers for FileRule?)
(01:38:54 PM) jjohansen: hehe, no thanks I already have the parser refactor for a file rule class on my docket
(01:39:48 PM) tyhicks: it would be good to know a bit more detail around what cboltz is planning for 2.11
(01:40:05 PM) tyhicks: cboltz: do you plan to work on a FileRule class? any other classes?
(01:41:10 PM) cboltz: we definitively need a FileRule class - files rules are the most complex ones (at least as long as dbus etc. have "just don't break them" implementations ;-)
(01:41:27 PM) cboltz: and we really want to be able to test that code in a sane way
(01:41:40 PM) cboltz: (so not the 500 line function we currently have ;-)
(01:42:22 PM) tyhicks: that would be nice
(01:42:30 PM) tyhicks: cboltz: anything else significant on your todo list?
(01:42:35 PM) cboltz: I won't promise a timeframe - the "most complex" part also means that writing that class isn't as easy as, let's say, CapabilityRule ;-)
(01:43:09 PM) cboltz: FileRule is probably the biggest thing
(01:43:44 PM) cboltz: but if you are bored, I can offer you some other stuff from my TODO list ;-)
(01:44:01 PM) jjohansen: cboltz: well I see the work as cleanup, as long as it isn't breaking things and features don't depend on it, so timeline isn't such an issue
(01:44:02 PM) darix: maybe put it into the bugtracker?
(01:44:19 PM) cboltz: jjohansen: agreed
(01:44:50 PM) tyhicks: sounds good
(01:44:59 PM) cboltz: darix: I'm not sure if I want to spam the bugtracker with every detail ;-)
(01:47:34 PM) jjohansen: anything else to add around 2.11
(01:47:37 PM) jjohansen: ?
(01:50:14 PM) jjohansen: okay, does anyone have anything else they would like to raise today?
(01:50:40 PM) cboltz: what about another 2.9 release?
(01:51:11 PM) cboltz: the 2.9 branch got quite some patches since 2.9.2 (with some more pending)
(01:51:47 PM) jjohansen: so yes we can do another 2.9, what time line where you looking for?
(01:52:22 PM) cboltz: as soon as the pending patches are in ;-)
(01:52:51 PM) sbeattie: cboltz: right, I need to look at the patches you nominated.
(01:52:52 PM) cboltz: (see the 2.9 candidates list on the ML)
(01:53:09 PM) sbeattie: I was thinking shortly after 2.10 goes out
(01:53:39 PM) cboltz: sounds good :-)
(01:53:58 PM) jjohansen: yeah, I would prefer getting 2.10 out first as well
(01:54:31 PM) cboltz: BTW: if you notice a patch that should go to 2.9, but isn't in my list, just ask (or simply add it) ;-)
(01:56:17 PM) jjohansen: cboltz: did you want the mtime patch for 2.9?
(01:56:35 PM) sarnold: I think it should probably go into 2.9
(01:56:46 PM) cboltz: if it applies easily, why not
(01:57:01 PM) cboltz: otherwise the workaround will continue to work ;-)
(01:57:22 PM) jjohansen: ack thanks
(01:57:28 PM) cboltz: (to be on the save side, I'll probably keep it until we have hashes)
(01:57:56 PM) jjohansen: makes sense
(01:59:10 PM) jjohansen: alright the next meeting is tentatively scheduled for July 12, speakup here or on the mailing list if that is going to be an issue
(01:59:59 PM) cboltz: Juli 12 is a sunday - is this intentional?
(02:00:16 PM) sbeattie: July 14, I think was meant
(02:00:17 PM) sarnold: heh
(02:00:25 PM) jjohansen: hah, no July 14
(02:00:37 PM) cboltz: ;-)
(02:00:38 PM) jjohansen: I'm not sure why I have it down as the 12
(02:01:10 PM) darix: let's just take july 4th
(02:01:19 PM) darix: i am sure no american needs to be in the office that day :p
(02:02:27 PM) cboltz: BTW: Did any of you notice that my secret plan behind the towel day patch series worked?
(02:02:39 PM) cboltz: https://launchpad.net/apparmor/+topcontributors  :-P
(02:02:56 PM) sarnold: heh nice :)
(02:03:35 PM) jjohansen: congratulations cboltz, you are hereby rewarded with more work
(02:04:04 PM) sarnold: a very teutonic reward :)
(02:04:19 PM) jjohansen: meeting adjourned
(02:04:19 PM) jjohansen: thanks everyone
(02:04:20 PM) tyhicks: what's that saying, karma is a ?
(02:04:29 PM) sarnold: thanks jjohansen, everyone :)
(02:04:33 PM) tyhicks: :)
(02:04:35 PM) tyhicks: thanks jjohansen 
(02:04:37 PM) cboltz: jjohansen: I'm not sure if sbeattie would survive another patch series like on towel day ;-)
``` 
