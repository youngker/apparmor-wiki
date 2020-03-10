 ```
(11:01:48 AM) jjohansen: sarnold, sbeattie, jdstrand, msalvatore, jmbl, cboltz, tyhicks: if you can make it, its meeting time
(11:01:58 AM) cboltz: hi jjohansen 
(11:02:11 AM) jjohansen: intrigeri has already replied that he can't make it
(11:02:45 AM) ***sarnold rubs eyes
(11:03:40 AM) jmbl: hi :-)
(11:04:17 AM) sarnold: hey jmbl :)
(11:04:24 AM) jmbl: hey sarnold :-)
(11:06:14 AM) jjohansen: I doubt we are going to get sbeattie, or tyhicks today
(11:06:50 AM) jjohansen: so lets gt started
(11:07:25 AM) sbeattie: eh, I am here.
(11:07:41 AM) msalvatore: hey
(11:10:09 AM) jjohansen: the agenda such as it is, is here https://gitlab.com/apparmor/apparmor/-/wikis/MeetingAgenda
(11:11:11 AM) sarnold: nice
(11:11:18 AM) jjohansen: For policy reorg I have a wip on the changes, but its not ready to push so I don't know that there is much to discuss
(11:12:43 AM) cboltz: same for /usr/etc/ - we already decided on @{etc_ro} and @{etc_rw}, now we only need someone with time to implement it ;-)
(11:13:01 AM) jjohansen: the idea is that we have 3 directories with their content as commits following history
(11:13:01 AM) jjohansen:   /experimental     <- this is what is in apparmor/profiles/extra
(11:13:15 AM) jjohansen: err not /experimental just experimental/
(11:13:45 AM) jjohansen:   policy/    <- similar to what already exists in the pull request
(11:14:35 AM) jjohansen:    externally_packaged            <- trying to track profiles in external packages and contact info etc
(11:15:06 AM) jjohansen:   externally_packaged    with break down into per distro and then per package directories
(11:15:08 AM) jjohansen: ie.
(11:15:24 AM) jjohansen:   externally_packaged/
(11:15:24 AM) jjohansen:       ubuntu/
(11:15:31 AM) jjohansen:       firefox/
(11:15:34 AM) jjohansen: gah,
(11:15:39 AM) jjohansen:            firefox/
(11:15:44 AM) jjohansen:       suse/
(11:15:50 AM) jjohansen:               firefox/
(11:16:09 AM) jjohansen: assuming a distro has packaged up some variant
(11:16:57 AM) jjohansen: suggestions welcome, but if you want to wait for an updated MP to comment there ...
(11:17:39 AM) cboltz: so basically $distro/$packagename/ for externally packaged profiles? Looks good
(11:18:42 AM) jjohansen: cboltz: yeah
(11:19:03 AM) jjohansen: I haven't been able to come up with something better
(11:19:24 AM) jjohansen: we may drop $distro and just go with $packagename
(11:19:45 AM) jjohansen: for some packages if the profile being shipped is the same for multiple distros
(11:20:18 AM) sdeziel: how about using $distro = common
(11:20:30 AM) jjohansen: oh that would work
(11:20:49 AM) jjohansen: yeah, we can definitely do that
(11:21:05 AM) cboltz: and it also works if two distros ship completely different profiles for $packagename - $distro instead of "common" in that case
(11:21:19 AM) jjohansen: right
(11:21:39 AM) cboltz: (and maybe ask the involved people to merge their profiles ;-)
(11:22:22 AM) jjohansen: yes that would be nice, but at a minimum we will try to keep them up to date, and have a central place for contact info
(11:22:43 AM) jdstrand: jjohansen: do I recall a concept of tags for say, ubuntu foo vs ubuntu bar where foo and bar correspond to releases?
(11:22:52 AM) jjohansen: so if we make changes we can more easily forward the patches to those packages mainatiners
(11:23:13 AM) jjohansen: jdstrand: yes, that would be for what is in the policy/
(11:23:26 AM) jjohansen: that will match up with the current MP
(11:23:49 AM) ***jdstrand nods
(11:23:57 AM) jjohansen: the only question there would be around whether to keep the lazy history import I did in the current MP
(11:24:29 AM) jjohansen: or to fully weave the history of the two separate branches
(11:24:51 AM) jjohansen: which requires rewriting history of the repo
(11:25:48 AM) cboltz: in theory, "git mv" should also keep the history - you'll just need to use   git log --follow
(11:26:33 AM) cboltz: which sounds more sane than rewriting history of a public repo
(11:28:05 AM) jjohansen: cboltz: uhmm, not exactly when bringing two separate repo histories together, take a look at the current MP
(11:28:05 AM) jjohansen: I have commits doing mv of the current, repo. But the profiles history then gets dropped in as a giant chunk
(11:28:05 AM) jjohansen: the histories are not woven together, but kind of separate entities
(11:28:53 AM) jjohansen: I don't think it really matters, keeping the histories as they are currently is more historically accurate
(11:29:23 AM) jjohansen: but might make it harder to find some changes in history if you are not familiar with the fact that the merge happened
(11:30:07 AM) jjohansen: okay lets move on
(11:30:16 AM) cboltz: I hope (didn't check) your first commit message of the profiles that moved to the other repo include something like "move profile $profile from $oldrepo to $newrepo"
(11:30:27 AM) cboltz: that should be good enough if we need to dig into the history
(11:30:37 AM) jjohansen: yeah there is something like that
(11:31:02 AM) jjohansen: 3.0 - alphas
(11:31:26 AM) jjohansen: sorry these are stalled out, I am still hoping to get something out this month
(11:31:59 AM) jjohansen: anyone who wants to pick up a remaining work item, feel free to ping me
(11:32:13 AM) jjohansen: moving on
(11:32:37 AM) jjohansen: we have a new issue that was brought up in https://lwn.net/Articles/812719/
(11:32:42 AM) jjohansen: and asked about here
(11:32:51 AM) jjohansen: so yes, apparmor has the same problem
(11:33:04 AM) jjohansen: I am working on a kernel fix
(11:33:16 AM) jjohansen: the goal is to get it into linux-next this week
(11:33:18 AM) JanC left the room (quit: Remote host closed the connection).
(11:34:02 AM) jjohansen: userspace will already work if built against the new kernel supporting CAP_PERFMON
(11:34:38 AM) jjohansen: however userspaces built against older kernels will not work with CAP_PERFMON if the user drops in the new kernel
(11:34:50 AM) JanC [~janc@0001178b.user.oftc.net] entered the room.
(11:34:50 AM) jjohansen: we do have a plane to fix this
(11:34:56 AM) cboltz: mostly, I'll need to add it in the tools
(11:35:48 AM) jjohansen: 1. The parser (I won't speak for cboltz on the tools) will pickup a static list of caps
(11:36:18 AM) jjohansen: 2. We will still autogenerate a list
(11:37:02 AM) jjohansen: 3. we will compare the static and autogenerated lists, so we can get a warning of changes, and also be able to update the static list
(11:37:35 AM) jjohansen: the purpose of the static list is to make sure we don't drop cap support if built against a kernel that doesn't have the latest caps
(11:37:55 AM) jjohansen: 4. The feature abi exports supported caps of the running kernel
(11:38:59 AM) jjohansen: the parser will pick this up and use it, so even if the static list doesn't support the cap, users will still be able to use it
(11:39:36 AM) jjohansen: this will of course mean their policy could break if they reboot into an older kernel but is the best we can do to support new caps
(11:39:51 AM) jjohansen: without updating the userspace
(11:40:34 AM) cboltz: the tools don't have a list of capabilities (yet?) and just accept any capability names. OK, saves me the work to update the list ;-)
(11:40:41 AM) jjohansen: 5. there is a possibility we can support the new cap in the feature files of apparmor 3 so policy won't break, on kernel rebuild but that is still to be investigate
(11:40:47 AM) cboltz: (for network rules, there's a fixed list of allowed keywords)
(11:40:58 AM) jjohansen: well that is one way to go about it :)
(11:41:38 AM) cboltz: not the best way because the tools will happily accept mis-typed capabilities - but for now, less work for me ;-)
(11:41:46 AM) jjohansen: the plan is to at a minimum backport the static caps list to older releases so that they can pickup support for the new capability when rebuilt
(11:42:35 AM) jjohansen: I hope that covers any questions people might have around the issue
(11:42:55 AM) jjohansen: the goal is to still fail closed
(11:43:37 AM) jjohansen: but abis can be used to remain compatible and that might mean failing open if a particular abi did not support the capability
(11:44:08 AM) jjohansen: for CAP_PERFMON we intend to map to CAP_SYSADMIN on abis where it is not supported
(11:44:51 AM) jjohansen: and this what we intend to do going forward for any capability split off from CAP_SYSADMIN in the kernel
(11:45:35 AM) jjohansen: moving on
(11:45:47 AM) jjohansen: does anyone have a topic they would like to bring up
(11:46:15 AM) cboltz: stable releases
(11:46:29 AM) cboltz: not much to discuss, we just need someone who does them ;.-)
(11:46:33 AM) jjohansen: cboltz: you have the floor
(11:47:02 AM) cboltz: from the openSUSE side, I'm mostly interested in 2.13.x so that we get the latest fixes into the upcoming Leap 15.2
(11:47:09 AM) jjohansen: cboltz: agreed, we need to be better about them and have a schedule for them
(11:47:28 AM) ***cboltz proposes "release date: yesterday"
(11:47:38 AM) jjohansen: cboltz: when do you need it by?
(11:48:07 AM) cboltz: as soon as possible - Leap 15.2 entered beta phase, release is planned for early May
(11:48:12 AM) jjohansen: the question really is whether to work on cutting something today, or try to get the CAP_PERFMON changes in before cutting it
(11:48:27 AM) cboltz: (and I have to go via SLE, which might cause some delays)
(11:48:36 AM) peetaur left the room (quit: Ping timeout: 480 seconds).
(11:48:56 AM) cboltz: I tend to having the release today
(11:49:15 AM) cboltz: Leap won't have the very latest kernel anyway
(11:49:19 AM) jjohansen: I expect most things using CAP_PERFMON are unconfined so its not an issue for those
(11:49:43 AM) jjohansen: Leap may not, but kernel devs using LEAP do, and this has bitten us before
(11:50:15 AM) cboltz: nothing stops us to push a later 2.13.x as maintenance update
(11:50:26 AM) jjohansen: but like I said I expect almost everything that needs CAP_PERMON is already unconfined so the risk is fairly low
(11:50:34 AM) jjohansen: ack
(11:50:44 AM) jjohansen: I will work on cutting a 2.13.x release
(11:51:05 AM) jjohansen: one other thing one maintenance releases since the topic was brought up
(11:51:36 AM) jjohansen: older releases will have a slower release cadence than newer releases
(11:51:47 AM) jdstrand: me notes that perf_event_open is used in two snapd interfaces (one for docker and one to support bcc) and it currently grants sys_admin
(11:52:37 AM) jdstrand: so, that should continue to work based on the above plan, but we have an opportunity to remove sys_admin from the policy if the kernel supports perfmon
(11:52:39 AM) jjohansen: jdstrand: yep, CAP_PERMON is being split off from cap sys_admin so on older kernel sysadmin is what it asked for
(11:52:49 AM) jjohansen: right
(11:53:11 AM) jjohansen: and old policy on new kernels breaks
(11:53:21 AM) jdstrand: jjohansen: is there a recommended way to introspect apparmor or the kernel to see if CAP_PERFMON is available?
(11:53:34 AM) jjohansen: because the kernel asks for perfmon, and welll policy doesn't know about it
(11:54:28 AM) jjohansen: cat /sys/kernel/security/apparmor/features/caps/mask
(11:55:18 AM) jjohansen: the parser doesn't currently give away to directly inspect what it supports but you can do
(11:55:18 AM) jjohansen: echo "/e { capability perfmon, }" | apparmor_parser -QT
(11:55:30 AM) jjohansen: and that will will if it is not supported by the parser
(11:55:39 AM) jdstrand: jjohansen: ok, so we'll map a perfmon rule to sys_admin on an old kernel, but on a new kernel with only sys_admin, it breaks
(11:55:53 AM) jjohansen: no
(11:56:02 AM) jjohansen: or not exactly
(11:57:27 AM) jjohansen: the compiler will map perfmon to sysadmin if the abi does not support it. Old kernels will not support policy compiled with perfmon
(11:57:27 AM) jjohansen: new kernels will map perfmon to sysadmin for abis that don't support it
(11:57:53 AM) jjohansen: so we have to do mappings both in kernel and userspace to try and cover our bases
(11:58:16 AM) jjohansen: but unfortunately the current compiler uses the kernel abi by default
(11:58:25 AM) jjohansen: which means you won't get mappings
(11:58:37 AM) jjohansen: and you will just fail closed
(11:58:42 AM) jdstrand: ok
(11:59:22 AM) jjohansen: there is no fix, until the kernel with a fix lands
(11:59:33 AM) jjohansen: at which point userspace could at least pin the abi
(11:59:36 AM) sarnold: will we get a chance to land the fix alongside the new cap?
(12:01:14 PM) jjohansen: it looks like we will land our fix one release behind and have to roll the fix back via stable release
(12:01:50 PM) jjohansen: there is of course a chance we can land a bug fix in the current dev release
(12:02:08 PM) jjohansen: but that will depend a lot on what the bug fix looks like
(12:02:19 PM) sarnold: :(
(12:02:45 PM) jdstrand: jjohansen: for something like snapd that generates policy on a variety of kernels and userspaces, is this the (non-opitmized) algorithm:
(12:02:48 PM) jdstrand: # if previously had sys_admin for perf_event_open(), etc
(12:02:48 PM) jdstrand: if parser does not support perfmon:
(12:02:48 PM) jdstrand:   add sys_admin rule
(12:02:48 PM) jdstrand: elif the kernel supports perfmon:
(12:02:48 PM) jdstrand:   add perfmon rule
(12:02:50 PM) jdstrand: else:
(12:02:52 PM) sarnold: it never should have been cap_sys_admin in the first place so this new cap feels like a good idea, but it also feels like it ought to be included only once everybody's had a chance to adapt
(12:02:53 PM) jdstrand:   add sys_admin rule
(12:05:24 PM) jjohansen: jdstrand: yeah, with the caveat that the first case, might still break if you are not using abi pinning and the kernel supports perfmon
(12:05:31 PM) ***jdstrand notes in said system, booting into a new kernel (or userspace) is detectable and can regen policy
(12:05:52 PM) jdstrand: ok, I'll finetune that algorithm. thanks!
(12:06:33 PM) jjohansen: okay back to stable releases
(12:06:54 PM) jjohansen: do we want to discuss the release cadence or push that back to the next meeting
(12:08:31 PM) sarnold: it's been years since I've done a release but I remmeber preparing the changelog was a fairly painful part of the whole experience
(12:08:55 PM) sarnold: and the mixing of new features alongside bug fixes felt funny
(12:08:58 PM) jjohansen: yes it is
(12:09:20 PM) sarnold: are there steps we ought to be taking to make this thing less painful? like no more hand-generated changelogs? a stronger stick for "no new featrures"?
(12:10:26 PM) jjohansen: I would love for better changelogs, but I am not going to make that a painful hurdle for contribution either
(12:10:46 PM) cboltz: I'm somewhat afraid that autogenerated changelogs (for example based on git log --oneline) would be less useful
(12:11:14 PM) jjohansen: right, I don't want autogenerated changelogs in the release notes
(12:11:35 PM) jjohansen: but better changelogs would sure help in generating the release notes
(12:11:58 PM) jjohansen: what would be really nice is to get rid of merges
(12:12:08 PM) jjohansen: but uhmm that isn't going to happen
(12:12:29 PM) jjohansen: as for reducing new features
(12:12:36 PM) jjohansen: yes, we need to do that
(12:12:45 PM) jjohansen: splitting off policy will help
(12:12:53 PM) jjohansen: releasing more often will help
(12:13:49 PM) jjohansen: using a bigger stick will help, but its not like any feature that has gone in wasn't given an exception by another core dev for some reason
(12:14:13 PM) jjohansen: we aren't just sticking in new features for giggles
(12:15:13 PM) sbeattie: well, not having merge commits have a subject of "merge branch 'thing' into 'master'" would help.
(12:15:48 PM) jjohansen: I do think having a more stable release cadence will help the most, because then distros can use the latest release instead of asking for X to be thrown onto the last stable release
(12:16:22 PM) jjohansen: sbeattie: agreed, and when we switched to gitlab that was one of the requirements put on merge commits
(12:16:54 PM) jjohansen: unfortunately there is no way to enforce this, as setting up the merge commit message comes after the approval
(12:17:38 PM) jjohansen: it would also help if people rebased, before merging
(12:17:59 PM) jjohansen: and uhmm only basing merges off of master
(12:18:35 PM) jjohansen: we have had a few cases where merges were based off another branch
(12:19:05 PM) jjohansen: this makes histories very messy when slogging through them for the change log
(12:19:45 PM) jjohansen: its completely understandable to create these types of merge requests while waiting for review
(12:20:18 PM) jjohansen: but it would be very helpful if once their parent branch gets merged that they can be rebased onto master
(12:21:09 PM) jjohansen: but again, I would rather the commits, than drive people away because we are being so picky about contributions
(12:21:38 PM) jjohansen: that is rather have the commits
(12:22:04 PM) cboltz: agreed
(12:22:11 PM) cboltz: maybe we should look for a middle ground
(12:22:19 PM) cboltz: a MR that is 5 commits behind isn't too bad
(12:22:34 PM) cboltz: but a months old MR would clearly benefit from a rebase ;-)
(12:23:15 PM) jjohansen: yep
(12:24:21 PM) jjohansen: lets try to do better with merges, feel free to ask for a rebase
(12:24:39 PM) jjohansen: but lets not just block if it doesn't happen
(12:25:06 PM) jjohansen: there is also always the option of doing the rebase yourself, but I hate doing it because it breaks gitlab
(12:25:22 PM) jjohansen: in the sense that the MR needs to be closed instead of showing up as merged
(12:25:58 PM) jjohansen: there is a ticket filed with gitlab to fix this that I don't ever expect to get fixed
(12:26:07 PM) jjohansen: alright lets move on
(12:26:22 PM) jjohansen: does anybody have anything else they would like to bring up
(12:26:41 PM) cboltz: nothing from me
(12:26:55 PM) msalvatore: me neither
(12:29:12 PM) jjohansen: next meeting is currently scheduled for Tue April 14, at 18:00 UTC
(12:29:12 PM) jjohansen: if this is a problem, reach out here or in apparmor@
(12:29:12 PM) jjohansen: if you have an item for the agenda please add it to https://gitlab.com/apparmor/apparmor/-/wikis/MeetingAgenda
(12:29:12 PM) jjohansen: or poke one of the core devs here or on apparmor@ to have them add it for you
(12:29:26 PM) jjohansen: thanks everyone, meeting adjourned
 ```
