```
(01:00:06 PM) cboltz: meeting time ;-)
(01:00:36 PM) jjohansen: indeed
(01:01:08 PM) ***sbeattie o/
(01:01:12 PM) jjohansen: jdstrand, sbeattie, sarnold, tyhicks, mdeslaur, cboltz, kees, kshitij8_, intrigeri: meeting time
(01:01:20 PM) tyhicks: hello
(01:01:35 PM) sarnold: \o/
(01:01:39 PM) mdeslaur: \o
(01:01:56 PM) kshitij8_: \o
(01:02:22 PM) ***lifeforms is just a fly on the wall
(01:02:22 PM) jjohansen: eh I guess that is good enough, lets get started
(01:02:46 PM) jjohansen: lifeforms: hehe, flies on the wall are welcome too :)
(01:03:27 PM) jjohansen: the agenda is at http://wiki.apparmor.net/index.php/MeetingAgenda#Next_Meeting:_IRC_Meeting.2C_20:00_UTC.2C_April_14_2015
(01:03:29 PM) ***cboltz looks for the flyswatter
(01:03:54 PM) jjohansen: The first item on the agenda on is the wiki update
(01:04:17 PM) jjohansen: it is not done, yet and I sent a new request in
(01:04:25 PM) Guest2072 left the room (quit: Ping timeout: 480 seconds).
(01:05:01 PM) jjohansen: I will continue to nag until it is done or if we can't get it done look for alternate hosting, where we can manage it ourselves
(01:05:27 PM) jjohansen: next up 2.9.x, sbeattie you're up
(01:05:37 PM) sbeattie: okay.
(01:05:42 PM) cboltz: I probably already mentioned that I know something about mediawiki, so if we decide for self-hosting... ;-)
(01:05:57 PM) kshitij8_: self-hosting is a bad idea IMHO
(01:06:12 PM) ***jjohansen notes cboltz has volunteered to manage the wiki :)
(01:06:24 PM) sbeattie: hehe
(01:07:00 PM) cboltz: kshitij8_: well, I always prefer if someone else does the work - but if the work doesn't get done, _we_ will have to do it
(01:07:03 PM) jjohansen: kshitij8_: meh, self-hosting is just one option. The current issue is we can't directly manage the wiki and have to send in support requests
(01:07:53 PM) jjohansen: I am happy to have external hosting, I would like the option of us doing some management on it
(01:08:02 PM) jjohansen: anyways back to 2.9, sbeattie
(01:09:02 PM) sbeattie: Thanks. So I plan to release 2.9.2 in the next couple of days; I want to spot check open bugs and do some testing, but it's a bit overdue IMO.
sbeattie sbeattie_ 
(01:09:47 PM) jjohansen: sbeattie: I have a few man page updates, I can drop in but don't hold up 2.9.2 for them
(01:10:12 PM) sbeattie: Okay.
(01:10:51 PM) jjohansen: everyone make sure your patches that have been nominated for 2.9 are in
(01:10:58 PM) jjohansen: alright lets move on to 2.10
(01:11:13 PM) cboltz: can we please add _pending_ patches to the list?
(01:11:31 PM) cboltz: some of my pending patches are bugfixes ;-)
(01:11:33 PM) jjohansen: cboltz: to the agenda?
(01:11:35 PM) sbeattie: I am happy to release a 2.9.3 for things that don't make it.
(01:11:46 PM) cboltz: jjohansen: to 2.9.2 ;-)
(01:11:50 PM) sbeattie: in a month or two.
(01:12:04 PM) sbeattie: but I also think we should be releasing 2.10 in that same time frame.
(01:12:13 PM) jjohansen: cboltz: ah, so we could, it depends on how long they will take
(01:12:17 PM) cboltz: actually my pending 3x patches are near timeout, so they could go in in the next days ;-)
(01:12:46 PM) jjohansen: cboltz: how many of those are appropriate for 2.9 vs. just going into 2.10
(01:13:13 PM) cboltz: at least 33, 35 and 36 should go into 2.9
(01:13:22 PM) sbeattie: cboltz: I pulled back a bunch of the trunk patches yesterday; I know you have more outstandinding for both...
(01:13:23 PM) cboltz: otherwise the libreoffice profile crashes the tools
(01:13:33 PM) sbeattie: okay, I'll try to look at those.
(01:14:03 PM) cboltz: also 30-logparser-change-mask-only-for-path-events.diff would be a good candidate, but it's a slightly bigger change
(01:14:25 PM) jjohansen: cboltz: can you go through and point out the ones that should go into 2.9 so we can prioritize them, it doesn't have to be done now just ping us with the list when you are done
(01:14:41 PM) sbeattie: yeah, that would be good.
(01:15:06 PM) cboltz: actually, the above is the complete list of patches that must go into 2.9.2 IMHO
(01:15:35 PM) jjohansen: okay, thanks
(01:15:35 PM) jjohansen: back to 2.10, I had hoped we would be ready to cut a release but we aren't
(01:15:46 PM) jjohansen: we still have quite a list of outstanding patches
(01:16:00 PM) tyhicks: all the code for 2.10 is out on the list
(01:16:06 PM) corey84- [~Corey84@ip-64-134-150-191.public.wayport.net] entered the room.
(01:16:07 PM) tyhicks: there aren't too many pending patches, though
(01:16:17 PM) jjohansen: I think we are at a point whether these are things that need to go into 2.10 or whether they can wait for 2.11
(01:16:18 PM) tyhicks: https://lists.ubuntu.com/archives/apparmor/2015-April/007654.html and https://lists.ubuntu.com/archives/apparmor/2015-April/007731.html are the two patch sets
(01:16:26 PM) tyhicks: and most of the patches in the first set already have acks
(01:16:52 PM) jjohansen: tyhicks: right, but for example you the split context patches
(01:17:04 PM) jjohansen: cboltz: has quite a list of patches
(01:17:22 PM) jjohansen: I actually have a large set of patches I haven't pushed
(01:17:43 PM) cboltz: oh, I think I should disclose what I'll sending later
(01:17:56 PM) tyhicks: jjohansen: the first patch set that I linked to needs to go in or needs to be NACKed before 2.10 as it modifies the new interfaces that have never been released
(01:17:59 PM) sbeattie: cboltz: okay, thanks.
(01:18:17 PM) cboltz: it will be a set of patches that replace the network rule handling with NetworkRule and NetworkRuleset classes :-)
(01:18:19 PM) sbeattie: (sorry, my dsl line is flaking out)
(01:18:21 PM) jjohansen: for example I have userspace dfa patches so we can improve our testing, but I don't think it is worth delaying 2.10 for them so I haven't sent them
(01:18:26 PM) tyhicks: the aa_splitcon() patch set could wait until 2.11
(01:18:31 PM) jjohansen: tyhicks: right
(01:19:24 PM) jjohansen: but with the splitcon patches done and on the list I am not opposed to including them
(01:19:39 PM) jjohansen: pending acks
(01:19:52 PM) sbeattie: I'd like to see them go in as well.
(01:20:09 PM) tyhicks: for Ubuntu's sake, they would be nice to have
(01:20:27 PM) jjohansen: what I am trying to say is, we need to start drawing lines and finish this stuff up so we can have a release this month
(01:20:32 PM) sbeattie: yeah
(01:20:52 PM) tyhicks: I can't start moving applications to the new, upstream D-Bus way of getting a peer's AppArmor context until aa_splitcon() is available
(01:21:02 PM) jjohansen: cboltz: what are you planning on sending, and is it 2.10 material or could it wait for 2.11
(01:21:23 PM) sbeattie: one thing I intend to propose is making python3 the default for 2.10, and perhaps putting a deprecation notice for python 2.x.
(01:21:24 PM) jjohansen: tyhicks: ack, so I think putting those in 2.10 is required
(01:21:32 PM) cboltz: see above - replacing the somewhat buggy handling of network rules with NetworkRule(set) classes
(01:21:41 PM) cboltz: I'm not sure if that shoud go into 2.10, but I'm sure it fixes some bugs in handling network rules
(01:21:56 PM) cboltz: (see my list of remaining bugs in one of my mails about write_net_rules)
(01:21:59 PM) jjohansen: cboltz: any time estimates on that?
(01:22:15 PM) cboltz: I'll send the patches after the meeting
(01:22:29 PM) jjohansen: cboltz: okay thanks
(01:22:44 PM) jjohansen: does anyone else having something they think needs to hit 2.10
(01:22:47 PM) cboltz: so it basically depends on the review speed and how many issues are found during review ;-)
(01:23:39 PM) jjohansen: cboltz: so how about this, we will do our best, but if it runs into big issue it may get bumped to 2.11
(01:24:44 PM) jjohansen: alright so lets move on
(01:24:45 PM) cboltz: sounds good - as long as you accept backporting it to 2.10.1 as valid solution for any network rule-related bug ;-)
(01:25:21 PM) jjohansen: I'll differ on commenting on that until after I see the patches :)
(01:25:29 PM) jjohansen: okay lets move on
(01:25:33 PM) cboltz: ;-)
(01:25:47 PM) jjohansen: does anyone else have something they would like to bring up
(01:26:40 PM) cboltz: just a small note - now that we have the BaseRule(set), migrating more rule types to classes became quite easy :-)
(01:27:08 PM) jjohansen: nice :)
(01:27:22 PM) cboltz: (path rules might be an exception, but that's not too surprising ;-)
(01:27:32 PM) lifeforms: I have that keepalive bug that cboltz and I hit in mod_apparmor, I don't have a clear test case yet but would have time in May to check it out.. is somebody still working on mod_apparmor?
(01:28:41 PM) cboltz: sbeattie: can you help that fly? ;-)
(01:29:06 PM) sbeattie_ left the room (quit: Remote host closed the connection).
(01:29:19 PM) sbeattie: well, we should work on more frequent releases anyway, so trunk stuff lands in people's hands more quickly.
(01:30:02 PM) sbeattie: lifeforms: argh, yeah, I keep meaning to get back to that, though I don't have any iOS devices to trigger it.
(01:31:57 PM) lifeforms: sbeattie: I'll try to make a script for it. I think it's related to a request coming in just at the wrong (near teardown?) moment of keepalive period but it's strange!
(01:32:37 PM) jjohansen: sbeattie: I have some ios devices that you might be able to borrow
(01:33:19 PM) jjohansen: okay I will throw in one more note, the Tor project is doing a "Tor summer of privacy" much like Google summer of code, intrigeri and disdi are putting together a proposal to improve/fix the apparmor/overlayfs interaction. This would be a great improvement so, give them what help you can
(01:33:35 PM) lifeforms: I guess most people don't hit it because they have longer keepalive, in any case, I'll mail to the maillist when I've a good test case.
(01:34:03 PM) sbeattie_ [~steve@208-151-246-43.dq1sn.easystreet.com] entered the room.
(01:34:36 PM) sbeattie: lifeforms: thanks, it's appreciated. I need to document the debug options for the module a bit better, too, though I'm wary of them changing the timing if this is a race of some kind.
(01:35:35 PM) jjohansen: alright the next meeting is scheduled for Tuesday May 12 @ 20:00 UTC, please yell if this doesn't work for you
(01:35:41 PM) sbeattie: improving overlayfs interaction would be great!
(01:36:00 PM) jjohansen: indeed
(01:36:25 PM) sbeattie: may 12 should be okay for me
(01:36:39 PM) lifeforms: cboltz are you watching the Eurovision song contest? ;)
(01:37:06 PM) tyhicks: may 12 works for me
(01:37:13 PM) cboltz: lifeforms: no, I'm not too interested in that. Too much show ;-)
(01:37:56 PM) cboltz: (and the winner is typically the "most crazy", not the "best singer or song")
(01:38:17 PM) cboltz: may 12 sounds good, BTW
(01:39:12 PM) jjohansen: okay, thanks for coming everyone
(01:39:17 PM) jjohansen: meeting adjourned
(01:39:29 PM) sarnold: thanks jjohansen, everyone ;)
(01:39:32 PM) sbeattie: thanks jjohansen 
(01:39:39 PM) tyhicks: jjohansen: did you get a response regarding the apparmor/overlayfs design and/or the tor proposal?
```
 
