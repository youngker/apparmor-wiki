```
(04:02:08 PM) jjohansen: cboltz, sbeattie, sarnold, jdstrand, tyhicks, mdeslaur: meeting time
(04:02:44 PM) mdeslaur: \o
(04:02:56 PM) jdstrand: hi
(04:03:17 PM) cboltz: low ;-)
(04:04:03 PM) ***h01ger raises an eyebrow
(04:04:29 PM) tyhicks: hello
(04:04:53 PM) jjohansen: h01ger: hey, your more than welcome. Its just the regular schedule monthly IRC meeting.
(04:04:58 PM) jjohansen: wiki.apparmor.net/index.php/MeetingAgenda
(04:05:08 PM) jjohansen: alright lets get started
(04:05:45 PM) jjohansen: tyhicks: can you give a quick update on dbus apparmor upstreaming as we have been getting a few inquiries again
(04:06:04 PM) tyhicks: jjohansen: there are no updates
(04:06:09 PM) ***h01ger is just watching from the sidelines, having recently become active in pkg-aa-profiles-team in debian
(04:06:19 PM) tyhicks: jjohansen: I'm waiting for a review
(04:06:28 PM) h01ger: s/active/a little bit active/
(04:06:54 PM) jjohansen: tyhicks: hrmmm, I thought you had a rev you were going to send up, or repoke
(04:07:25 PM) tyhicks: jjohansen: I've incorporated all feedback that I've got
(04:07:40 PM) tyhicks: jjohansen: I may have said that I'd poke Simon
(04:07:46 PM) tyhicks: jjohansen: but I haven't done that lately
(04:07:54 PM) jjohansen: yeah, oh I got it you where just rebasing, no changes. sorry /me misremembered
(04:08:25 PM) jjohansen: tyhicks: np, thanks. Be prepared to be annoyed by my forgetfulness next month
(04:08:32 PM) jjohansen: moving on
(04:08:52 PM) tyhicks: I'm (nicely) asking again for a review in the bug right now
(04:09:10 PM) jjohansen: tyhicks: oh nice, thanks
(04:09:49 PM) jjohansen: we need to get 2.8.4 out the door soon. Sbeattie did we land all the mod_apparmor patches that we want there?
(04:10:25 PM) ***cboltz still didn't have time to test the last patch
(04:10:28 PM) sbeattie: jjohansen: no, I just resubmitted the two outstanding for 2.8.4
(04:10:58 PM) sbeattie: I'm running the trunk version of them live, but I don't have a terribly complex environment.
sbeattie sbeattie_ 
(04:11:37 PM) jdstrand: I thought we weren't going to do 2.8.4 before rtm?
(04:12:02 PM) cboltz: "rtm"?
(04:12:10 PM) jjohansen: sbeattie: okay thanks. We will get them in this week and see if we can't cut a 2.8.4 in the next week or two
(04:12:32 PM) jdstrand: cboltz: sorry, it is an Ubuntu acronym. we have an earlier feature freeze looming
(04:12:32 PM) jjohansen: jdstrand: that was the plan, but we also wanted to get in the mod_apparmor fixes
sbeattie sbeattie_ 
(04:13:15 PM) jdstrand: I'm fine spinning 2.8.4 but can we wait a couple/few weeks for abstract sockets to land?
(04:13:24 PM) jdstrand: (in trunk/Ubuntu)
(04:13:47 PM) jjohansen: jdstrand: abstract sockets won't land in 2.8.4, that will be 2.9-betaX
(04:13:53 PM) jdstrand: I know
(04:13:59 PM) jjohansen: ah okay
(04:14:18 PM) jdstrand: I mean, can we focus on 2.9-betaX for a coule/few more weeks before diverting resources to 2.8.4?
(04:14:48 PM) jdstrand: (specifically a full release-- I don't care about patch review)
(04:14:59 PM) jjohansen: well the question is, do we call for broader testing of the 2.8.4 changes and poke kees etc to test or do we roll 2.8.4 and push it out, as it can't be worse than 2.8.3
(04:15:12 PM) jdstrand: the full release always seems to take a while and we need all the days we can get
(04:15:27 PM) cboltz: give me some days to test the latest mod_apparmor patch
(04:15:31 PM) jdstrand: why do we need to push 2.8.4 now?
(04:15:40 PM) cboltz: besides that, I vote for "just get it out"
(04:15:42 PM) jjohansen: jdstrand: oh understood, mostly I want to have a plan, and get someone else testing the changes
(04:16:18 PM) jjohansen: jdstrand: well because 2.8.3 is broken for apache, that isn't exactly a good thing
(04:16:19 PM) jdstrand: that's fine-- again, it is just when we spin up the tarball, actual release, release notes, etc, etc it never seems to be a short amount of time
(04:16:37 PM) jjohansen: it does take a bit, yes.
(04:16:49 PM) jdstrand: sure-- people can cherrypick the patches for a couple of weeks if needed-- apache has been broken for months
(04:16:51 PM) jjohansen: and again I understand where you are coming from
(04:17:03 PM) jdstrand: ok
(04:18:36 PM) jjohansen: cboltz: okay, noted I'm fine with that and since you are the mostly likely immediate consumer that is probably what we will do
(04:19:16 PM) cboltz: luckily I didn't push out 2.8.3 as maintenance release on openSUSE
(04:19:30 PM) jjohansen: cboltz: we will get the patches in ASAP, but I can't promise a release unless some cycles free up some where. But we will do the best we can. you are always welcome to pull before the official release
(04:19:34 PM) cboltz: (which would have given us some good statistics of mod_apparmor users *g*)
(04:19:45 PM) jjohansen: cboltz: yeah
sbeattie sbeattie_ 
(04:19:59 PM) cboltz: so I'll test the mod_apparmor patch on one server
(04:20:02 PM) sarnold: does kees know to not pull a 2.8.3 for debian in the meantime?
(04:20:05 PM) cboltz: and then just wait until 2.8.4 is out
(04:20:07 PM) jjohansen: sbeattie: is there anything else you wanted to get in?
(04:20:18 PM) jjohansen: sarnold: yeah he hit lots of 2.8.3 issues
(04:20:24 PM) sarnold: jjohansen: okay good :) thanks
(04:20:38 PM) sbeattie: Off the top of my head, no, that's the only world burning issue.
(04:20:55 PM) jjohansen: okay thanks.
(04:20:55 PM) sbeattie: I *think* kees was using trunk, as he hit signal issues.
(04:21:31 PM) jjohansen: oh hrmmm, yeah now that you mention it. He was using trusty, which needs an SRU
(04:21:47 PM) cboltz: the patch I sent for abstractions/nameservice an hour ago might be an interesting candidate also for 2.8
(04:21:52 PM) jjohansen: but that is something for Ubuntu to discuss somewhere else :P
(04:22:00 PM) cboltz: (basically, nscd in /run is slightly broken)
(04:22:50 PM) jjohansen: cboltz: okay, we will make sure to clear off the outstanding patches this week so we can make sure to get them in, for our next alpha beta, and we can commit them to 2.8 as well if nominated
(04:22:55 PM) jjohansen: okay moving on
(04:23:20 PM) jjohansen: So we need to roll the next 2.9 asap
(04:23:39 PM) jjohansen: is there any point in doing it before the abstract socket stuff lands?
(04:23:53 PM) ***jjohansen thinks not, but just wants to make sure
(04:24:10 PM) cboltz: depends - when will it land, and what will it break? ;-)
(04:24:34 PM) jjohansen: it will start showing up this week
(04:25:23 PM) jjohansen: should have already started showing up, but some one (jjohansen) is making revisions that are taking longer than planned for.
(04:25:26 PM) sbeattie: cboltz: when do you want to pull it into opensuse?
(04:25:28 PM) tyhicks: there's no point in a 2.9 before abstract sockets lands, IMO
(04:25:31 PM) sarnold: jjohansen: nahh, the annoyance of new releases makes me think we ought to wait for the abstract sockets
(04:25:37 PM) ***jjohansen still expects parser patches to land today
(04:26:02 PM) cboltz: I don't have a hard schedule, but I'd like to get 2.9 in ASAP
(04:26:04 PM) jjohansen: alright, so we wait for abstract to land, and shake out a little bit
(04:26:18 PM) cboltz: but not beta1 with all the bugs it had (and that were fixed in the meantime)
(04:26:25 PM) jjohansen: that means we are looking at rolling 2.9 next week some time
(04:27:17 PM) jjohansen: jdstrand: can you live with that?
(04:27:48 PM) tyhicks: that seems a little ambitious
(04:27:52 PM) cboltz: BTW: jjohansen, you didn't answer the "what will it break" part of my question ;-)
(04:28:09 PM) jjohansen: cboltz: the world of course
(04:28:15 PM) sbeattie: tyhicks: I think he means "rolling the next 2.9 beta next week"
(04:28:20 PM) sbeattie: BICBW
(04:28:20 PM) tyhicks: yeah
(04:28:27 PM) jdstrand: jjohansen: yeah, our date to land is next friday iirc
(04:28:33 PM) jjohansen: tyhicks: it is ambitious ^
(04:28:51 PM) jdstrand: I mean, we'll do what we have to, but that is why it is all hands on deck :)
(04:29:33 PM) jjohansen: tyhicks: bug fixes can happen after landing the feature
(04:29:58 PM) sarnold: cboltz: netstat -x will show unix domain sockets -- iirc the abstract sockets have '@' at the start of their names...
(04:30:13 PM) tyhicks: sure, but I'm just thinking back to similiar features such as signal mediation
(04:30:13 PM) jjohansen: yes
(04:30:29 PM) tyhicks: we didn't go from no patches on the list to being ready for a beta release in a week
(04:30:36 PM) tyhicks: it doesn't matter though
(04:30:40 PM) tyhicks: just move as fast as possible :)
(04:31:47 PM) jjohansen: alright, that is all I have on the agenda, does anyone have anything else?
(04:31:48 PM) tyhicks: s/just move/we'll just move/
(04:31:53 PM) tyhicks: I don't have anything
(04:32:12 PM) sbeattie: jjohansen: when the time comes to spin the releases, I'll take them
sbeattie sbeattie_ 
(04:32:32 PM) jjohansen: sbeattie: thanks
(04:32:50 PM) sbeattie: Nothing else from me.
(04:34:33 PM) jjohansen: okay, I'll take silence as nothing from anyone else
(04:34:55 PM) jjohansen: our next meeting is tentatively schedule for Aug 12 @ 20:00 UTC
(04:35:18 PM) jjohansen: if this is a problem, ping us here or on the ml
(04:35:22 PM) mdeslaur: thanks jjohansen!
(04:35:24 PM) tyhicks: thanks jjohansen 
(04:35:27 PM) jjohansen: thanks eveyone
(04:35:33 PM) jjohansen: meeting adjourned
(04:35:34 PM) sbeattie: jjohansen: should be okay for me
(04:35:38 PM) sarnold: thanks jjohansen!
(04:35:39 PM) sbeattie: jjohansen: thanks!
```
