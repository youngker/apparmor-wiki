 ```
(11:01:54 AM) cboltz: meeting time ;-)
(11:02:02 AM) jjohansen: yep
(11:02:59 AM) ***sbeattie is here
(11:03:07 AM) jjohansen: sarnold, sbeattie, jdstrand_ meeting time
(11:03:16 AM) jjohansen: err, who else is around
(11:05:36 AM) jjohansen: so lets get started
(11:05:53 AM) jjohansen: there is nothing on the agenda!
(11:06:12 AM) jjohansen: does anybody have anything they would like to bring up
(11:06:28 AM) cboltz: hmm, release dates? *g,d&r*
(11:06:52 AM) jjohansen: how did I know that was coming
(11:06:58 AM) jdstrand_: jjohansen: I'm going to have to skip
(11:07:46 AM) jjohansen: jdstrand_: oh I figured that would be the case, just poking you to give you better options :)
(11:08:10 AM) jjohansen: can't say we didn't try
(11:08:16 AM) jdstrand_: hehe
(11:08:30 AM) sarnold: omg didn't we already have a meeting this year?
(11:08:44 AM) cboltz: ;-)
(11:09:28 AM) jjohansen: sarnold: uhmmm, maybe. I mean if you count all the meetings that have been canceled ... :)
(11:09:40 AM) jjohansen: alright back on topic
(11:10:23 AM) jjohansen: So we can release 2.13.5 this week if there isn't anything specific people want to get in
(11:11:02 AM) jjohansen: 3.0 - is under going testing in ubuntu, and that has turned up a few issues
(11:11:13 AM) jjohansen: like the warnings being ownerous
(11:11:38 AM) jjohansen: that lead to the patch series to control warning last week
(11:13:14 AM) jjohansen: there is the a couple outstanding fixes for v8 networking to land, and there are v8 unix rule changes that would be nice to land but could be part of a 3.1 release
(11:14:10 AM) jjohansen: at this point I am thinking the v8 unix is going to have to be 3.1
(11:14:25 AM) jjohansen: just so we can get 3.0 out the door
(11:14:37 AM) sbeattie: ack
(11:14:56 AM) cboltz: agreed
(11:16:23 AM) jjohansen: the other big wi is release notes, 2.13.5 just needs a few items added, 3.0 uhmm is a fair bit of work still
(11:16:47 AM) jjohansen: not that we couldn't release without full release notes its just not good form
(11:17:03 AM) jjohansen: and writing release notes is uhmmm, not priority for me atm
(11:17:50 AM) jjohansen: sorry I just have other more pressing things that I am running out of time on
(11:18:26 AM) zyga [~zyga@178.182.246.167.nat.umts.dynamic.t-mobile.pl] entered the room.
(11:18:39 AM) cboltz: would it help if I write the release notes up to the originally planned release date for 3.0, and you do the remainder?
(11:19:04 AM) jjohansen: hahaha
(11:20:27 AM) cboltz: damn, you noticed that this was a trick question ;-)
(11:20:41 AM) jjohansen: cboltz: if you are amenable to us releasing we bare bone release notes, https://gitlab.com/apparmor/apparmor/-/wikis/Release_Notes_3.0
(11:21:16 AM) jjohansen: basically some hightlighted features and commit ranges, we can probably get a release out this or next week
(11:21:20 AM) cboltz: "AppArmor 3.0 was released 2020-06-07." - really? ;-)
(11:22:12 AM) cboltz: does that also mean that the release notes have notes about everything up to that date?
(11:22:36 AM) jjohansen: well it was a target for me, I just missed the bulls eye, a tiny little bit
(11:22:49 AM) jjohansen: cboltz: no
(11:23:11 AM) cboltz: yeah, what are a few months compared with 2012 vs. 2020? ;-)
(11:24:06 AM) jjohansen: exactly!
(11:24:18 AM) jjohansen: we are close
(11:24:51 AM) jjohansen: I am will get the network stuff up today
(11:25:42 AM) jjohansen: alright anything else?
(11:26:08 AM) cboltz: I have a backport question for 2.13 - with expected answer "no" ;-)
(11:26:23 AM) cboltz: it's about the postfix profiles (in extra)
(11:26:23 AM) jjohansen: cboltz: no
(11:26:26 AM) jjohansen: that was easy
(11:26:36 AM) jjohansen: cboltz: okay, go
(11:27:22 AM) jjohansen: what do you want to do with the postfix profiles in extra
(11:27:23 AM) cboltz: basically they were completely changed ("/usr/lib/postfix/foo" -> "profile postfix-foo /usr/lib/postfix/{bin/,sbin/,}foo", even the filenames changed to "postfix-foo")
(11:27:50 AM) jjohansen: yeah
(11:28:00 AM) cboltz: openSUSE needs the new profiles because the postfix-* binaries now live in /usr/lib/postfix/bin/
(11:28:30 AM) cboltz: but I also understand that such a change is "a bit" too big for a maintenance release, and I'm willing to do the backport as openSUSE-only patch
(11:28:38 AM) jjohansen: okay
(11:28:42 AM) jjohansen: so
(11:28:45 AM) cboltz: unless you completely surprise me and tell me that such a change is ok for 2.13 ;-)
(11:29:26 AM) jjohansen: 1. profiles in extras are considered reference. So that can be taken two ways
(11:30:28 AM) jjohansen: either, its only a best effort we are not dropping huge changes OR people downstream shouldn't care about them too much so lets drop in big changes
(11:31:22 AM) jjohansen: 2. when it comes to backports, we try to stick with bug fixes UNLESS one of the down streams requests the change a provides a reason
(11:31:39 AM) jjohansen: suse is a downstream, suse has provided a reason
(11:32:15 AM) jjohansen: its not just trying to keep it the same as what is in dev head
(11:32:49 AM) jjohansen: so while I don't particularly like such a large change, I am not opposed to it
(11:33:16 AM) jjohansen: cboltz: the only caveat is get it in this week so we can release 2.13.5
(11:34:38 AM) cboltz: that should be doable (even if I foresee to pick *lots of* cheries) - and if I can't make it, dont wait for me - I can still add a patch to the package ;-)
(11:35:36 AM) jjohansen: Is there anything else to discuss?
(11:35:57 AM) cboltz: policy filenames
(11:36:11 AM) jjohansen: oh that one is ugly
(11:36:23 AM) cboltz: with recommending named profiles, should we use the "nice" name also for the profile file
(11:36:41 AM) jjohansen: generally speaking, a nice name would be preferrable
(11:36:41 AM) cboltz: I'm more looking at _new_ profiles, renaming existing profiles in the apparmor.d directory would be a pain
(11:37:00 AM) cboltz: my actual question is about the php-fpm profile, which is new in 3.0
(11:37:07 AM) jjohansen: but practically, moving profiles to new filenames is a huge pita
(11:37:36 AM) jjohansen: cboltz: in that case new profiles, I whole heartedly support having nice names
(11:37:48 AM) jjohansen: nice filenames
(11:38:31 AM) jjohansen: I hope that we will eventually get all profiles moved to nice filenames just not in apparmor 3
(11:40:45 AM) jjohansen: oh 1 more thing cboltz I won't hold up the 3.0 release for that change ...
(11:40:45 AM) cboltz: as you already said - renaming profile files is a pita
(11:40:46 AM) jjohansen: :P
(11:40:59 AM) cboltz: https://gitlab.com/apparmor/apparmor/-/merge_requests/620 ;-)
(11:43:20 AM) jjohansen: hahah, I should have know
(11:44:02 AM) jjohansen: cboltz: anything else?
(11:44:28 AM) cboltz: I don't think so
(11:44:51 AM) jjohansen: okay,
(11:44:51 AM) jjohansen: meeting adjourned.
(11:44:51 AM) jjohansen: thanks everyone
 ```
