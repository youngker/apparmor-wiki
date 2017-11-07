```
(01:02:12 PM) jjohansen: jdstrand, sbeattie, sarnold, tyhicks, cboltz: meeting time
(01:02:26 PM) ***sbeattie o/
(01:03:02 PM) jdstrand: hey
(01:06:35 PM) jjohansen: hrmmm, well lets roll with it
(01:06:54 PM) jjohansen: the agenda such as it is http://wiki.apparmor.net/index.php/MeetingAgenda
(01:07:27 PM) jjohansen: so first up, the wiki update is complete, and everything should be working. please let us know if you encounter any issues
(01:08:18 PM) sbeattie: jjohansen: thanks for pushing that
(01:08:32 PM) jjohansen: np, sorry it took so long
(01:08:50 PM) jjohansen: lets move, 2.9.2 has been released
sbeattie sbeattie1 
(01:09:38 PM) jjohansen: sbeattie: what time frame do you think we should be looking at for 2.9.3? I am thinking it should be the last of 2.9.x because we should be releasing 2.10 soon
(01:10:42 PM) sbeattie: I agree that it should probably be the last 2.9 release, am guessing in a month or two as we accumulate more fixes.
(01:10:51 PM) cboltz: jjohansen: you said something similar for 2.8.x - and I probably don't need to tell you that "next version released" doesn't prevent a maintenance release for the previous branch ;-)
(01:11:17 PM) tyhicks: I'm here (I missed the earlier ping)
(01:11:31 PM) jjohansen: cboltz: it doesn't but we are trying to minimize those. I'm not ruling them out, but not planning for them yet
(01:11:32 PM) sbeattie: cboltz: yeah, I'm not ruling out a 2.9.4 
(01:12:16 PM) sbeattie: but I really think we should be focused on getting 2.10 out
(01:12:28 PM) jjohansen: yes
(01:12:35 PM) cboltz: indeed, I agree on focussing on 2.10
(01:12:40 PM) jjohansen: speaking of which that is the next item
(01:12:51 PM) cboltz: but if fixing 2.9 is just "apply that patch", then it's cheap to do that ;-)
(01:12:53 PM) jjohansen: so what do we have left to land for 2.10?
(01:13:09 PM) tyhicks: I think we're only missing some patch reviews
(01:13:24 PM) cboltz: does someone want to claim my special offer from yesterday?
(01:13:37 PM) jjohansen: cboltz: sure, we are planning for a 2.9.3 and will re-evaluate as we go after that
(01:14:02 PM) tyhicks: [apparmor] [PATCH v2 00/14] libapparmor API improvements/fixes, openat(2) style, man pages
(01:14:09 PM) tyhicks: [apparmor] Provide public function to split confinement contexts
(01:14:36 PM) tyhicks: AFAIK, the patches in those two threads ^ are the only ones waiting acks for 2.10
(01:14:46 PM) cboltz: the ChangeProfileRule patchset is also pending ;-)
(01:14:59 PM) jjohansen: okay
(01:15:14 PM) jjohansen: so lets focus on getting those in
(01:15:43 PM) jjohansen: I will kick out my most recent set of man page up dates and updates to the library log parsing
(01:16:05 PM) cboltz: maybe also a fix for the regression I mentioned yesterday - did someone find out what I broke? ;-)
(01:16:15 PM) jjohansen: though I don't consider those blockers for 2.10, if they make it fine but if not they can go in the next release
(01:16:42 PM) tyhicks: oh nice, more than half of the 14 patches for 'libapparmor API improvements/fixes, openat(2) style, man pages' already have acks
(01:16:51 PM) jjohansen: cboltz: I haven't, do you want to start guessing on the large list of regressions that I created :)
(01:17:24 PM) cboltz: I'm only aware of one regression related to NetworkRule and CapabilityRule
(01:17:38 PM) cboltz: but if you found another one, I'll also accept that
(01:17:47 PM) cboltz: so - what's on your list? ;-)
(01:19:18 PM) jjohansen: well uhm lets not dig into the sad bug list right now
(01:20:12 PM) cboltz: you can still claim your price after the meeting - let's say until 22:00 UTC
(01:20:17 PM) jjohansen: okay, so sbeattie, tyhicks, cboltz do you think if we target next week for 2.10, that getting all this in by then is feasible?
(01:20:21 PM) cboltz: after that, I'll disclose what I found ;-)
(01:21:12 PM) cboltz: next week sounds good to me
(01:21:23 PM) sbeattie: jjohansen: that sounds... ambitious. But eh, wouldn't hurt to push a bit.
sbeattie sbeattie1 
(01:21:53 PM) jjohansen: sbeattie: notice I didn't specify when next week
(01:21:54 PM) tyhicks: jjohansen: it mostly depends on how much time can be devoted to patch review
(01:22:08 PM) jjohansen: tyhicks: ack
(01:22:34 PM) jjohansen: alright, lets try for it, I certainly would like to get 2.10 out the door this month
(01:22:50 PM) tyhicks: this month would be nice
(01:23:09 PM) cboltz: tyhicks: send your patches *now*, and they'll be acked-by <timeout> before jjohansen does the release ;-)
(01:23:33 PM) jjohansen: so with 2.10 going out the door soon, do we want to start planning a rough time line for 2.11 or should we leave that for next month
(01:24:07 PM) cboltz: features or timeline planning?
(01:24:29 PM) jjohansen: cboltz: just rough timeline atm
(01:24:48 PM) jdstrand: besides the stuff already mentioned as needed patch review, what are key changes in 2.10?
(01:24:48 PM) jjohansen: I don't think we are in a position to talk features, just yet.  At least I am not
(01:24:59 PM) jdstrand: s/needed/needing/
(01:25:24 PM) jdstrand: (just otoh)
(01:25:35 PM) sarnold: perhaps next month makes more sense; we'll know better where we are once 2.10 is out
(01:25:45 PM) sbeattie: jjohansen: I'd imagine relasing 2.11 in 5-6 months. But some of that depends on the features that we want for it.
(01:25:50 PM) jjohansen: jdstrand: I think atm we are just waiting on those pieces to be reviewed and go in
sbeattie sbeattie1 
(01:26:37 PM) jjohansen: sbeattie: somewhat, but it also can depend on distro release timing, and we can always cut certain features if needed
(01:26:52 PM) jdstrand: right, I meant-- why are we rolling 2.10 at all (ie, what changed that is prompted the release)
(01:27:06 PM) sbeattie: right, I was just about to say that, too, that disto release needs may drive it somewhat.
(01:27:40 PM) jjohansen: jdstrand: actually there are a fair few patches in 2.10, that ubuntu carries distro patched on 2.9
(01:27:46 PM) ***jdstrand notes there is no judgement passing-- just curious what important things are in 2.10 
(01:27:50 PM) sbeattie: jdstrand: the sysmted launch stuff depends on the libapparmor changes tyhicks already landed no? We (ubuntu) definitely want that for 15.10.
(01:27:51 PM) tyhicks: jdstrand: it would be nice to land the libapparmor cache loading API and the aa_splitcon(3) interface
(01:27:52 PM) jjohansen: it would be nice to have a clean 2.10 for people to work from
(01:28:22 PM) jjohansen: yes, the systemd work warrants a nice versioned release
(01:28:29 PM) cboltz: jdstrand: the python *Rule classes are an important change IMHO (they make code readable and uncovered and fixed some bugs along the way)
(01:28:30 PM) jdstrand: ah right, so systemd is the big feature, with incremental tool updates and bug fixes
(01:29:02 PM) jjohansen: yes
(01:29:06 PM) jdstrand: ok, thanks, sorry for derailing (I didn't mean for it to be a justifying thing-- I just couldn't remember otoh :)
(01:29:07 PM) tyhicks: and the dbus-daemon mediation code that landed requires aa_splitcon(3)
(01:30:59 PM) jjohansen: right
(01:31:21 PM) jjohansen: okay, lets do some 2.11 planning next month
(01:31:47 PM) jjohansen: does anybody have anything else they would like to discuss?
(01:32:03 PM) cboltz: yes, two technical issues ;-)
(01:32:24 PM) cboltz: number one - handling of "systemctl restart apparmor"
(01:32:47 PM) cboltz: on openSUSE, I have the problem that systemd maps "restart" to "stop, then start"
(01:32:59 PM) cboltz: and that means removing AppArmor protection from running processes
(01:33:17 PM) jjohansen: I have 2 possible solutions
(01:33:31 PM) jjohansen: 1. fix systemd so it uses restart instead of stop
(01:34:04 PM) jjohansen: 2. make stop a nop like ubuntu did, and add teardown to strip protection if really wanted
(01:34:11 PM) sarnold: cboltz: where does that remapping happen? does it happen because it's still using sysvinit compatibility scripts?
(01:34:20 PM) jjohansen: apparmor doesn't really fit the traditional daemon stop
(01:34:45 PM) cboltz: the current solution is a *.service that calls /etc/init.d/boot.apparmor
(01:35:02 PM) cboltz: (we needed to introduce that because of a broken systemd release that messed up the dependencies)
(01:35:34 PM) cboltz: IMHO the perfect solution would be to have   ExecRestart=   in systemd units
(01:35:46 PM) cboltz: but I'm not sure if it's easy to get that in
(01:35:59 PM) cboltz: I know it was requested at least twice in the past, and declined
(01:36:13 PM) cboltz: but nobody mentioned "security" as keyword back then ;-)
(01:37:02 PM) sarnold: did they come forward with patches? or just a vague desire to have it?
(01:37:17 PM) jjohansen: cboltz: so atm, systemd can't/won't provide it
(01:37:45 PM) cboltz: I didn't dig that deep in the discussion, but IIRC it was just a desire
(01:37:46 PM) jjohansen: I am not saying that avenue shouldn't be pursued but a parallel alternate route is needed
(01:37:54 PM) cboltz: and declined with "nobody needs it"
(01:38:37 PM) cboltz: I'll write a mail to systemd-devel when I have some time and request ExecRestart a third time
(01:39:34 PM) cboltz: with the arguments that a) we need it for security, b) I'm not the first one who asks for it, so there is a real need ;-)
(01:40:04 PM) cboltz: I'll try to avoid trolling and evil sidenotes in the first mail, even if that might be hard ;-)
(01:41:41 PM) cboltz: so if nobody wants to add anything about systemd, I have another technical issue
(01:41:51 PM) cboltz: handling of hats and subprofiles in aa-complain
(01:42:18 PM) cboltz: in old versions (up to 2.8.x), aa-complain etc. changed the flags in all subprofiles and hats
(01:42:18 PM) MoC [~u@3OZAABM2B.tor-irc.dnsbl.oftc.net] entered the room.
(01:43:01 PM) cboltz: 2.9 doesn't do that because of a buggy regex that matched lots of stuff, but not hats ;-)
(01:43:18 PM) cboltz: and in the meantime that part is commented out to at least avoid changing something wrong
(01:43:22 PM) cboltz: now the questions are:
(01:43:34 PM) emj___ [uid17349@id-17349.uxbridge.irccloud.com] entered the room.
(01:43:36 PM) cboltz: - should the hat and subprofile be changed?
(01:44:12 PM) cboltz: - should that happen always / by default / if --with-subprofiles is specified / only if --only-main-profile is not specified?
(01:44:30 PM) sarnold: it probably makes sense to change them all -- it seems rare to me that someone would want "mostly enforcing except for this bit" -- it feels like it only makes sense to swap them all at once.
(01:44:46 PM) cboltz: - do we want to backport that to 2.9.x? (which would mean a behaviour change)
(01:44:49 PM) jjohansen: I think people are generally surprised when they try to stick a profile into complain and its hats don't get changed as well
(01:44:59 PM) tyhicks: I agree with sarnold and jjohansen 
(01:45:33 PM) jjohansen: I think we could classify the 2.9 behavior as a regression, in which case a backport is fine
(01:45:46 PM) jjohansen: but I am not going to push for it either
(01:45:53 PM) cboltz: yes, "it's a bugfix" ;-)
(01:46:19 PM) cboltz: (interestingly, I didn't see a flood of bugreports about this topic)
(01:46:49 PM) jjohansen: I have had people asking/complaining about the behavior
(01:47:08 PM) jjohansen: I don't think any of them bothered with bug reports though
(01:47:13 PM) cboltz: doing it for all hats opens another can of worms on the programming side, but let's ignore this technical detail in the meeting
(01:47:45 PM) cboltz: (just a hint - if I re-enable the hat code, it sets the hats to the same flags as the main profile, independent of the flags the hat had before)
(01:48:01 PM) jjohansen: pfftt its only technical details, and cboltz's blood sweat and tears.
(01:48:04 PM) jjohansen: :)
(01:48:04 PM) sbeattie: cboltz: it doesn't just affect aa-complain, aa-genprof/logprof leave child profiles/hats in complain mode after setting the parent to enforcing.
(01:48:47 PM) sarnold: sbeattie: eww.
(01:48:49 PM) cboltz: sbeattie: I didn't notice that yet, but it's a while since I used aa-genprof to create a profile with hats ;-)
(01:49:03 PM) sbeattie: I noticed it more by creating child profiles.
(01:49:08 PM) cboltz: (but at least that finally qualifies this issue as bugfix)
(01:50:04 PM) sbeattie: "No thanks, please don't create a global mount(1) profile when all this tool is using mount for is to query the list of existing mounts."
(01:50:04 PM) cboltz: how important is keeping the other flags of hats?
(01:50:11 PM) sarnold: probably setting the flags the same is fine too -- it might not necessarily be ideal but I don't seee any issues with it either
(01:50:57 PM) cboltz: I could probably come up with a "override hat flags" patch quickly, but doing it right (preserving different hat flags) is a bit more work
(01:50:57 PM) sbeattie: I can see it being somewhat problematic for disconnected_paths, where I child needs it but the parent doesn't, but I can live with it as a smaller regression.
(01:51:13 PM) sbeattie: s/I child/a child/
(01:52:16 PM) cboltz: ok, so I'll go the quick fix route for now ;-)
(01:52:25 PM) jjohansen: I think that is acceptable
(01:52:42 PM) jjohansen: alright anything else?
(01:53:18 PM) cboltz: not from me ;-)
(01:53:58 PM) tyhicks: I don't have anything else
(01:54:45 PM) jjohansen: okay, next meeting is tentatively scheduled for Tuesday June 9, 
(01:54:58 PM) jjohansen: let us know if this is an issue so we can move it
(01:55:31 PM) cboltz: I'm probably away on that evening
(01:56:19 PM) tyhicks: I can attend
(01:57:13 PM) sbeattie: it works for me, but I'm available other times
(01:57:33 PM) jjohansen: alright, so lets run with it for now, cboltz keep us posted if you want to attend we can reschedule when it gets closer
(01:57:49 PM) cboltz: ok
(01:57:57 PM) jjohansen: thanks everyone,
(01:57:57 PM) jjohansen: meeting adjourned
(01:58:00 PM) sarnold: thanks jjohansen, everyone :)
``` 
