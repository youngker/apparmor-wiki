 ```

(11:01:38 AM) jjohansen: cboltz, sarnold, sbeattie, jdstrand_, msalvatore: meeting time
(11:01:47 AM) sarnold: woo!
(11:02:16 AM) jjohansen: a willing victim^W volunteer!
(11:03:53 AM) msalvatore: hey!
(11:04:17 AM) ***sbeattie is here
(11:04:56 AM) jjohansen: okay, lets get started
(11:05:02 AM) jjohansen: there is not much on the agenda
(11:05:22 AM) jjohansen: so first up stable releases
(11:05:36 AM) jjohansen: we need to get these out asap
(11:05:50 AM) cboltz: no objections ;-)
(11:06:11 AM) jjohansen: I still have the mount fix I am iterating on, but I no longer want to hold up the releases on them, because its just taking too long
(11:06:20 AM) jjohansen: is there anything else we should get into them
(11:06:42 AM) sarnold: someone recently pinged us about some chromium profile fixes ..
Sargun sarnold 
(11:07:04 AM) jjohansen: sarnold: do we have the fixes available?
(11:07:29 AM) jjohansen: I assume they are easy but just haven't looked at that
(11:07:49 AM) sarnold: here we are.. https://gitlab.com/apparmor/apparmor-profiles/merge_requests/23
(11:08:39 AM) cboltz: sarnold: that's another construction site^W^Wtarball ;-)
(11:08:45 AM) sarnold: :D
(11:08:55 AM) jjohansen: okay, I think we can include those, I will look at it more after the meeting
(11:09:54 AM) cboltz: check the pending merge requests - some of them are backport candidates
(11:10:07 AM) jjohansen: indeed
(11:10:28 AM) jjohansen: we need to spend some time today going through the merge requests, and doing backports
(11:10:39 AM) jjohansen: I would like to start rolling the releases tomorrow
(11:10:41 AM) vidal72[m]: apparmor-prfiles aren't part of apparmor usrerspace tool so they shouldn't block its release
(11:11:16 AM) jjohansen: vidal72[m]: true
(11:11:50 AM) jjohansen: though that does being up an interesting point, we have never done a "release" of apparmor-profiles
(11:12:13 AM) jjohansen: do we want to?
(11:12:27 AM) jjohansen: and if so should it be synchronized with the userspace releases
(11:13:19 AM) sbeattie: probably?
(11:13:41 AM) sbeattie: I'm not sure how synchronized they need to be with the userspace tools.
(11:13:47 AM) cboltz: Only if Ubuntu or Debian want to do maintenance updates whenever we do a release. Without that, it's probably pointless.
(11:14:23 AM) cboltz: sbeattie: the only "contact point" with the apparmor tarball are the abstractions, the tools etc. shouldn't matter
(11:14:32 AM) sbeattie: true
(11:14:37 AM) jjohansen: cboltz: I'll take that to mean suse doesn't
(11:15:04 AM) vidal72[m]: "This repository contains AppArmor profiles that are currently in development. Once a profile reaches maturity, it gets removed from this repository and moved into a distribution's official packaging."
(11:15:40 AM) cboltz: packaging them in an "apparmor-profiles-experimental" package (or, for the fun of it, "apparmor-profiles-ubuntu"?) is on my TODO since a long time, but nothing done yet
(11:15:54 AM) jjohansen: vidal72[m]: I am not sure that is true anymore
(11:16:06 AM) cboltz: therefore I don't have a need for a tarball (yet) ;-)
(11:16:12 AM) sbeattie: intigeri has been packaging them in debian, which ubuntu has inherited
(11:16:24 AM) jjohansen: regardless we can skip doing a release this time around
(11:16:32 AM) jjohansen: that is a profiles release
(11:17:04 AM) sbeattie: yeah, fair enough, but long term we should decide if we want to; depends on if we want distros shipping them.
(11:17:31 AM) sbeattie: want to encourage, that is
(11:17:42 AM) cboltz: "profile repo" is probably a bigger topic, but I'm not sure if it makes sense to discuss that today
(11:18:29 AM) cboltz: (like "does it make sense to have some profiles in apparmor and some in apparmor-profiles")
(11:19:22 AM) sbeattie: jjohansen: did we backport the optimization caching fix?
(11:19:26 AM) cboltz: (and yes, I agree that distros should ship more profiles - but that obviously needs some testing and probably a few additional rules)
(11:19:54 AM) jjohansen: sbeattie: no
(11:20:18 AM) sbeattie: does it make sense to? 
(11:20:32 AM) jjohansen: sbeattie: jdstrand_ cherry-picked it for Ubuntu, but we have not in the upstream project
(11:20:35 AM) ***sbeattie wanted to add a testcase for the issue, too
(11:20:39 AM) jjohansen: that was an open question
(11:21:06 AM) jjohansen: I am unsure whether it is worth backporting, it doesn't affect suse
(11:21:16 AM) jjohansen: I still need to check debian
(11:21:39 AM) sarnold: at this point ni their freeze they may not be willing to take it anyway
(11:21:53 AM) jjohansen: right
(11:22:35 AM) jjohansen: debian have very much expressed a desire that we be more strict  with what we backport
(11:24:34 AM) jjohansen: still it is a fix, my only concern with backporting is that it won't have the subsequent patches that don't exist yet
(11:26:54 AM) jjohansen: if you have an opinion please comment on https://gitlab.com/apparmor/apparmor/merge_requests/385
(11:27:10 AM) jjohansen: we have to make a decision later today
(11:27:40 AM) jjohansen: anything else for the stable releases?
(11:27:56 AM) cboltz: well, (open)SUSE doesn't need it and Debian probably won't take it because of the feature freeze, so I don't see why you should backport it ;-)
(11:28:33 AM) cboltz: yes, some of the pending merge requests (especially the profile and abstraction updates)
(11:28:58 AM) ChibaPet left the room (quit: Quit: leaving).
(11:29:28 AM) sarnold: done and done
(11:29:33 AM) jjohansen: well, while ubuntu, debian, and suse are our primary users they aren't the only, the question becomes whether we want the fix in any of our stable releases, or just on dev head
(11:30:05 AM) jjohansen: it might make sense for example for the fix to be in 2.13 and not backported to previous stable
(11:31:01 AM) cboltz: I'd say either 2.12 and 2.13, or none of them since 2.12 and 2.13 are very close
(11:31:49 AM) sbeattie: jjohansen: I could live with 2.13 only.
(11:31:52 AM) finsternis left the room (quit: Ping timeout: 480 seconds).
(11:32:09 AM) jjohansen: well the split being 2.13 is current release, everything else is maintenance
(11:33:54 AM) jjohansen: okay lets move on
(11:34:09 AM) jjohansen: translations are next up
(11:34:59 AM) jjohansen: we haven't run a release since, sbeattie did some work to improve the sync, so I would like to delay discussing until we have actually tried doing it with the new sync
(11:35:19 AM) jjohansen: any objections?
(11:35:39 AM) sbeattie: jjohansen: ack, some commits have come in, and I haven't merged them into trunk
(11:36:08 AM) sbeattie: sorry for that
(11:36:31 AM) cboltz: so your plan is to do a manual sync from time to time?
(11:37:08 AM) jjohansen: cboltz: always, translations have never had automatic commit to the main repo
(11:37:17 AM) sbeattie: I mean, I didn't let the launchpad translations do direct commits to master in the past, so it's not a change.
(11:37:25 AM) jjohansen: they sync automatically from the main repo, but not to
(11:37:55 AM) cboltz: ok
(11:38:32 AM) jjohansen: the issue we were having that sbeattie fixed was that with the move to git, the translations branch wasn't automatically syncing to the main repo anymore
(11:39:08 AM) finsternis [~Y@00026e28.user.oftc.net] entered the room.
(11:39:17 AM) jjohansen: sbeattie: can you get those synced today?
(11:39:48 AM) jjohansen: if not I will see what I can do tongith
(11:39:52 AM) jjohansen: tonight even
(11:39:58 AM) sbeattie: jjohansen: yeah, I can work on that.
(11:40:01 AM) jjohansen: thanks
(11:40:27 AM) jjohansen: lets move on, does anyone have anything they would like to discuss
(11:42:05 AM) cboltz: I'm just diffing profiles and abstractions between 2.13 and master, there are some (small) backport candidates
(11:42:30 AM) cboltz: do you want clean cherry-picked commits, or can I just do a "backport stuff from master" commit?
(11:43:08 AM) jjohansen: cboltz: clean picks are a lot nicer
(11:43:32 AM) cboltz: I know, but they are also more work ;-)
(11:44:12 AM) jjohansen: they are greatly preferred but, getting the work done is even more important, so if the difference means work or no. go with getting work done
(11:44:13 AM) sbeattie: +1 on clean cherrypicks please, they make tracking down the reasons why if need be easier.
(11:44:22 AM) jjohansen: indeed
(11:44:42 AM) cboltz: ok, I'll see where I end up after cleaning up the diff
(11:45:27 AM) cboltz: (skipping all the "#include if exists <abstractions/foo.d/>" makes the diff much smaller)
(11:47:02 AM) Talkless left the room (quit: Quit: Konversation terminated!).
(11:47:11 AM) jjohansen: anything else?
(11:47:42 AM) sarnold: nothing from me
(11:47:52 AM) sbeattie: I don't have anything
(11:47:55 AM) cboltz: not from me, better spend your time on reviewing the acked-by-timeout merge requests ;-)
(11:48:04 AM) msalvatore: nothing from me
(11:48:28 AM) jjohansen: okay then
(11:48:34 AM) jjohansen: meeting adjourned
(11:48:37 AM) jjohansen: thanks everyone
 ```