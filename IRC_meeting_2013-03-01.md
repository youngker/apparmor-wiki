```
(12:01:29 PM) jjohansen: cboltz, sbeattie, sarnold, jdstrand, mdeslaur: if your interested its time for the monthly apparmor meeting
(12:01:39 PM) mdeslaur: hi jjohansen!
(12:01:49 PM) jdstrand: hi :)
(12:02:20 PM) terryh [~Terry@71-212-104-91.tukw.qwest.net] entered the room.
(12:02:21 PM) ***sbeattie o/
(12:03:16 PM) jjohansen: Well I guess that will have to do, lets get started
(12:03:20 PM) ***cboltz hides
(12:03:29 PM) jjohansen: cboltz: good idea :)
(12:03:57 PM) jjohansen: I guess first up is dealing with the 3.0 release
(12:04:02 PM) sarnold: o/
(12:04:43 PM) jjohansen: its running behind schedule (big surprise), and I propose we postpone its release
(12:05:16 PM) jjohansen: I'd rather do a good release with the base target features than trickle out just a few things
(12:06:06 PM) jjohansen: what I was thinking was setting a new target date for the beta to say the start of June or so
(12:06:10 PM) jdstrand: that seems to make sense
(12:06:55 PM) jjohansen: that gives us about 3 months more dev and polish time and we can then look at rolling a release at some point in the summer
(12:07:06 PM) jjohansen: cboltz: what is suse schedule looking like
(12:07:18 PM) sbeattie: jjohansen: would you want to do an alpha release before the beta?
(12:07:25 PM) cboltz: 12.3 will be released in two weeks
(12:07:34 PM) cboltz: and 13.1 will be in +8 months
(12:07:47 PM) jjohansen: sbeattie: yes, I'd like to roll several alphas before the beta
(12:08:04 PM) jjohansen: cboltz: so a release in 5-6 months would work out well for you then :)
(12:08:20 PM) cboltz: assuming we don't postpone again, it should work ;-)
(12:08:45 PM) sbeattie: jjohansen: okay, I wasn't sure given the previous alphas being skipped.
(12:08:54 PM) jjohansen: cboltz: well we are getting there
(12:09:08 PM) sbeattie: (but on the whole, I am okay with this plan)
(12:09:25 PM) jjohansen: sbeattie: well I kind of did a half assed alpha1 kernel, which was really on the 2.8 userspace
(12:09:35 PM) jjohansen: but never really announced it as such
(12:10:11 PM) jjohansen: sbeattie: I do really want to get an alpha out but I'd like to have certain things working well enough
(12:10:23 PM) jjohansen: we are close on the labeling/stacking and dbus
(12:10:39 PM) jjohansen: the env filtering is further out
(12:10:51 PM) cboltz: BTW and OT: I'm also making good progress with PostfixAdmin 3.0. We'll see who wins the "release 3.0" race ;-)
(12:10:59 PM) jjohansen: oh I guess we have the new fs interface which seems to be solid
(12:11:43 PM) jjohansen: cboltz: heh well I wouldn't be surprised if apparmor lost :)
(12:12:22 PM) cboltz: are you always that pessimistic? ;-)
(12:12:45 PM) jjohansen: so alpha wise my goal is to roll an actual alpha in a week or two, it won't be next week as I want to coordinate with tyhicks and get the new query interface in
(12:13:44 PM) jjohansen: cboltz: who me, I am very optimistic, I believe we will eventually have a 3.0 release ;-)
(12:14:20 PM) cboltz: ;-)
(12:15:54 PM) jjohansen: so I haven't heard any complaints with the plan, so I will record it as doing monthlyish alphas and a beta target for early june
(12:16:05 PM) jjohansen: Moving on to 2.8.2
(12:16:30 PM) jjohansen: sbeattie: you had some things you thought should probably go in?
(12:17:22 PM) jjohansen: Basically with 3.0 being delayed I think we need to make sure we roll all the fixes we can into 2.8
(12:17:34 PM) sarnold: would the new libaudit link -> cap_audit_write needed in PAM be worth adding to 2.8.2?
(12:18:04 PM) sbeattie: Oh, yes, some of the configury stuff around python3 is broken in 2.8, and needs a couple of cherrypicked patches.
(12:18:09 PM) jjohansen: sarnold: yes I think so, though I am surprised suse hasn't seen it already
(12:18:43 PM) sbeattie: sarnold: where I ran into it was in mostly custom emitted profiles in the ubuntu tests of the pam_apparmor stack.
(12:18:56 PM) jjohansen: also I am open to 2.8.2 picking up a few little tweaks/improvements to the build etc above strictly just bug fixes
(12:19:15 PM) jjohansen: ah right
(12:19:30 PM) sbeattie: jjohansen: do you have any particular improvements in mind?
(12:19:33 PM) sarnold: sbeattie: ohh
(12:19:33 PM) jjohansen: still I would expect if someone tried pam_apparmor on suse they would hit it
(12:20:24 PM) jjohansen: sbeattie: not at the moment, but there was discussion of cleaning up the rpm infrastructure a bit, and tweaks to the make system etc last time
(12:20:33 PM) cboltz: jjohansen: pam_apparmor is one of the things I don't use myself
(12:20:40 PM) cboltz: what exactly is broken with it?
(12:20:55 PM) jjohansen: we would have to evaluate case by case but I am open to a few tweaks like that going in
(12:21:21 PM) jjohansen: cboltz: its a pam module that can be used to put users/applications into apparmor profiles
(12:21:36 PM) cboltz: I know what it is/does ;-)
(12:22:10 PM) jjohansen: ah right, the issue is there are new rejects etc if you have auditd enabled
(12:22:16 PM) sbeattie: cboltz: nothing is broken with it; the discussion was prompted by ubuntu finally getting libaudit in main and enabled at build time for a bunch of things, which broke some tests of the pam_apparmor stack, because adding libaudit caused audit events to be written from userspace, requiring capability audit_write where it hadn't been needed before.
(12:22:33 PM) jjohansen: so Ubuntu has been using it with syslog, but hit errors with the inclusion of auditd
(12:22:46 PM) jdstrand: jjohansen: just with auditd enabled? it doesn't required a newer pam or something?
(12:23:04 PM) ***jdstrand uses pam-apparmor, but hasn't seen it on 12.04
(12:23:34 PM) sbeattie: no, it's not bound to auditd; it's the build-time enabling of the libaudit configure option.
(12:24:16 PM) jjohansen: sbeattie: well its both, as it won't use auditd if it isn't present
(12:24:49 PM) sbeattie: jjohansen: pam will still try generate the userspace audit events even if auditd is not running.
(12:25:14 PM) jdstrand: ok, that was more in line with what I was thinking
(12:26:37 PM) sbeattie: anyway, it's a bit of a diversion. At best, there might be improvements in an abstraction or two that could be made related to it.
(12:27:13 PM) jjohansen: yeah, but that is something that could definitely got into 2.8.2 as if you hit it you would call it a bug
(12:27:45 PM) jjohansen: Anyway I don't think 2.8.2 is a rush but I was thinking maybe we could get it out next month
(12:28:30 PM) jjohansen: If you see anything thats a bug, or think it would be appropriate with 3.0 still being a ways out please nominate it
(12:28:47 PM) sbeattie: Agree on 2.8.2
(12:29:19 PM) jjohansen: so we can move on
(12:29:25 PM) sbeattie: I'm happy to be the sucker^W^W^W handle that release, if you want.
(12:29:59 PM) jjohansen: sbeattie: thanks
(12:30:50 PM) jjohansen: cboltz: we have a packaging item around rpm on the agenda. Did you want to talk to that
(12:31:02 PM) cboltz: yes
(12:31:13 PM) cboltz: it's just an idea and I'd like to hear feedback
(12:31:23 PM) jjohansen: okay, go
(12:31:24 PM) cboltz: the (possible) problem is basically:
(12:31:35 PM) cboltz: a package contains an apparmor profile
(12:31:41 PM) cboltz: what should it have in its requirements?
(12:32:02 PM) cboltz: if it requires apparmor-profiles, people might complain because they want to run ping unprotected ;-)
(12:32:20 PM) cboltz: and if it does not require it, the abstractions are missing and the profile fails to load
(12:32:32 PM) cboltz: that's why I was thinking about splitting the package into
(12:32:35 PM) cboltz: a) profiles
(12:32:49 PM) cboltz: b) everything else (directory structure, abstractions, tunables etc.)
(12:33:02 PM) cboltz: what's your opinion about this?
(12:33:35 PM) mdeslaur: that's basically what we do on ubuntu
(12:34:31 PM) jjohansen: cboltz: it makes a lot of sense and I really don't know why the abstractions aren't split off from the rest of the profiles
(12:34:56 PM) jjohansen: my guess is it was just more work and we want to get something out the door
(12:35:06 PM) cboltz: probably because "nobody did it"
(12:35:45 PM) sarnold: probably paralyzed trying to come up with The Best Way to do it... I recall some talk about automatically building a giant pile of packages, one for each profile (family) and requirements to match...
(12:35:50 PM) jjohansen: cboltz: well it was our goal at one time to get all the profiles into their respective packages but that proved to be hard :)
(12:35:52 PM) cboltz: and it doesn't cause too much problems (only if a package comes with its own profile _and_ the parser is installed)
(12:36:15 PM) sarnold: cboltz: your suggestion sounds very practical to me. It's hard to see a downside, anyway. :)
(12:36:44 PM) cboltz: sarnold: well, the rpm database will grow a bit ;-)
(12:37:03 PM) sarnold: cboltz: hehe, it already tracks 27k files...
(12:37:26 PM) cboltz: jjohansen: IMHO getting the profiles into their respective packages only makes sence if the package maintainer cares about the profile
(12:37:39 PM) cboltz: otherwise it just means I have to update 20 packages instead of one ;-)
(12:38:10 PM) jjohansen: cboltz: yep, like I said it proved to be hard :)
(12:38:21 PM) cboltz: (ideally the _upstream_ maintainers should care, but that's even harder...)
(12:40:08 PM) jdstrand: ya
(12:40:32 PM) cboltz: BTW: after some discussion on the opensuse-factory ML around protecting stuff like firefox and acroread, I'm also thinking about creating an apparmor-profiles-paranoid-will-break-something package ;-)
(12:40:59 PM) sarnold: ooh
(12:41:14 PM) cboltz: (you all know that profiling applications with a "save as..." menu option never makes everybody happy)
(12:41:50 PM) cboltz: (and that's also the reason why I'm not too keen to add an acroread profile to the profiles package)
(12:42:43 PM) jjohansen: cboltz: well get them to patch the file dialog to use the new apparmor sandbox file dialog that is coming (eventually) and everyone will be happy :)
(12:43:12 PM) cboltz: that's the long-term goal, but I haven't seen this dialog yet ;-)
(12:43:28 PM) jjohansen: cboltz: hence the eventually
(12:43:41 PM) cboltz: I'm really looking forward to it, because it will solve lots of problems
(12:44:01 PM) jjohansen: yes, it will be nice to have
(12:44:51 PM) jjohansen: alright so can we get a sucker^w volunteer to do some base packaging work around this
(12:45:05 PM) jjohansen: it would make sense to have this upstream I think
(12:45:35 PM) cboltz: I'm not sure if upstream makes sense here ;-)
(12:45:45 PM) cboltz: basically I only have to split the %files in the spec
(12:46:12 PM) cboltz: so the only upstream thing that could be helpful is a README.packaging that recommends to split profiles and abstractions etc. in separate packages
(12:46:20 PM) jjohansen: well, we do ship a reference set, and currently the abstractions and profiles are treated the same
(12:46:34 PM) jdstrand: I think that is probably right
(12:46:44 PM) jdstrand: it is a referene set, it can be shipped together
(12:47:42 PM) jdstrand: I can see the benefit of the split I guess, but README.packaging sounds easier, especially since distros may not use whatever split we come up with
(12:47:53 PM) jjohansen: heh, okay I'll defer, cboltz feel free to submit patches to the README :)
(12:48:18 PM) jjohansen: might not but are lazy so are likely too
(12:48:28 PM) ***jdstrand nods
(12:48:45 PM) cboltz: I'll send a patch whenever I'm bored ;-)  (which doesn't happen too often)
(12:50:07 PM) jjohansen: Alright moving on
(12:50:26 PM) jjohansen: next meeting.  Should we move back to tuesday?
(12:50:56 PM) jjohansen: And if so March 2 or March 9th?
(12:51:04 PM) jjohansen: gah, stupid jj.
(12:51:13 PM) jjohansen: April 2 or April 9th
(12:51:51 PM) ***jjohansen would really like to have an extra February of dev time
(12:51:55 PM) jdstrand: I'd suggest 9th
(12:52:08 PM) jdstrand: but it can be either
(12:52:13 PM) jdstrand: (imho)
(12:52:13 PM) sarnold: n odifference for me
(12:52:34 PM) cboltz: I don't really care about the day, and both dates you proposed are OK for me
(12:53:00 PM) sbeattie: I'm okay with either
(12:53:11 PM) jjohansen: alright thats good enough for me
(12:53:11 PM) jjohansen: Tuesday April 9th, @20:00 UTC
(12:53:29 PM) jjohansen: does anyone have anything else they would like to discuss
(12:53:30 PM) sbeattie: would that be the intended regular date, 2nd tues of each month?
(12:53:42 PM) sbeattie: (just to try for a consistent schedule)
(12:53:47 PM) jjohansen: sbeattie: yeah that is what we have tried for
(12:54:20 PM) sbeattie: okay, couldn't rmemeber if it was 1st or 2nd tues. Works for me,
(12:54:35 PM) jjohansen: hrmm or was it first tuesday? Anyways I am good with codifying it as we try for the 2nd
(12:54:49 PM) cboltz: the 2nd will avoid April 1st 2014 ;-)
(12:55:16 PM) jjohansen: well that settles it then
(12:55:17 PM) ***jdstrand votes for 2nd tuesday, but doesn't have a strong opinion
(12:55:55 PM) ***cboltz votes for the first because he wants to do some april fool's jokes next year
(12:56:20 PM) jdstrand: heh
(12:57:53 PM) jjohansen: alright thanks for coming everyone, see you on the 9th
(12:57:58 PM) jjohansen: or sooner
(12:58:02 PM) jdstrand: thanks jjohansen! :)
(12:58:05 PM) mdeslaur: thanks jjohansen!
(12:58:07 PM) cboltz: I have two small things left ;-)
(12:58:19 PM) cboltz: one: any news on updating the wiki?
(12:58:31 PM) jjohansen: cboltz: oh, drat
(12:59:02 PM) cboltz: two: did someone write a description for a GSoC project about rewriting the tools + adding some features?
(12:59:17 PM) jjohansen: no, no news on the wiki we really do need to do that. I'll poke kees to remind me about who I need to contact
(12:59:58 PM) jjohansen: cboltz: oh drat no I haven't managed to get to that yet, sarnold have you?
(01:00:26 PM) sarnold: no, I didn't, I thought got a bit intimidated when I saw the other proposals were more than just hand-waving :)
(01:00:49 PM) jjohansen: alright I will see what I can come up with this weekend
(01:01:34 PM) jjohansen: and then I can send it to the ml for people to help with revisions
(01:01:56 PM) ***jjohansen sticks a sticky to his monitor
(01:02:22 PM) cboltz: another one? how big is your monitor? ;-)
(01:02:41 PM) jjohansen: all too small
(01:03:15 PM) cboltz: for the stickies or to display the applications?
(01:04:02 PM) jjohansen: stickies, and well I guess trying to see applications through all the stickies
(01:04:28 PM) cboltz: ;-)
(02:15:46 PM) terryh left the room (quit: Ping timeout: 480 seconds).
(03:46:45 PM) cboltz left the room (quit: ).
```
