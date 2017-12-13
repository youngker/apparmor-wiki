```
(01:07:27 PM) jjohansen: jdstrand, sbeattie, sarnold, tyhicks, mdeslaur, cboltz: I suppose it is meeting time (a little past actually)
(01:07:42 PM) angasule_ [~angasule@24-107-4-211.dhcp.stls.mo.charter.com] entered the room.
(01:07:48 PM) mdeslaur: hey jjohansen
(01:07:49 PM) ***sbeattie waves
(01:07:51 PM) ***cboltz is here
(01:07:54 PM) jdstrand: hi
(01:08:07 PM) jdstrand: I have a hard stop in !22 minutes
(01:08:17 PM) jdstrand: ^
(01:08:21 PM) jdstrand: meh
(01:08:21 PM) jjohansen: jdstrand: that is fine
(01:08:23 PM) jdstrand: ~22 minutes
(01:08:30 PM) jjohansen: so lets get started
(01:08:31 PM) jdstrand: there, that is the right character :)
(01:08:54 PM) jjohansen: darn I liked the not 22 mins better :)
(01:09:17 PM) jdstrand: hehe on that 22nd minute, I have to go, but any other one, I'm good :P
(01:09:45 PM) jjohansen: alright, we don't have much for an agenda
(01:09:54 PM) tyhicks: I'm here
(01:10:54 PM) jjohansen: tyhicks: is working on addressing feedback from his last submission of the dbus upstreaming
(01:11:11 PM) jjohansen: I don't think there is much more to say there
(01:11:34 PM) jjohansen: 2.8.4 has been delayed due to lack of time
(01:11:34 PM) tyhicks: nope
(01:11:45 PM) ***sarnold here
(01:11:55 PM) cboltz: jjohansen: and due to an interesting[tm] bug in mod_apparmor :-/
(01:12:03 PM) jjohansen: it will be coming out in the next week or two as time/resources are available
(01:12:13 PM) ***intrigeri lurking but not really available. hi!
(01:12:15 PM) sbeattie: 2.8.4> yep, sorry about that. I need to spend time investigating the mod_apparmor issue cboltz raised.
(01:12:37 PM) jjohansen: cboltz: right, but there just hasn't been any time available to work on it
(01:12:44 PM) cboltz: sbeattie: did you check if 2.8.96 also has the mod_apparmor bug?
(01:13:00 PM) jjohansen: I expect we can get that wrapped up once we can put some time on it
(01:13:23 PM) jjohansen: cboltz: last I recall it didn't exist in 2.8.96
(01:13:42 PM) cboltz: :-)
(01:13:42 PM) jjohansen: of course I could be wrong, but it got a larger patchset
(01:14:01 PM) cboltz: then maybe the solution for 2.8.4 is to revert mod_apparmor to its 2.8.2 state?
(01:14:04 PM) sbeattie: cboltz: I have not checked. It would help if I had a small example config that demonstrated the issue.
(01:14:17 PM) jjohansen: I don't have anything more for 2.8.4, as we just haven't had time to do anything on it
sbeattie sbeattie_ 
(01:14:37 PM) cboltz: sbeattie: I'm afraid my server setup doesn't fit "small config" anymore
(01:14:38 PM) jjohansen: ah, okay thanks sbeattie, I guess I remembered wrong
(01:15:16 PM) intrigeri: I would have some profile updates backports to submit for 2.8.4, but I doubt I'll have time.
(01:15:26 PM) cboltz: sbeattie: did you ask an apache dev to review mod_apparmor (at least the apache side of it)?
(01:16:06 PM) cboltz: that would probably be more effective than programming based on the apache documentation ;-)
(01:16:11 PM) jjohansen: intrigeri: well, if you get a chance please do, I'm not sure of when we will be able to get it out of the door, but it should be some time this month
(01:16:16 PM) jjohansen: just super busy atm
(01:16:38 PM) intrigeri: got it. I think we'll simply upload 2.8.3 to Debian unstable first, then.
(01:17:07 PM) cboltz: intrigeri: add a patch that downgrades mod_apparmor to the 2.8.2 code - it's totally broken in 2.8.3
(01:17:37 PM) cboltz: (everything else in 2.8.3 is fine AFAIK)
(01:18:24 PM) peetaur left the room (quit: Quit: Konversation terminated!).
(01:18:42 PM) jjohansen: yeah, sadly 2.8.3 is bad around mod_apparmor
(01:18:57 PM) jjohansen: okay anything else to discuss around 2.8.3
(01:19:50 PM) cboltz: just wondering - should we add a warning about mod_apparmor on http://wiki.apparmor.net/index.php/ReleaseNotes_2_8_3 ?
(01:20:20 PM) jjohansen: yes
(01:20:25 PM) debfx: has anyone tried to bzr bisect the offending commit?
(01:21:20 PM) jjohansen: debfx: we actually know the commit
(01:21:27 PM) ***cboltz didn't - the server where I noticed the problem is in production mode, therefore I just did a downgrade to mod_apparmor 2.8.2
(01:21:57 PM) jjohansen: there are a set of "fixes" to mod_changehat unfortunately the fixes break it
(01:22:25 PM) jjohansen: unfortunately the fixes address real problems
(01:22:47 PM) jjohansen: it is possible we could just revert the mod_apparmor changes so we could push a 2.8.4 out the door
(01:22:51 PM) jjohansen: sbeatte: ?
(01:23:08 PM) sarnold: the bugs didn't seem to bother cboltz's production use before, right?
(01:23:26 PM) cboltz: more or less ;-)
(01:23:45 PM) cboltz: they are "known bugs", and it doesn't really matter if I see them some more months
(01:23:50 PM) sbeattie: jjohansen: well, my concern is that the same issue is present in trunk.
(01:23:57 PM) cboltz: but of course I'd prefer to have them fixed ;-)
sbeattie sbeattie_ 
(01:24:31 PM) jjohansen: sbeattie: sure, but that wouldn't stop us from working on them for a 2.8.5, 3.0 release
(01:25:06 PM) jjohansen: is it worth reverting the mod_apparmor changes on 2.8.4 so we can push it out the door
(01:25:18 PM) jjohansen: and deal with the mod_apparmor issue later
(01:25:46 PM) sbeattie: jjohansen: perhaps. I'm not happy about reverting to a known buggy state, but, alas, stupid other deadlines are interfering.
(01:26:11 PM) ***sbeattie just doesn't want to kick the can down the road forever.
(01:26:12 PM) jjohansen: right, I wouldn't even suggest it except we have been delayed months already
(01:26:27 PM) sbeattie: yeah.
(01:26:41 PM) cboltz: well, the "new" bugs are more critical than the "old" ones, therefore I tend to revert the changes
(01:26:47 PM) cboltz: to get a working 2.8.4 out soon
(01:27:01 PM) cboltz: BTW: http://wiki.apparmor.net/index.php/ReleaseNotes_2_8_3 <--- Warning added
(01:27:13 PM) jjohansen: cboltz: thanks
(01:28:04 PM) sbeattie: cboltz: if I backed out the apache api usage change (aka rev 2131) could you test that and see if it break basic http auth?
(01:28:34 PM) cboltz: yes, that should be possible
(01:29:02 PM) jjohansen: alright, I think there is a grudging consensus to revert mod_apparmor changes and get 2.8.4 out as soon as possible, and then roll the changes into a 2.8.5
(01:29:22 PM) cboltz: just send me a patch against 2.8.3
(01:29:23 PM) jjohansen: lets plan to do that (with cboltz testing the change) as soon as we can
(01:29:58 PM) sbeattie: cboltz: okay.
(01:30:07 PM) jjohansen: ok lets move on
(01:30:09 PM) sbeattie: jjohansen: ack.
(01:30:22 PM) jjohansen: 2.9 beta3 is in testing
(01:30:51 PM) jjohansen: there are still some known kernel bugs being worked on
(01:31:10 PM) ***cboltz just submitted beta2 towards openSUSE Factory - and you are already testing beta3?
(01:31:16 PM) jjohansen: but it is finally starting to shape up
(01:31:56 PM) sbeattie: FYI beta2 tarball is at https://launchpad.net/apparmor/2.9/2.9.beta2 but I have neglected to send an announcement to the list.
(01:32:01 PM) jjohansen: cboltz: yeah, I moved to the beta3 tag for the kernel when we let the unix domain socket extended mediation go live
(01:32:12 PM) jjohansen: sbeattie: oh nice thanks
(01:33:13 PM) jjohansen: currently the plan is to keep working on 2.9 beta, I don't think there are any new features planned before release
(01:33:34 PM) jjohansen: just trying to make it solid, with a release date some time next month
(01:34:01 PM) ***cboltz guesses Oct 31
(01:34:16 PM) jjohansen: hrmmm, hopefully before then
(01:34:41 PM) cboltz: we have some experience with release dates, right? *eg*
(01:34:56 PM) jjohansen: the userspace end (which will be the 2.9 release) is in pretty good shape
(01:35:02 PM) jjohansen: yes we do
(01:35:19 PM) jjohansen: don't ask me when 3.0 will be released
(01:36:15 PM) jjohansen: cboltz: basically there are probably a few bugs to fix still, certainly some outstanding patches to get in, and more of the new tests, but it is looking good
(01:36:30 PM) jjohansen: again 2.9 isn't the target for any new features
(01:36:52 PM) jjohansen: I'd like to get it forked so we can get the next release going
(01:36:53 PM) cboltz: speaking of bugs - rguenter thinks the compiler issue is a bug in AppArmor
(01:36:58 PM) cboltz: see https://bugzilla.novell.com/show_bug.cgi?id=895495 for details
(01:37:06 PM) ***jdstrand has to go
(01:37:12 PM) jjohansen: yes, I saw that. I haven't had time to look into it yet
(01:37:30 PM) jjohansen: however I will not it compiles just fine on other compiler releases
(01:38:09 PM) sarnold: I suspect a sed s'/__unused/___unused/g' might do the trick
(01:38:23 PM) jjohansen: sarnold: no
(01:38:23 PM) sbeattie: jjohansen: it's more an issue of specific versions of the kernel had an x86_64 specific header that has an __unused variable defined.
(01:38:41 PM) sbeattie: which our __unused macro expands.
(01:38:46 PM) sarnold: sbeattie: ahhhh
(01:38:51 PM) sarnold: I wondered where it came from
(01:38:58 PM) jjohansen: sbeattie: it should not be an error to define an __ macro when that macro isn't defined
(01:39:22 PM) jjohansen: yes we are defining something for the "system" namespace
(01:39:25 PM) sarnold: I was especially confused since there's an #ifndef guard..
(01:39:31 PM) jjohansen: right
(01:39:44 PM) cboltz: "wrong" include order?
(01:39:47 PM) jjohansen: s/system/implemenation/
(01:39:49 PM) jjohansen: nope
(01:40:11 PM) sbeattie: jjohansen: sure. it's a kernel bug. it's been fixed in later versions of the kernel, which is why we don't see it on 14.04 or 14.10.
(01:41:02 PM) jjohansen: sbeattie: hrmmm I am pretty sure I have built it with a different gcc and the broken kernel
(01:41:05 PM) jjohansen: but what ever
(01:41:10 PM) jjohansen: we can look at it later
(01:41:43 PM) jjohansen: okay, anything else for 2.9?
(01:42:33 PM) sbeattie: cboltz: I've only vaguely been following what you and kshitj have been fixing wrt the python utils; anything to say there?
(01:42:54 PM) cboltz: yes, but nothing really new
(01:43:07 PM) cboltz: we should get the simplified syntax of aa-mergeprof into 2.9
(01:43:36 PM) cboltz: besides that, we are fixing bugs here and there ;-)
(01:44:56 PM) cboltz: we even found the reason why logprof assigns //null-xx events to the main profile
(01:45:07 PM) cboltz: and have half of the fix ready
(01:45:28 PM) cboltz: unfortunately the other half is quite difficult (at least with non-trivial profiles)
(01:45:47 PM) cboltz: which means it will take some time until we have a production-ready patch
(01:46:34 PM) cboltz: (hunting down this bug was quite interesting[tm] and took me several hours...)
(01:46:59 PM) cboltz: anyway, that's the summary from the utils
(01:48:26 PM) jjohansen: thanks cboltz
(01:48:35 PM) jjohansen: okay moving on
(01:48:48 PM) cboltz: BTW: can someone sync the translations from lp and create an updated *.pot for the utils?
(01:49:43 PM) sbeattie: cboltz: yeah, I can do that. Though I haven't seen much recent from launchpad on the translations front.
(01:49:46 PM) jjohansen: yes, that is something we should have actually done already
(01:49:46 PM) jjohansen: sbeattie, or sarnold
(01:49:46 PM) jjohansen: thanks to all those who where able to participate in the hack fest
(01:49:58 PM) ***sbeattie merges them back periodically
(01:50:08 PM) jjohansen: haha, oops my bad on the paste there
(01:50:23 PM) sarnold: sorry, I haven't touched the translations yet..
(01:51:21 PM) jjohansen: sarnold: that is fine, just asking one of you to pick it up when you have a chance. It is really something we should have done for the snapshot that is working its way through ci-train
(01:53:52 PM) jjohansen: okay, anyways, there was an apparmor hack fest and meet up in Portland a couple of weeks ago
(01:53:55 PM) sbeattie: jjohansen: like I said, I periodically merge the lp translations branch back to trunk and also monitor the commits launchpad makes. I haven't seen much since the last time I merged.
(01:54:05 PM) jjohansen: thanks to everyone who attended, I hear it was good
(01:54:11 PM) sbeattie: (but I'll double check)
(01:54:28 PM) jjohansen: sbeattie: yep, thanks for that
(01:54:50 PM) intrigeri: I guess I should report back on what happened at DebConf14..
(01:54:58 PM) jjohansen: yes please
(01:55:11 PM) ***jjohansen hands intrigeri the floor
(01:55:27 PM) intrigeri: I meant, over email. Can't do that right now.
(01:55:31 PM) intrigeri: todo++
(01:55:55 PM) sarnold: "fun was had by all:
(01:56:28 PM) cboltz: nice to see that even debian follows the openSUSE /etc/motd ;-)
(01:56:48 PM) sarnold: :D
(01:58:47 PM) ***sbeattie was really happy to meet intrigeri and h01ger 
(01:59:04 PM) intrigeri: same! :))
(01:59:32 PM) jjohansen: intrigeri: okay, sure email is fine an update there would be great in fact
(01:59:40 PM) jjohansen: sorry I couldn't make it
(02:02:37 PM) jjohansen: alright does have anyone have anything else to discuss?
(02:04:21 PM) ***h01ger too :)
(02:06:24 PM) jjohansen: okay, I think that is a wrap then. Next meeting tentatively planned for Tue Oct. 14
(02:06:28 PM) jjohansen: thanks everyone
(02:06:33 PM) jjohansen: meeting adjourned
``` 
