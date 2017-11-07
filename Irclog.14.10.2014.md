```
(01:01:10 PM) jjohansen: sbeattie, tyhicks, sarnold, mdeslaur_, jdstrand, cboltz, kees, h01ger: if you are interested meeting time
(01:01:22 PM) tyhicks: hi
(01:01:35 PM) ***cboltz is still here
(01:03:05 PM) ***sbeattie waves hello
(01:03:08 PM) jjohansen: alright lets get started
(01:03:33 PM) jjohansen: tyhicks: any status to report on dbus upstreaming efforts?
(01:04:51 PM) tyhicks: jjohansen: I did get another review from upstream a 2 or 3 weeks ago
(01:04:59 PM) tyhicks: jjohansen: there's a lot to respond to and I haven't had a chance yet
(01:05:08 PM) tyhicks: jjohansen: will be able to do that sometime this week
(01:05:26 PM) tyhicks: I've skimmed through most of it and don't see any deal breakers
(01:06:18 PM) jjohansen: tyhicks: great thanks for the report
(01:06:38 PM) jjohansen: okay, moving on
(01:07:09 PM) jjohansen: 2.8.4 has been released, thanks to sbeattie for doing all the hard work to finally get it out the door
(01:07:58 PM) sbeattie: thanks, but it was a group effort.
(01:08:43 PM) sbeattie: I don't expect we'll release a 2.8.5, but I didn't expect to need to release 2.8.4, either.
(01:09:31 PM) jjohansen: I don't expect a 2.8.5 either but if there are some major bugs, well ...
(01:09:44 PM) jjohansen: which brings us to our next topic
(01:10:02 PM) jjohansen: 2.9 is in final beta/release candidate
(01:10:48 PM) jjohansen: now its just a matter of either deciding when to cut the release or hitting some show stopping bugs before then
sbeattie sbeattie_ 
(01:11:34 PM) jjohansen: sbeattie, cboltz: do either of you have a timeline of when you would like to see 2.9 release
(01:12:18 PM) sbeattie: jjohansen: I'm wanting to release 2.9.0 final this week
(01:12:43 PM) cboltz: I'd like to get the simplified usage of aa-mergeprof in
(01:13:02 PM) cboltz: (too bad Kshitij isn't here to provide the status)
(01:13:27 PM) jjohansen: cboltz: how much time do you think that will take?
(01:14:08 PM) cboltz: good question ;-)
(01:14:41 PM) jjohansen: I wouldn't be opposed to having 2.9.1 being a quick two week or some such update to 2.9, if that helps get 2.9 out the door
(01:14:49 PM) jjohansen: 2.8.x has lived far too long
(01:15:09 PM) cboltz: Kshitij sent a v1 patch a month ago (which looks quite good, but not final), but didn't send an update since then
(01:15:22 PM) ***sbeattie also was wondering if we wanted to add aa-sandbox to the set of tools that get installed, but jdstrand is not here to update status on that.
(01:15:32 PM) cboltz: jjohansen: does the version number really matter?
(01:15:51 PM) cboltz: I'd prefer not to change the commandline of aa-mergeprof after the final release
(01:16:11 PM) cboltz: so I'd propose to call this week's release beta again
(01:16:21 PM) jjohansen: cboltz: yes and no, its about what is getting developed on, which branch of the tree is current etc, there are people unwilling to take 2.9 until it is released
(01:17:03 PM) cboltz: hmm, we could commit Kshitij's v1 patch quickly
(01:17:07 PM) jjohansen: cboltz: understandable
(01:17:21 PM) cboltz: if it breaks something, we could call the result a bugfix and then release 2.9.1 ;-)
(01:18:21 PM) jjohansen: cboltz: yes we could, though I would rather not release if it, if it completely breaks the tool
(01:18:48 PM) jjohansen: if it introduced some smaller bugs, well that isn't so bad
(01:19:26 PM) cboltz: it should only introduce smaller bugs, if any ;-)
sbeattie sbeattie_ 
(01:19:42 PM) jjohansen: cboltz: okay
sbeattie sbeattie_ 
(01:20:14 PM) jjohansen: sbeattie: how do you feel about running with v1 and releasing a 2.9.1 in a couple weeks to a month if needed
(01:20:18 PM) sbeattie: okay, so let's go ahead and commit it, so that's out of the way as a blocker, and we can refine from there.
(01:20:26 PM) sbeattie: jjohansen: I think that's fine.
(01:20:44 PM) jjohansen: alright cboltz, please commit asap
(01:21:14 PM) cboltz: ok, I'll give the patch some more testing and if I don't notice a serious bug, I'll commit it
(01:21:26 PM) ***cboltz has to merge some profiles anyway
(01:21:42 PM) jjohansen: I think that covers 2.9, lets move on to the python tools
(01:22:32 PM) jjohansen: cboltz: I so what do you have in mind for python tools, improvement, maintenance and cleanup?
(01:22:57 PM) cboltz: that's a topic Kshitij proposed ;-)
(01:23:06 PM) cboltz: but let me answer from my POV
(01:23:26 PM) cboltz: we still have some not-so-critical bugs which should be fixed
(01:24:10 PM) cboltz: besides that, I started to change regexes to named matches (easier and less error-prone than match[7] ;-)
(01:24:51 PM) cboltz: I'm sure I'll notice some things to improve over the time, but don't have a list I could present now
(01:25:30 PM) cboltz: there's also the idea to use classes instead of nested arrays (aka "hasher")
(01:25:50 PM) jjohansen: yes please
(01:25:58 PM) cboltz: I like that as a long-term goal, but wouldn't call it a priority
(01:26:00 PM) ***sbeattie would like to see the codebase refactored into more pythonistic/OOP code, with fewer gigantic functions; instead with smaller, more easily unit-testible code blocks, and much wider automated test coverage.
(01:26:21 PM) cboltz: (if someone wants to do it _now_ - just do it. I won't stop you)
(01:26:38 PM) cboltz: yes, I'd also like to get rid of functions with >100 lines ;-)
(01:27:39 PM) sbeattie: so say we all.
(01:28:00 PM) cboltz: and I'm already collecting testcases for some of the bugs I fixed recently
(01:28:50 PM) cboltz: (where "collecting testcases" means I store the reproducer in a ToDo directory. Actually writing the test is also on the ToDo list - any volunteers? ;-)
(01:29:50 PM) sbeattie: cboltz: I can't make it a high priority, but I can certainly help with writing the test.
(01:29:54 PM) sbeattie: err, tests.
(01:30:18 PM) cboltz: when you have some time, ping me and I'll send you the content of my tests-to-write directory
(01:31:44 PM) jjohansen: alright, without kshitij here I think we can move on
(01:31:54 PM) jjohansen: does anyone have anything else to discuss
(01:35:45 PM) jjohansen: alright I will take the silence as a no, on to the next meeting should it be Nov 4 (first tues), Nov 18 (third) or should we just skip the Novemeber meeting (yes I skipped Nov 11 as an option)
(01:36:28 PM) ***cboltz doesn't like the "skip" option
(01:37:21 PM) tyhicks: either one should work for me
(01:37:36 PM) jjohansen: another option would be move it to wed Nov 12
(01:38:01 PM) ***sbeattie is fine with any of those options.
(01:38:05 PM) jjohansen: though switching days instead of weeks may be harder for some
(01:38:59 PM) cboltz: I'd vote for Nov 4 - that still leaves the option to move it to one of the later dates ;-)
(01:39:16 PM) jjohansen: I was thinking the 4th as well
(01:40:00 PM) tyhicks: lets do it
(01:40:05 PM) jjohansen: okay, done
(01:40:12 PM) sbeattie: oh, the ubuntu online summit is the 12th-14th. Either that's a conflict or we have our meeting as part of it; /me votes for the former.
(01:40:32 PM) sbeattie: 4th works for me.
(01:40:46 PM) jjohansen: ack
(01:40:54 PM) jjohansen: thanks everyone meeting adjurned
``` 
