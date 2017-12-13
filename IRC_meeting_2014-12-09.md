```
(12:06:09 PM) jjohansen: mdeslaur, jdstrand, sbeattie, tyhicks, sarnold, kees: meeting time
(12:06:16 PM) mdeslaur: \o
(12:06:19 PM) sarnold: wow didn't we just do this?
(12:06:56 PM) ***cboltz only had a meeting with some christmas cookies some minutes ago
(12:07:31 PM) Guest1075 left the room (quit: Ping timeout: 480 seconds).
(12:07:40 PM) tyhicks: hello
(12:07:50 PM) ***sbeattie is kinda here
(12:08:06 PM) jjohansen: sarnold: of course we hold back to back meetings just to confuse you
(12:08:18 PM) sarnold: jjohansen: mission accomplished
(12:09:16 PM) jjohansen: alright lets get started. There isn't really anything much on agenda except discussing 2.10/3.0 dev directions/targets
(12:10:02 PM) jjohansen: as this currently stand it looks like the next release will be a minor update
(12:10:21 PM) cboltz: nevertheless, don't expect this to be a 5 minute meeting ;-)
(12:11:18 PM) jjohansen: - better support for systemd (a library)
(12:11:18 PM) jjohansen: - better tooling
(12:11:18 PM) jjohansen: - kernel work focused on upstreaming the out of tree patches
(12:11:18 PM) jjohansen: and a few other niceties that creep in
(12:11:42 PM) jjohansen: cboltz: ah come on I always plan on 5 minute meetings
(12:11:50 PM) jjohansen: :)
(12:11:54 PM) cboltz: ;-)
(12:11:54 PM) tyhicks: jjohansen: and it will be versioned as "2.10"?
(12:12:10 PM) jjohansen: tyhicks: I think so
(12:12:31 PM) jjohansen: though, meh we could go to 3.0
(12:12:41 PM) tyhicks: 2.10 sounds good to me
(12:13:08 PM) tyhicks: I had just opened up the libapparmor.map file to add new symbols and didn't know what the next version was going to be :)
(12:13:15 PM) tyhicks: so I was only curious
(12:13:19 PM) sarnold: will both dpkg and rpm handle 2.2 < 2.10 fine?
(12:13:28 PM) cboltz: let's decide based on the changes (and remaining TODO list) shortly before the release ;-)
(12:13:41 PM) jjohansen: sarnold: it should
(12:13:56 PM) jjohansen: s/it/they/
(12:13:59 PM) sbeattie: sarnold: yes
(12:14:26 PM) jjohansen: cboltz: yeah, the actual versioning isn't really important atm
(12:15:15 PM) sbeattie: let's tentatively plan on calling it 2.10 and if it warrants calling it 3.0 at a later point, then we can adjust.
(12:15:46 PM) tyhicks: +1
(12:15:49 PM) jjohansen: +1
(12:16:36 PM) sarnold: +1  :)
(12:16:51 PM) jjohansen: cboltz: is there anything is particular you would like to see in the next release
(12:17:21 PM) cboltz: I even have a with for 2.9.1 ;-)
(12:17:22 PM) jjohansen: I know debian/tails would like to see better overlay file system support
(12:17:43 PM) cboltz: please fix log parsing so that it works with syslog format - we have bugreports from multiple distributions about this
(12:18:08 PM) jjohansen: and I plan to dust off my alias patches and try to get them in, to help meet that request
(12:18:31 PM) cboltz: besides that, my obvious wish is to get the rule classes into the python tools
(12:18:52 PM) cboltz: (the question is if we want them in 2.9.1 or 2.10...)
(12:19:18 PM) jjohansen: I'd tend to think the classes change is too big for 2.9.1
(12:19:58 PM) jjohansen: the next release should be early sprint next year
(12:20:27 PM) cboltz: one big reason to get them into 2.9.1 is to avoid maintaining of the current python code ;-)
(12:20:47 PM) cboltz: I'm not aware of terrible bugs (which means we hit a good time to release 2.9.0)
(12:20:48 PM) jjohansen: I think there is some wiggle room on dates, but we should get it out instead of just slipping as happened last time
(12:21:12 PM) cboltz: but still I'm not too keen to fix bugs in the 2.9-style python code ;-)
(12:21:21 PM) jjohansen: cboltz: I get not wanting to maintain the current python code
(12:22:25 PM) sarnold: cboltz: do you feel the patches are better-tested than the 2.9.0 release was? I'm on the fence about it .. on the one hand, I -really- know that feeling, on the other hand, the changes are pretty big.
(12:22:31 PM) jjohansen: and since we do have a fair few bugs in it that need fixing, I am not completely opposed to shoving the class work into to 2.9.1 or 2.9.2
(12:23:16 PM) cboltz: sarnold: the class itsself are fully covered by testcases - only the aa.py changes are not
(12:23:32 PM) sbeattie: keep in mind that we've only converted capabilities to use the rules stuff; I suspect we will iterate on the structures a bit as we convert network and file rules.
(12:23:34 PM) cboltz: so the risk of introducing bugs is quite low (but it's not impossible)
(12:24:23 PM) sbeattie: .... considering that we'd already introduced two major bugs that we've thankfully squished, I'd say the risk is non-trivial.
(12:25:02 PM) cboltz: I know, but those bugs were very obvious and catched (and fixed) quickly
(12:25:25 PM) sbeattie: so I'd like to see it land on trunk and move forward on converting additional rule types, and if we're happy with the way things look after that, we can consider backporting and landing in a 2.9.2 release.
(12:25:56 PM) cboltz: ok, sounds like a good idea
(12:26:35 PM) jjohansen: +1
(12:27:17 PM) cboltz: this means you'll create a 2.9 branch before commiting the class patches, right?
(12:27:24 PM) sbeattie: yes
(12:29:14 PM) jjohansen: alright does anyone have anything else to discuss around 2.10/3.0?
(12:30:24 PM) cboltz: as I already said (and nobody noticed?), fixing logparser for syslog format sounds like the top-priority bug to fix ;-)
(12:30:40 PM) jjohansen: cboltz: yes, it needs work
(12:30:54 PM) sbeattie: cboltz: yeah, I'm hoping to look at that this week
(12:32:01 PM) cboltz: :-)
(12:32:56 PM) jjohansen: ok, lets move on then
(12:33:15 PM) tyhicks: regarding 2.10> In the next 24 hours, I'll send out patches that move the aa_match, aa_features, aa_policy_cache, and aa_kernel_interface APIs (the patch set I sent to the list last Friday) over to libapparmor
(12:34:01 PM) tyhicks: and then move on to implementing the "multiple policy caches" feature
(12:34:09 PM) jjohansen: tyhicks: nice
(12:34:18 PM) tyhicks: (that's all that I wanted to mention)
(12:34:47 PM) jjohansen: ok, moving on tyhicks: do you want to give a brief update on dbus upstreaming
(12:35:09 PM) tyhicks: no update on that front :(
(12:35:25 PM) jjohansen: ack
(12:35:39 PM) jjohansen: does anyone have anything else they want to discuss?
(12:35:46 PM) cboltz: I'm just reading today's bugmail, and noticed that lots of bugs expired. Will one of you check them and reopen if needed?
(12:36:02 PM) cboltz: (I'm quite sure some of them are valid bugs.)
(12:36:43 PM) Corey84 [~Corey84@devel.mesa.gmu.edu] entered the room.
(12:36:45 PM) jjohansen: cboltz: indeed they are, I have the set sitting in my bug mail too. I just haven't gone over them yet
(12:37:20 PM) Corey84 is now known as Guest1083
(12:38:06 PM) sbeattie: yeah, same here.
(12:39:11 PM) jjohansen: anything else?
(12:39:43 PM) cboltz: well, just the usual reminder that there are some pending patches ;-)
(12:40:48 PM) jjohansen: the next meeting is tentatively scheduled for Tue Jan 13 @20:00 UTC, if anyone has conflicts you can speak up now or poke the list
(12:41:44 PM) sbeattie: no conflict from me
(12:41:57 PM) cboltz: I'm away on that day and don't know when I'll be back
(12:42:29 PM) cboltz: (depending on how good party after the main event is ;-)
(12:42:58 PM) sarnold: man what kinda party is this? :)
(12:43:31 PM) jjohansen: actually with the christmas/winter break I am wondering if we shouldn't just skip the Jan meeting
(12:44:19 PM) cboltz: sarnold: it's the party after the first day of the "Weinbautage" (wine grower days), so I know one thing for sure - there will be good wine ;-)
(12:44:28 PM) cboltz: (and I do NOT talk about the software ;-)
(12:44:45 PM) sarnold: cboltz: nice :)
(12:46:13 PM) jjohansen: cboltz: so do you want us to shift the schedule of the meeting to accomodate?
(12:46:38 PM) cboltz: if you want to be sure I'll attend the meeting, it would be a good idea ;-)
(12:47:21 PM) cboltz: (and if we do it a week later, we can collect some things to discuss after the christmas break)
(12:47:32 PM) jjohansen: cboltz: so the question being shift to February or just a few days?
(12:47:50 PM) cboltz: I'd say some days
(12:48:48 PM) jjohansen: A week would put it on the 20th, 2 weeks the 27, 3 weeks Feb 3
(12:49:03 PM) jjohansen: I am assuming you would rather the 20th?
(12:49:26 PM) cboltz: yes, if you all are ok with it
(12:49:49 PM) jjohansen: I'm fine with that, any objections?
(12:50:39 PM) tyhicks: that's fine
(12:50:48 PM) sbeattie: nope, I'll look at what I can schedule to conflict with the 20th.
(12:50:56 PM) sarnold: looks fine for me
(12:51:07 PM) jjohansen: okay, lets tenatively schedule for the 20th
(12:51:19 PM) jjohansen: meeting adjourned
(12:51:19 PM) jjohansen: thanks everyone
``` 
