```
(01:03:51 PM) jjohansen: jdstrand, sbeattie, sarnold, tyhicks, mdeslaur, cboltz: meeting time
(01:03:52 PM) jdstrand: o/
(01:04:00 PM) ***sbeattie waves
(01:04:24 PM) cboltz: I'm happy to see kshitij8 is also here :-)
(01:04:37 PM) jjohansen: hey kshitij8
(01:05:08 PM) jjohansen: so lets get started, the meeting agenda such as it is is at http://wiki.apparmor.net/index.php/MeetingAgenda#Next_Meeting:_IRC_Meeting.2C_20:00_UTC.2C_Tuesday_Sep_13_2016
(01:05:38 PM) jjohansen: first up getting 2.11 actually released
(01:06:11 PM) jjohansen: so do we have in everything we need?
(01:06:32 PM) cboltz: I'm still waiting to get the FileRule series reviewed ;-)
(01:06:36 PM) jjohansen: cboltz: what of your monster patchset? were you looking to have that part of 2.11?
(01:06:39 PM) kshitij8: hi
(01:06:48 PM) cboltz: yes ;-)
(01:06:54 PM) jjohansen: cboltz: I know sarnold has started on it
(01:07:20 PM) tyhicks: I'm not working on anything that is required for 2.11 (there were a few things that I wanted to get finished up but they're not going to happen)
(01:07:38 PM) ***jdstrand notices that we talked about 2.11 in Nov of last year! :)
(01:07:42 PM) jjohansen: tyhicks: is that the errno cleanup?
(01:08:02 PM) jjohansen: jdstrand: yes we did. It has taken way to long to release
(01:08:27 PM) jdstrand: well, not criticizing, it isn't like there wasn't some pretty intense stuff going into 2.11
(01:08:28 PM) tyhicks: jjohansen: errno cleanup, I almost finished the multiple policy cache feature a few weekends ago, etc.
(01:08:42 PM) tyhicks: jjohansen: nothing urgent
(01:08:43 PM) jjohansen: tyhicks: oh, nice
(01:08:48 PM) jjohansen: right
(01:09:17 PM) jjohansen: I could push part of the gconf stuff as well but its not really important if the rest of it isn't going to make it
(01:09:21 PM) jjohansen: which it isn't
(01:09:45 PM) tyhicks: yeah, might as well do it all together
(01:10:04 PM) jjohansen: I suppose I would like to push in the updated kernel patches for 4.7 and 4.8 kernels
(01:10:10 PM) jjohansen: so they are part of the release
(01:10:16 PM) jjohansen: but I could do that today
(01:10:42 PM) sbeattie: that would be appreciated.
(01:11:11 PM) tyhicks: would be good to get an ack on this simple autogen.sh patch: https://www.mail-archive.com/apparmor@lists.ubuntu.com/msg08201.html
(01:11:28 PM) jjohansen: okay, I'll look at it after the meeting
(01:11:40 PM) jjohansen: do we have anything else?
(01:12:05 PM) jjohansen: so basically just waiting on cboltz's patchset
(01:12:08 PM) lifeforms: I'd like to raise some attention for mod_apparmor at the end of the meeting ;) (nothing urgent)
(01:12:12 PM) tyhicks: I think the most concerning is the file rule patchset
(01:12:18 PM) tyhicks: I don't think sarnold has the bandwidth to get through all of that
(01:12:24 PM) jjohansen: lifeforms: ack, will do
(01:13:09 PM) cboltz: kshitij8 also started to review the FileRule patchset - kshitij8, what's your progress on this?
(01:13:15 PM) cboltz: (besides the mails you already sent)
(01:13:17 PM) jjohansen: what about having cboltz do an acked-by-timeout, and address any issues raised in the review as patches in 2.11
(01:13:22 PM) jjohansen: err 2.11.1
(01:14:10 PM) kshitij8: cboltz: expect more reviews around weekend. Not getting enough time during weekdays.
(01:14:41 PM) cboltz: jjohansen: it's hard to blame "Mr. Timeout" if some critical bug goes in, so I'd prefer a real review ;-)
(01:14:48 PM) kshitij8: Is there a timeline we're tracking for 2.11.1? (sorry if I missed it)
(01:15:17 PM) jjohansen: cboltz: oh I get that, its getting that real review in a decent time-frame
(01:15:28 PM) jjohansen: 2.11 has been delayed a long time
(01:16:20 PM) cboltz: does that mean I can blame _you_ for any unnoticed bug if I commit as Acked-by <timeout>? ;-)
(01:16:55 PM) kshitij8: so except the FileSet we're good for 2.11.1 this week realistically?
(01:16:56 PM) jjohansen: blame me all you want, but you are still going to have to fix it
(01:16:58 PM) jjohansen: :P
(01:17:03 PM) jdstrand: cboltz: I haven't reviewed the patch-- is there full coverage for the changed code?
(01:17:11 PM) jdstrand: (in the testsuite)
(01:17:19 PM) tyhicks: kshitij8: 2.11, not 2.11.1
(01:17:28 PM) cboltz: there is full test coverage for apparmor/rule/file.py
(01:17:39 PM) cboltz: but the coverage for the aa.py changes is (as usual) incomplete
(01:18:12 PM) jdstrand: cboltz: are that parts of aa.py that call out to file.py covered?
(01:18:22 PM) cboltz: at least the coverage percentage for aa.py gets better, because I dropped quite some code from it ;-)
(01:18:35 PM) tyhicks: kshitij8: also, we'd have to see when sbeattie can absorb doing a release as it takes a considerable amount of time
(01:19:12 PM) cboltz: jdstrand: in general, yes - the aa.py changes are (mostly) related to FileRule
(01:19:23 PM) sarnold: I really uncomfortable putting the FileRUle changes into a 2.11.1 release; they belong in a .0 release
(01:20:09 PM) jdstrand: cboltz: I may not be clear. great that there is 100% coverage for file.py. are all the changes to aa.py covered by tests? (ie, other parts that you didn't touch still might not have full coverage)
(01:20:14 PM) tyhicks: sarnold: that wasn't suggested - what was suggested was to do acked-by-timeout and include them in 2.11 and then address any review feedback or discovered bugs in 2.11.1
(01:20:25 PM) ***jjohansen agrees with sarnold on .0 not .1
(01:20:53 PM) sarnold: ahh okay
(01:21:15 PM) jdstrand: I'm asking because I'm leaning towarads what jjohansen and sarnold are saying. need it in .0. if the changed code has coverage, I'd be inclined to push it sooner and fix in .1 myself
(01:21:31 PM) cboltz: jdstrand: unfortunately, it's hard to have test coverage for large parts of aa.py (especially the interactive stuff), so the answer is: no, the new/changed code in aa.py doesn't have full test coverage
(01:21:52 PM) jdstrand: that came out a little weird. I like jjohansen's idea in general and would feel pretty good about it depending on coverage
(01:22:01 PM) sbeattie: cboltz: what's the status of the json/yast stuff ?
(01:22:39 PM) jdstrand: cboltz: are the manual tests needed for the uncovered parts documented somewhere?
(01:23:30 PM) cboltz: sbeattie: besides my proof of concept patch (basically a quick hack at the openSUSE Conference), there are no news
(01:23:41 PM) jdstrand: if so, perhaps there could be an abbreviated review-- go through the manual tests, so if there is anything else to poke, then push with the understanding a full review would come later
(01:23:48 PM) cboltz: jdstrand: does /dev/brain count? ;-)
(01:23:56 PM) jdstrand: s/so if/see if/
(01:24:10 PM) sbeattie: cboltz: okay. long term, that might make automating interactive testing easier.
(01:24:27 PM) cboltz: indeed ;-)
(01:24:40 PM) jdstrand: cboltz: cat /dev/brain > ./utils/manual-testing.txt :)
(01:24:54 PM) cboltz: ;-)
(01:25:13 PM) tyhicks: manual testing instructions would really be nice
(01:25:15 PM) ***kshitij8 is not sure if we want to dsitribute cboltz's brain 
(01:25:15 PM) jdstrand: like sbeattie said, that would be a good thing regardless of this merge
(01:26:09 PM) tyhicks: since I'm not a user of the utils, I don't like making changes to them because I don't know how to drive them very well
(01:26:28 PM) cboltz: basically I'm testing aa-genprof and aa-logprof with my "hello world" script I'm using in my AppArmor talk because that contains some bits of everything (exec, read, write)
(01:27:22 PM) cboltz: and I'm sometimes doing specific tests that are expected to use code paths I changed - but that's specific for each patch and hard to document
(01:28:13 PM) jdstrand: imho, utils/manual-tests.txt wouldn't have to be a tutorial or massive guide. just the commands to exercise certain things, etc. if it is already covered in the unit tests then no need to deep dive
(01:28:31 PM) cboltz: and the hardest to document and reproduce test is the usual "just use it", which I'm doing all the time
(01:29:20 PM) cboltz: I'd argue having a manual-tests.txt file is a bug
(01:29:30 PM) cboltz: (that can/should be fixed by JSON-based tests)
(01:29:48 PM) tyhicks: we're dragging on this topic
(01:29:50 PM) jdstrand: maybe shipping a manual-tests.log and point the tools at it would be part of this
(01:29:56 PM) jdstrand: we are, I'm sorry
(01:30:01 PM) tyhicks: are we trying to decide if we are comfortable with acked-by-timeout?
(01:30:13 PM) jdstrand: I guess I was hoping that the tests would have full coverage for the changed code
(01:30:30 PM) jjohansen: I think I am inclined to vote for you to do a commit by acked-by-timeout, blame me and you fix the bugs
(01:30:31 PM) cboltz: jdstrand: that would be nice, but boring ;-)
(01:30:39 PM) jdstrand: well, it seemed like cboltz didn't want acked-by-timeout, and others did. I was trying to strike a balance
(01:30:58 PM) jjohansen: sure, it was certainly worth asking
(01:31:11 PM) cboltz: I _never_ want the timeout way ;-)
(01:31:21 PM) tyhicks: he does say in the 00 patch that "Test coverage jumps from 48% to 56%" so that is nice
(01:31:48 PM) jdstrand: unfortunately I have a very hard timeout now and have to leave
(01:31:58 PM) jdstrand: acked-by-timeout: +0 (abstain)
(01:32:08 PM) tyhicks: jdstrand: you're excused-by-timeout
(01:32:11 PM) jdstrand: hehe
(01:32:17 PM) cboltz: ;-)
(01:32:21 PM) jjohansen: basically we have 3 choices
(01:32:21 PM) jjohansen: 1. ship without cboltz's file rules patch
(01:32:21 PM) jjohansen: 2. ship with the file rules patch + acked-by timeout
(01:32:21 PM) jjohansen: 3. delaying shipping until review
(01:32:46 PM) sbeattie: how soon do we need/want to ship 2.11?
(01:33:04 PM) sbeattie: If not necessarily this week, we can give a bit more time for review.
(01:34:15 PM) tyhicks: I don't think we're going to try to a shoehorn a 2.11 into Ubuntu Yakkety, are we?
(01:34:58 PM) tyhicks: if not, there's no urgent need from the Ubuntu side for a 2.11 release this week
(01:35:13 PM) tyhicks: if so, we'd probably want the release ASAP since we're well past feature freeze
(01:36:01 PM) cboltz: feature freeze? What's that? ;-)
(01:37:20 PM) tyhicks: cboltz: do you have an urgent need for 2.11?
(01:38:35 PM) cboltz: openSUSE Leap 42.2 will be released in November, but I don't have an urgent reason to get 2.11 in. 2.10.1 is also fine ;-)
(01:40:22 PM) cboltz: (actually 2.10.1 + all patches we have in the 2.10 branch)
(01:44:46 PM) tyhicks: so I don't think there's an urgent need for 2.11 but if it happens within the next week or so, I could try to get it into Ubuntu Yakkety
(01:45:24 PM) cboltz: Yakkety is 16.10?
(01:45:28 PM) tyhicks: yes
(01:45:31 PM) cboltz: LTS?
(01:45:33 PM) tyhicks: no
(01:45:43 PM) tyhicks: 16.04 was the LTS
(01:46:53 PM) cboltz: ah, ok
(01:48:08 PM) tyhicks: sbeattie: since it doesn't sound like there's an urgent need, maybe we decide based upon your schedule
(01:50:01 PM) sbeattie: okay.
(01:50:25 PM) tyhicks: sbeattie: have any idea when a good time would be?
(01:50:31 PM) sbeattie: How about we shoot for releasing next, that gives time for last minute reviews and patches to come in?
(01:50:35 PM) sbeattie: next week
(01:50:46 PM) jjohansen: I'm good with that
(01:50:51 PM) tyhicks: me too
(01:51:26 PM) sbeattie: cboltz: work for you?
(01:51:36 PM) cboltz: yes
(01:51:41 PM) jjohansen: alright settled, lets move on
(01:51:55 PM) cboltz: just tell me when I shall commit as "timeout" ;-)
(01:52:08 PM) sbeattie: okay
(01:54:42 PM) jjohansen: so for the upstream patches I am going to be pushing I am going to be referring to them as apparmor 4.
(01:54:42 PM) jjohansen: This is because during its dev, it has seen 3.0, 3.1, and 3.5 releases in ubuntu, and I'd like to avoid confusion around what goes upstream 
(01:54:42 PM) jjohansen: I previously brought up making the next release version 3 to sync with the kernel version, so now I think the next version should be 4 instead of 3
(01:54:53 PM) jjohansen: same goal, sync it to the kernel version
(01:55:31 PM) cboltz: sounds good
(01:55:35 PM) tyhicks: I think that's fine
(01:55:35 PM) cboltz: when do you expect
(01:55:38 PM) cboltz: a) the kernel bugfixes
(01:55:43 PM) cboltz: b) the new rule types
(01:55:50 PM) cboltz: to appear in the official kernel?
(01:56:18 PM) jjohansen: ah, so many bug fixes have gone up into the 4.8 kernel, but that is against the old code base
(01:56:21 PM) tyhicks: jjohansen: I think you may be the only one that pays attention to apparmor kernel release versioning so you can make the versioning look however you want :)
(01:56:46 PM) jjohansen: I want the new code base to land in 2.11, I am too late for 2.10 already
(01:56:47 PM) sbeattie: jjohansen: so should 2.11 really be 3.0?
(01:57:09 PM) jjohansen: and features will follow, some will make 2.11 other later depending on upstream
(01:58:02 PM) jjohansen: to make a 2.10 release I would have had to push to James already, well not quite I have a few weeks, but I don't think its enough
(01:58:41 PM) tyhicks: I'm extremely confused
(01:58:45 PM) ***sbeattie too
(01:58:47 PM) jjohansen: sbeattie: meh, if you want, but suse and upstream are still on v2 kernels so its not as urgent
(01:58:54 PM) tyhicks: when you say, "to make a 2.10 release", what are you talking about?
(01:58:55 PM) jjohansen: okay
(01:59:07 PM) ***cboltz guesses "2.10.2"
(01:59:43 PM) jjohansen: tyhicks: urg did I say 2.10? I mean 4.10, 4.11
(01:59:44 PM) jjohansen: sorry
(01:59:59 PM) jjohansen: switching between userspace and kernel versions ...
(02:00:32 PM) tyhicks: you have time for 4.10 since they're only at 4.8-rc6
(02:00:36 PM) sarnold: wait your kernel versioning is to 4.10 already? :)
(02:01:42 PM) cboltz: sounds like you {c,sh}ould push the new rule types to 4.9, not 4.10
(02:01:47 PM) jjohansen: as for kernel version 4.8 is nearly done baking and James has already basically closed his merge window for 4.9, I might be able to hit 4.10 but James wants the patches no later than 4.9-rc3, and I need time for them to be on the LSM list and respond etc.
(02:01:52 PM) kshitij8 left the room (quit: Ping timeout: 480 seconds).
(02:02:18 PM) jjohansen: that is why I said I missed 4.10 already, its such a tight window already with uhm other responsibilities to finish up first
(02:02:31 PM) jjohansen: tyhicks: ^
(02:02:52 PM) tyhicks: jjohansen: thanks for explaining, I forgot about James' requirements
(02:03:06 PM) jjohansen: so basically our merge window for 4.10 is a month earlier than other peoples
(02:03:41 PM) jjohansen: sarnold: no 4.8 is about to drop, but see above
(02:04:33 PM) jjohansen: so for our next userspace release I would like to set a release date target of Feb. 2017
(02:05:01 PM) jjohansen: I think the feature set should largely be the things that are already in progress but aren't landing in 2.11
(02:05:22 PM) jjohansen: to make it realistically possible to release in Feb next year
(02:05:32 PM) tyhicks: that'd be nice
(02:05:57 PM) tyhicks: gsettings mediation, better support for compound labels in libapparmor, what else?
(02:05:57 PM) jjohansen: but we can do actual more in depth planning at next months meeting, in case there are a few additional items we want to target
(02:06:05 PM) tyhicks: ok, I'm fine waiting until next month
(02:06:14 PM) jjohansen: caching improvements
(02:06:20 PM) ***tyhicks nods
(02:06:21 PM) jjohansen: possibly policy caching
(02:06:46 PM) jjohansen: type support and caching (much of this is done in the xapparmor side project)
(02:07:02 PM) jjohansen: but again I think next month
(02:07:25 PM) jjohansen: anyone else want to chime in before we move on
(02:07:26 PM) cboltz: xapparmor? Did I miss that?
(02:07:54 PM) jjohansen: cboltz: no, it is something I have been slowly working on, on the side, there have been no announcements
(02:08:05 PM) jjohansen: it is an apparmor pluggin for xace
(02:08:21 PM) jjohansen: it is not ready yet
(02:08:28 PM) tyhicks: IIRC, it is a full rewrite in golang, right?
(02:08:30 PM) jjohansen: and I haven't worked on it for months atm
(02:08:43 PM) jjohansen: tyhicks: exactly :)
(02:08:58 PM) ***tyhicks stops trolling
(02:09:25 PM) jjohansen: cboltz: I don't expect it to be done for the next release, but it might be nice to cherry-pick some of the work done, and move it into the libs
(02:09:31 PM) jjohansen: but we can discuss next month
(02:09:41 PM) cboltz: ok
(02:09:49 PM) jjohansen: alright apparmor-profiles conversion in git eval
(02:10:19 PM) jjohansen: any comments? sbeattie this has been your nightmare, how do you think it went
(02:10:33 PM) cboltz: I miss the commit mails ;-)
(02:11:37 PM) sbeattie: cboltz: me too, I'm told that's coming soon(tm).
(02:12:04 PM) cboltz: does that soon[tm] come with a bug number I could monitor?
(02:12:06 PM) sbeattie: jjohansen: it's okay, I missed cleaning up some things in the repo, but at this point, it is what it is.
(02:12:16 PM) sbeattie: cboltz: not that I'm aware of.
(02:13:28 PM) tyhicks: this is new: http://blog.launchpad.net/code/linking-git-merge-proposals-to-bugs
(02:13:39 PM) sbeattie: anyway, I'm trying to revisit converting the main tree (and release branches) to git. 
(02:14:34 PM) cboltz: personally, I'd call the missing commit mails a blocker
(02:14:45 PM) cboltz: (RSS would also be ok, but I want some type of commit notification)
(02:16:11 PM) sbeattie: I still also need to find out whether the launchpad translations stuff supports git
(02:17:27 PM) jjohansen: that would be another blocker
(02:17:39 PM) jjohansen: so we aren't ready to convert yet
(02:17:59 PM) jjohansen: and we are still evaluating and maybe waiting on commit notifications
(02:18:23 PM) jjohansen: but we can evaluate that later when we have figured out translations etc.
(02:18:30 PM) jjohansen: so we will revisit next month
(02:18:59 PM) jjohansen: alright moving on lifeforms you wanted to discuss mod_apparmor
(02:19:49 PM) lifeforms: aight!
(02:20:50 PM) jjohansen: lifeforms: the floor is yours
(02:21:23 PM) lifeforms: mod_apparmor is still working pretty well, with one small bug (KeepAlive), however I've done some experiments with HTTP/2 (mod_h2, which is still marked experimental by Apache) and mod_apparmor completely breaks down when using it
(02:21:42 PM) lifeforms: I'm afraid due to the new multiplexing, some assumptions may not hold any more
(02:21:49 PM) jjohansen: lifeforms: so what OS and kernel version are you using?
(02:22:33 PM) lifeforms: jjohansen: currently I'm using 14.04 LTS / 3.13.0-93
(02:23:03 PM) lifeforms: I don't think the module has wide usage but this is kind of a shame as it's really great!
(02:23:38 PM) lifeforms: I'm not ready yet to take over its maintainership ;) but I would gladly put some days in to help debugging, maybe some time at the end of this year
(02:23:53 PM) jjohansen: lifeforms, okay so this is really an issue with bugs/usage that you need help with?
(02:23:56 PM) lifeforms: I'm not sure if you guys have apache devs currently on the team?
(02:24:22 PM) lifeforms: no, I think some structural changes might unfortunately be needed to support new features of Apache like HTTP/2 :(
(02:24:25 PM) lifeforms: but, I may be wrong
(02:24:44 PM) sarnold: do you see the brakage only with http2? I thought jj fixed a kernel bug recently that was strongly correlated to buggy mod_apparmor
(02:24:44 PM) jjohansen: lifeforms: no apache devs, sbeattie did the original hacking on apache for mod_apparmor but he would not claim to be an apache dev
(02:24:56 PM) jjohansen: we are more than willing to take all the help we can get
(02:25:13 PM) jjohansen: sarnold: that bug was in suse/upsteam
(02:25:38 PM) jjohansen: the ubuntu kernels are different, but a similar bug might exist in the 14.04 kernel, I need to check
(02:25:41 PM) sarnold: ahhh
(02:26:21 PM) jjohansen: lifeforms: can we delay this discussion a few minutes, to finish up the meeting and let some of the other people go and then we will come right back to your problem
(02:26:26 PM) lifeforms: jjohansen: sure!
(02:26:44 PM) lifeforms: if it's just a local problem then sorry for interrupting :)
(02:27:31 PM) jjohansen: lifeforms: I don't know about it being just a local problem, but certainly not a problem that everyone in the meeting necessarily wants to hang around for
(02:27:42 PM) lifeforms: <3
(02:27:46 PM) jjohansen: alright does anyone else have something to discuss
(02:29:08 PM) jjohansen: alright I will take that as a no
(02:29:34 PM) tyhicks: not me
(02:29:39 PM) jjohansen: next meeting is scheduled for Tues Oct 11, 2016. If that is a problem speak now or raise it on the ml
(02:30:16 PM) sarnold: "why is it so far away?"
(02:30:22 PM) tyhicks: wfm
(02:30:24 PM) sarnold: "oh. that's next month"
(02:30:56 PM) jjohansen: sarnold: sorry I know 2017 would have worked better for you but ...
(02:31:11 PM) cboltz: ;-)
(02:31:36 PM) jjohansen: alright meeting adjourned, thanks everyone
``` 
