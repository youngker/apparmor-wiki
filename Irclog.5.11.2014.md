```
(12:01:32 PM) jjohansen: sbeattie, sarnold, tyhicks, jdstrand, mdeslaur, cboltz: meeting time
(12:01:44 PM) mdeslaur: \o
(12:02:04 PM) sarnold: /o\
(12:02:20 PM) ***sbeattie is here
(12:02:24 PM) sarnold: (that's the emoticon for burrying one's head in the sand, right?)
(12:03:07 PM) jjohansen: http://wiki.apparmor.net/index.php/MeetingAgenda
(12:03:07 PM) ***cboltz looks out for some cement
(12:03:41 PM) tyhicks: hello
(12:03:47 PM) jjohansen: alright lets get started
(12:04:49 PM) jjohansen: 2.9 has been released! We have queued up a few fixes already so I think we will probably roll a 2.9.1 sooner than later
(12:05:39 PM) jjohansen: cboltz: do you have a set of tool changes you would like to get in?
(12:06:21 PM) cboltz: no pending patches, everything I have is in bzr
(12:06:43 PM) cboltz: (well, except the "make clean" patch I sent last week)
(12:07:45 PM) jjohansen: okay, so I was thinking a time frame of early december, giving us a couple more weeks to accumulate a few more fixes (since there hasn't been any really serious bugs)
sbeattie sbeattie1 
(12:08:40 PM) ***cboltz wonders - can this be true?
(12:09:19 PM) jjohansen: cboltz: of course not, let me rephrase. No one has complained loudly about any serious bugs yet
(12:09:25 PM) jjohansen: :)
(12:09:47 PM) sarnold: :)
(12:10:29 PM) jjohansen: anyone have anything else for 2.9?
(12:11:16 PM) jjohansen: alright lets move on
(12:11:29 PM) jjohansen: 2.10/3.0 dev targets and planning?
(12:12:11 PM) jjohansen: basically I'm not ready to discuss this yet, I could give some vague handy wavy stuff
(12:12:47 PM) jjohansen: generally I am thinking a 2.10 release
(12:13:22 PM) cboltz: using rule classes in the py tools is something I have in mind, but without a fixed timeline
(12:13:34 PM) cboltz: (I prefer "good" over "fast" ;-)
(12:13:41 PM) jjohansen: right
(12:14:06 PM) jjohansen: timeline wise I think we are looking at a minor release in 4-5 months
(12:14:06 PM) cboltz: any opinions about the class proposal I sent some weeks ago?
(12:14:27 PM) jjohansen: hopefully we can get better systemd support in and a few other cleanups
(12:14:58 PM) jjohansen: I don't think we are targeting any major feature developments in this time frame
(12:15:14 PM) sbeattie: cboltz: I didn't think about it very hard, but yes, I'd most definitely like to move in that direction going forward.
(12:15:20 PM) tyhicks: I'm working on breaking the cache setup and binary cache loading into a libapparmor (for systemd support)
(12:16:12 PM) tyhicks: lifting out the use of globals, exit() calls, and printing from the cache setup code is a bit of a beast, but I'm making progress
(12:16:43 PM) cboltz left the room (quit: Remote host closed the connection).
(12:16:58 PM) cboltz [~cb@95.90.21.179] entered the room.
(12:17:00 PM) jjohansen: and there will be more iteration on getting the rest of the parser beat into shape to be used as a library, but we won't get all the way there in the next 4-5 months
(12:17:41 PM) jjohansen: btw tyhicks work to split out the cache loading is a big part of the better systemd support I mentioned
(12:18:01 PM) jjohansen: though there will be also work in systemd itself
(12:18:18 PM) ***cboltz is back from a connection issue - did I miss something between tyhicks "I'm working on breaking ..." and jjohansen "and thre will be more ..."?
(12:18:30 PM) tyhicks: only one message
(12:18:37 PM) tyhicks: < tyhicks> lifting out the use of globals, exit() calls, and printing from the cache setup code is a bit of a beast, but I'm making progress
(12:18:59 PM) cboltz: sounds like fun ;-)
(12:19:48 PM) cboltz: just FYI, I discussed the class layout with Kshitij on IRC, and he mostly liked it
(12:20:18 PM) tyhicks: cboltz: moving to a class-based design is definitely the right thing to do but I haven't had a chance to take a good look at your proposal
(12:20:20 PM) cboltz: (with the exception of having access to all profiles - just the profile a rule class belongs to is enough)
(12:20:45 PM) jjohansen: yeah its definitely the right direction
(12:21:04 PM) cboltz: feel free to point out any road bumps in it ;-)
(12:21:35 PM) jjohansen: does anyone have anything else for 2.10/3.0?
(12:21:50 PM) cboltz: oh, and as usual, I'm quite sure we'll find the more interesting problems when actually implementing it
(12:22:09 PM) cboltz: my plan is to start with a simple rule type (maybe capability)
(12:22:21 PM) cboltz: to find out if the class layout works in practise
(12:23:03 PM) cboltz: and later move on to the more interesting rule types like file rules
(12:26:14 PM) jjohansen: okay, lets move on. Think about 2.10/3.0 for the next meeting so we can plan it a little better
(12:26:44 PM) jjohansen: cboltz: do have anything more to discuss about python tool improvements, maintenance and cleanup?
(12:27:12 PM) cboltz: nothing as big as the rule classes ;-)
(12:27:27 PM) cboltz: sbeattie: I had a short look at your patch to improve the utils test
(12:27:50 PM) cboltz: in general I like it, but I'd prefer to loop over an array intead of having a function for each severity
(12:28:10 PM) cboltz: is there a special reason why you wrote so many little functions?
(12:29:34 PM) jww [~jww@0001f161.user.oftc.net] entered the room.
(12:29:54 PM) sbeattie: to have them be independent testcases, rather than one large. The other alternative that does that is to add the test functions dynamically at startup, but that breaks python-nose's ability to pull out individual tests (useful for debugging an individual failing test)
(12:31:12 PM) cboltz: so it's basically independent, individual debugable testcases vs. avoiding code repetion ;-)
(12:32:01 PM) ***cboltz would prefer an array with all _simple_severity_test testcases
(12:32:28 PM) sbeattie: with a side helping of trying not to make the test code too clever, which can also make figuring out what's going wrong more difficult.
(12:32:50 PM) sbeattie: but I guess that's the 'debuggable' part.
(12:33:11 PM) sarnold: indeed, I prefer simple and stupid especially in test cases :)
(12:33:27 PM) sarnold: I don't want to spend more time debugging test frameworks than the code itself..
(12:33:27 PM) cboltz: even if it's basically copy&paste programming?
(12:33:30 PM) tyhicks: I have to say that I prefer simple, little test functions instead of the array-based design
(12:34:33 PM) tyhicks: cboltz: yes - IMO, simple and debuggable tests take precedence over bad code reuse when it comes to tests
(12:35:34 PM) cboltz: sounds like NAK'ing all those little functions won't have a chance ;-)
(12:35:48 PM) cboltz: ok, if you all prefer little functions, I'll accept that style
(12:36:13 PM) cboltz: (but I'll still fight copy&paste programming outside the testcases ;-)
(12:36:20 PM) tyhicks: we all will
(12:36:32 PM) sarnold: thanks cboltz :)
(12:37:08 PM) jjohansen: okay, does anyone else have anything they would like to discuss
(12:37:44 PM) tyhicks: a quick update on upstreaming the dbus-daemon patches...
(12:37:45 PM) sbeattie: cboltz: indeed, we have wayyyy too much copy&waste programming in the utils already.
(12:38:08 PM) tyhicks: I've gotten another round of feedback from upstream
(12:38:21 PM) tyhicks: I've addressed all but one of those pieces of feedback
(12:38:45 PM) tyhicks: and I just shot off an email asking if they want me to keep rebasing the patch set or if they want small fix patches at this point
(12:39:05 PM) tyhicks: so, I should have v4 of the patch set posted to the upstream bug by next week
(12:39:25 PM) tyhicks: lets hope that it is "the one"
(12:39:44 PM) sbeattie: tyhicks: woot, thanks for persevering on that.
(12:40:25 PM) tyhicks: sbeattie: having to merge new dbus releases into each ubuntu release provides plenty of motivation :)
(12:41:15 PM) cboltz: speaking about upstreaming - what about the kernel patches? ;-)
(12:41:31 PM) jjohansen: hehe, how did I know that was coming
(12:41:39 PM) cboltz: BTW: openSUSE 13.2 ships with AppArmor 2.9.0, but the kernel doesn't support the new rule types
(12:41:43 PM) jjohansen: I am sending a small set of patches up this week
(12:42:03 PM) tyhicks: cboltz: that's the only sane thing for openSUSE to do atm
(12:42:07 PM) jjohansen: I am also working on cleaning up the dev tree so we can start submitting it
(12:42:12 PM) jjohansen: yes
(12:42:21 PM) cboltz: tyhicks: the kernel patches are insane?
(12:42:23 PM) jjohansen: the dev tree is kind of big and ugly
(12:42:43 PM) tyhicks: cboltz: trying to pull in the apparmor dev tree would be a decent amount of work
(12:42:47 PM) jjohansen: cboltz: its big, about 180 patches atm
(12:43:11 PM) tyhicks: jjohansen: what small set of patches are you planning on sending up this week?
(12:43:14 PM) ***jjohansen wants to get it cleanedup and down to about 30-40 patches for the upstreaming
(12:43:20 PM) cboltz: oh, that sounds interesting[tm] ;-)
(12:43:57 PM) jjohansen: tyhicks: so a few small bug fixes cherry-picked from the series, the parameter to turn on and off hashing
(12:44:14 PM) jjohansen: basically the little bits that are independent from the rest of the labeling mess
(12:44:19 PM) tyhicks: ok
(12:44:31 PM) tyhicks: was just curious if it was the stuff needed for dbus mediation
(12:44:42 PM) tyhicks: (I would have added a note to the upstream dbus bug)
(12:45:07 PM) jjohansen: no, though it will have the some of the query interface patches
(12:45:17 PM) tyhicks: ok
(12:45:38 PM) tyhicks: that's all of the questions/comments from me
(12:45:45 PM) jjohansen: the advertising of dbus support will stay out (as promised) until the net labeling lands
(12:45:51 PM) tyhicks: ack
(12:45:54 PM) jjohansen: okay lets move on
(12:45:56 PM) jjohansen: The next meeting is tentatively scheduled for Tuesday Dec 9 @ 20:00 UCT.  Any conflicts?
(12:46:18 PM) cboltz: not yet ;-)
(12:46:47 PM) sbeattie: not from me.
(12:47:15 PM) sarnold: not yet :)
(12:47:48 PM) tyhicks: works for me
(12:48:10 PM) jjohansen: okay lets run with it, if conflicts arise please send a message to the list
(12:48:21 PM) jjohansen: meeting adjourned
(12:48:23 PM) jjohansen: thanks everyone
``` 
