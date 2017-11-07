```
(12:01:25 PM) kshitij8: \o
(12:01:26 PM) sarnold: nabend cboltz :)
(12:02:05 PM) tyhicks: hello
(12:02:32 PM) cboltz: sarnold: just curious - when you write about "a fun weekend project for someone interested in learning C better. :)", do you have a specific person in mind? ;-)
(12:02:33 PM) sarnold: cboltz: I didn't get any grapes on my vine last summer after a fairly brutal pruning last winter; do grapes need to grow on 'old wood'? will I get grapes this summer? Or did I kill the vine? :)
(12:02:53 PM) sarnold: cboltz: no, just that sometimes people look for littl eprojects.. :)
(12:03:43 PM) cboltz: ;-)
(12:03:50 PM) kshitij8: sounds interesting.
sbeattie sbeattie_ 
(12:04:47 PM) jjohansen: sarnold, sbeattie, jdstrand, tyhicks, mdeslaur, cboltz, kshitij8: meeting time
(12:05:02 PM) kshitij8: sarnold: how many hours does a normal weekend have?
(12:05:52 PM) cboltz: I'm not sarnold, but - friday afternoon till sunday late night, so at least 36 hours ;-)
(12:06:08 PM) sarnold: kshitij8: not nearly enough :)
(12:06:44 PM) jdstrand: hey
(12:06:52 PM) jjohansen: 0. Its a trick question, there is no such thing as the mythical weekend
(12:07:33 PM) kshitij8: not clock hours, I meant "effective hours" per your "weekend project" guidlines :P Mine has only 12 if I'm not too sleepy.
(12:07:44 PM) kshitij8: jjohansen nails it though :D
(12:10:00 PM) jjohansen: alright shall we get started? The Meeting agenda (such as it is) is at http://wiki.apparmor.net/index.php/MeetingAgenda#Next_Meeting:_IRC_Meeting.2C_20:00_UTC.2C_Tuesday_Jan_19_2016
(12:10:21 PM) jjohansen: so first up 2.9.3, and 2.10.1
(12:10:37 PM) jjohansen: both of these are needed, we have pending patches for them
(12:10:43 PM) jjohansen: but they haven't happened
(12:11:54 PM) jjohansen: I'd like to see them happen soon, instead of getting pushed off more
(12:11:58 PM) sbeattie: ugh, sorry about that
(12:12:15 PM) sbeattie: I'll add some time to my schedule to release those.
(12:12:21 PM) jjohansen: meh, I could have done the release as well, ...
sbeattie sbeattie_ 
(12:12:27 PM) jjohansen: sbeattie: thanks
(12:12:46 PM) cboltz: sbeattie: please include some time to review the pending patches
(12:13:17 PM) cboltz: (a quick grep shows that 10 of my patches are also for 2.10 and/or for 2.9)
(12:14:08 PM) jjohansen: yeah we do have quite the backlog of pending patches to get reviewed, it would be good if we can each make some time and get at least one reviewed
(12:14:42 PM) jjohansen: alright moving on
(12:15:03 PM) cboltz: one? I have 27 patches pending ;-)
(12:15:32 PM) jjohansen: caching update: last month we discussed cache and aa-sha1. There has been no progress on those actions items
(12:15:35 PM) sarnold: it'd be 24 if sbeattie, jjohansen, and I cuold each review one :) haha
(12:16:06 PM) jjohansen: cboltz: yeah I know, but 1 is better than the current 0 that seems to be happening
(12:16:15 PM) cboltz: indeed ;-)
(12:16:23 PM) ***sbeattie has some patches that need to make it off of his laptop
(12:16:29 PM) jjohansen: cboltz: and it was at least, so more would be possible
(12:16:42 PM) jjohansen: yeah, I have some more too
(12:16:44 PM) kshitij8: cboltz: you ought to bring better/newer offers for patch reviews
(12:17:02 PM) jjohansen: kshitij8: don't encourage him
(12:17:10 PM) kshitij8: ;-)
(12:17:51 PM) jjohansen: alright next topic
(12:18:15 PM) cboltz: I can probably offer a new patch series soon - FileRule ;-)
(12:18:18 PM) jjohansen: when to move the apparmor development repo to git
(12:18:47 PM) kshitij8: are there any known blockers for this movement?
(12:19:14 PM) jjohansen: no
(12:19:21 PM) tyhicks: it'll slow the workflow of some folks who don't use git daily
(12:19:42 PM) ***jjohansen and tyhicks are already using git through a translation layer to deal with the repo
(12:19:57 PM) cboltz: kshitij8: I'd finally have to learn git ;-)  (more than the pull/commit/push I use nowadays)
(12:19:59 PM) jjohansen: it has its issues and is fairly brittle but it works
(12:20:26 PM) sarnold: tyhicks: it might be more than made up for by you and jj moving faster without dealing with constant bzr-git troubles :)
(12:20:38 PM) jjohansen: the only issue, is developers willing to make the switch
(12:20:38 PM) tyhicks: possibly
(12:20:42 PM) kshitij8: git is fairly easy [per my limited usage], plus very similar to bzr
(12:20:59 PM) kshitij8: cboltz: you
(12:21:11 PM) kshitij8: err you will find branches nice
(12:22:09 PM) cboltz: maybe, but let me first make sure I understand a bit more than the basic commands before entering that area ;-)
(12:22:14 PM) kshitij8: is there a git-bzr layer? ;-)
(12:22:30 PM) tyhicks: there is but we should avoid that in this discussion
(12:22:54 PM) sbeattie: so, my half-baked plan was to start the process by the converting the profiles-repository first, then move the main repo 
(12:22:56 PM) cboltz: I'd guess "just use the basic commands" comes close to bzr, so... ;-)
(12:22:59 PM) jjohansen: kshitij8: it is brittle, and I wouldn't recommend it
(12:23:18 PM) ***jdstrand notes he is willing to change to git when others are (ie, don't block on me)
(12:23:32 PM) cboltz: sbeattie: sounds like a good idea
(12:23:51 PM) cboltz: while at it, should we also switch to the proposed cross-distro repo layout?
(12:23:52 PM) sbeattie: that way, people would have an opportunity to gain experience with git without it being required for the main repo.
(12:24:17 PM) jjohansen: okay, how about we convert the profile repo this week and see how that goes for people
(12:24:19 PM) cboltz: that would mean people get a new location only once (bzr -> git) and not twice (bzr -> git -> moving files around)
(12:24:20 PM) tyhicks: cboltz: seems unrelated
(12:24:34 PM) kshitij8: the repos will still be hosted at the same place right?
(12:24:40 PM) sbeattie: cboltz: I've recycled the brain cells (or never had them to begin with) that knew what the new layout was to be.
(12:24:55 PM) sbeattie: kshitij8: yeah. launchpad has gained support for git hosting.
(12:24:56 PM) tyhicks: kshitij8: yes - still launchpad but the path to the repo will be slightly different
(12:25:12 PM) cboltz: sbeattie gobby.debian.org debconf15/bof/AppArmor
(12:25:18 PM) kshitij8: ok. nice to hear that.
(12:25:57 PM) kshitij8: what is the effort to convert from bzr to git? does launchpad have tools to clone the commit tree or something?
(12:26:52 PM) jjohansen: launchpad provides support to convert, it should be fairly easy
(12:27:48 PM) tyhicks: what would we lose?
(12:28:03 PM) tyhicks: 1) some devs may be slowed by the change
(12:28:11 PM) jjohansen: hopefully nothing
(12:28:20 PM) tyhicks: 2) is there equivalent support for bzr --fixes ?
(12:30:04 PM) tyhicks: I don't think there's a --fixes equiv, so we'll have a slight bit of added overhead to update bug status
(12:30:15 PM) kshitij8: so we'd probably loose the metadata I think
(12:30:26 PM) tyhicks: I'm fine with the added overhead
(12:30:37 PM) tyhicks: (it'll be offset in other areas for me)
(12:30:39 PM) sbeattie: tyhicks: sadly --fixes doesn't actually update the bug status; it just references the repo in the bug report.
(12:31:01 PM) tyhicks: sbeattie: oh, right - I always forget that
(12:31:05 PM) tyhicks: ok
(12:31:06 PM) sbeattie: it's not like the bug autoclose when packages land in the ubuntu archive
(12:31:28 PM) sbeattie: that said, they're highly visibile when reviewing via bzr qlog, which is something I'll miss.
(12:31:53 PM) jjohansen: kshitij8: actually we should be able to keep most of the meta data. Tags, etc should be preserved
(12:32:22 PM) tyhicks: this page specifically says that tags will be preserved if using the described method: https://help.launchpad.net/Code/Git#Converting_from_Bazaar_to_Git
(12:33:26 PM) kshitij8: "Git Fast-import does not support metadata properties" per SO
(12:34:44 PM) tyhicks: kshitij8: outside of the 'fixes bug:' metadata, what else did you have in mind?
(12:35:13 PM) kshitij8: I was looking at that when I came across the quote
(12:35:46 PM) sbeattie: tyhicks: is git able to differentiate between the author of a patch and the committer? 
(12:35:56 PM) kshitij8: Apparently bugzilla also ahd to do something for it: https://wiki.mozilla.org/Bugzilla:Migrating_to_git#Plan
(12:36:04 PM) kshitij8: s/ahd/had/
(12:36:15 PM) kshitij8: yes git is able to
(12:36:16 PM) sbeattie: (e.g. will we lose some of the metadata when someone used --author on a commit)
(12:36:46 PM) sarnold: fwiw I don't think I ever used --author when checking in merges from folks..
(12:36:51 PM) tyhicks: sbeattie: I suspect that we will lose that bit of metadata, as well
(12:37:21 PM) sbeattie: sarnold: not needed for merges, that's handled correctly, it's if you manually took someone's patch, applied it, and committed it.
(12:37:23 PM) tyhicks: in git, you typically denote the committer by adding your Signed-off-by at the bottom of the commit message
(12:37:29 PM) jjohansen: http://stackoverflow.com/questions/14518594/migrate-bazaar-to-git-with-commit-properties-to-link-bugs-fixed
(12:38:01 PM) sbeattie: Anyhoo, I don't think we need to hash out all the details here.
(12:38:02 PM) kshitij8: yeah my quote from that SO link. but its pretty old
(12:38:07 PM) jjohansen: created a script to include bug-ids in the commit message
(12:38:55 PM) tyhicks: we all typically include the link in the commit message already
(12:39:00 PM) jjohansen: yeah, I am sure there are some limitations but we should be able to preserve most everything
(12:39:20 PM) jjohansen: tyhicks: true
(12:40:33 PM) tyhicks: I think we should follow through with the plan to move the profiles-repository over to git asap
(12:40:51 PM) kshitij8: yeah we'll figure out the caveats in that process
(12:41:38 PM) jjohansen: yeah
sbeattie sbeattie_ 
(12:42:25 PM) jjohansen: sbeattie: did you want to take that as well? Or should I claim it
(12:43:20 PM) sbeattie: jjohansen: probably best if I take that
(12:43:30 PM) jjohansen: okay, thanks
(12:43:45 PM) cboltz: it might be a good idea to handle the merge requests first - https://code.launchpad.net/apparmor-profiles/+activereviews
(12:44:00 PM) jjohansen: yes it would
(12:44:08 PM) jjohansen: moving on, does anyone have anything else they would like to bring up
(12:44:44 PM) tyhicks: sbeattie: I may be able to cover that conversion, as well
(12:45:22 PM) tyhicks: jjohansen: I don't have anything else except to mention that I am trying to circle back to the stacking interfaces thread
(12:45:28 PM) tyhicks: probably tomorrow
(12:46:35 PM) jjohansen: tyhicks: ack, we can argue about that when you manage to circle back to it
(12:46:54 PM) sarnold: hehe
(12:46:54 PM) jjohansen: well I will take that as a no
(12:47:15 PM) jjohansen: Should the next meeting be Feb 9, or March 8
(12:47:34 PM) sarnold: feb 9 seems soo close
(12:47:37 PM) jjohansen: I am leaning towards March as the 9th isn't that far a way
(12:47:57 PM) cboltz: feb 9 is the last carnival day, so... ;-)
(12:48:53 PM) ydev [~ydev@c-71-198-46-216.hsd1.ca.comcast.net] entered the room.
(12:49:20 PM) arekm: Failed to change_hat to 'HANDLING_UNTRUSTED_INPUT' apparmor
(12:49:25 PM) arekm: damn, and nothing in audit log ;/
(12:49:54 PM) cboltz: arekm: I'd guess the apache profile isn't loaded
(12:50:05 PM) jjohansen: cboltz: yes but does that mean you need an excuse to miss the carnival, or are using the carnival as an execuse to miss the meeting
(12:50:27 PM) cboltz: it's an excuse to miss the meeting ;-)
(12:50:35 PM) cboltz: (what else?)
(12:50:38 PM) sarnold: or better yet, both, and just go get some doener? :)
(12:50:49 PM) ydev left the room (quit: Remote host closed the connection).
(12:51:01 PM) ydev [~ydev@c-71-198-46-216.hsd1.ca.comcast.net] entered the room.
(12:51:04 PM) jjohansen: alright March 8 it is
(12:51:29 PM) jjohansen: if anyone has problems with that date, let us know here or on the mailing list
(12:51:37 PM) cboltz: did you look at my calender? I'm away on that evening ;-)
(12:51:38 PM) jjohansen: thanks everyone meeting adjourned
(12:52:15 PM) jjohansen: cboltz: how did you know? :-)
(12:52:28 PM) cboltz: ;-)
(12:52:43 PM) jjohansen: cboltz: propose a different date?
(12:52:50 PM) cboltz: the other days in that week would work
(12:52:59 PM) cboltz: or the week before
(12:53:37 PM) jjohansen: Alright how about Tuesday March 1 or Wed March 9?
(12:53:46 PM) jjohansen: anyone have issues with those
(12:53:49 PM) cboltz: both work for me
(12:54:29 PM) sarnold: bo5th fine here
(12:54:47 PM) jjohansen: well lets run with March 1 then, since we are skipping February
(12:54:50 PM) jjohansen: thanks again everyone
(12:55:46 PM) arekm: cboltz: /usr/sbin/httpd (enforce)
(12:55:50 PM) arekm: cboltz: so something else
(12:55:54 PM) cboltz: FYI: openSUSE conference is June 27-30 in N?rnberg - just in case you need a reason to visit germany ;-)
(12:56:20 PM) cboltz: is the HANDLING_UNTRUSTED_INPUT hat also loaded?
(12:56:39 PM) cboltz: (aa-status should report it as /usr/sbin/httpd//HANDLING_UNTRUSTED_INPUT)
(12:56:39 PM) sarnold: thanks jjohansen :)
``` 
