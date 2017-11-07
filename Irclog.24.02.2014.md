```
(12:01:59 PM) jjohansen: cboltz, kshitij8, sarnold, sbeattie, tyhicks, jdstrand, mdeslaur, kees, everyone else who are not interested: meeting time
(12:02:07 PM) mdeslaur: \o
(12:02:18 PM) ***jdstrand is here
(12:02:28 PM) ***sarnold is here
(12:02:36 PM) tyhicks: hello
(12:02:37 PM) kshitij8: "not interested"? 
(12:02:42 PM) jjohansen: hehe :)
(12:02:43 PM) ***kshitij8 \o
(12:03:23 PM) jjohansen: kshitij8: well obviously we don't want people who are interested otherwise the meeting wouldn't be quick :)
(12:03:41 PM) cboltz: ;-)
(12:03:50 PM) kshitij8: hehe... 
(12:04:02 PM) jjohansen: alright lets get started
(12:04:08 PM) ***sbeattie o/
(12:04:31 PM) jjohansen: sarnold: got the 2.8.3 release out last week. Thankyou sarnold
(12:04:52 PM) ***sarnold bows
(12:05:05 PM) jjohansen: We need to decide if we are going to do another 2.8 release or focus on 2.9
(12:05:55 PM) cboltz: I'd say "we'll see" - if the 2.8 branch collects some more patches, do another release - if not, then not
(12:05:56 PM) jjohansen: I think there are a few things that could go into a 2.8.4 so it seems worth doing even when 2.9 is released. Any objections?
(12:06:03 PM) sarnold: I know cboltz has committed some things for 2.8 even after 2.8.3; a 2.8.4 at some point in the future might make sense, though we probably won't collect as much for it as we did for 2.8.3. we probably shouldn't wait as long for the next 2.8.4, either..
(12:06:26 PM) jjohansen: right we waited too long for 2.8.3
(12:06:48 PM) jjohansen: I see 2.8.4 as likely the last of the 2.8 series
(12:07:46 PM) cboltz: sarnold: I wouldn't call 0.75 patches "some things" ;-)
(12:08:08 PM) sarnold: cboltz: no more brain. I knew it was more than 0 things. :)
(12:08:16 PM) jjohansen: heh, well I seem to recall a request to update the kernel patches in there
(12:08:28 PM) sarnold: that'd make sense too
(12:08:31 PM) sbeattie: well, we certainly didn't walk through the trunk commits to see if there was anything worth pulling back for 2.8.3.
(12:08:44 PM) sarnold: though as I wouldn't be the one doing the work it's hard to say that someone else shoudl. :)
(12:08:47 PM) jjohansen: that would make sense as well
(12:09:06 PM) sarnold: sbeattie: true. but we've been good about nominating at the time of submission..
(12:09:23 PM) sbeattie: ehhh, we're maybe 50/50 on those.
(12:09:37 PM) jjohansen: yeah
(12:09:39 PM) sarnold: okay, maybe 2.8.4 will be larger than I expected :)
(12:09:49 PM) jjohansen: its worth making a pass at it
(12:09:57 PM) sarnold: who would be a consumer for 2.8.4?
(12:10:02 PM) sbeattie: e.g. the parser error on invalid arg options, I didn't nominate.
(12:10:17 PM) sarnold: .. and when would said consumer want 2.8.4? :)
(12:10:41 PM) jjohansen: sarnold: people who are doing an update on a 2.8 package and don't want to pull in everything in 2.9/3.0x
(12:11:16 PM) cboltz: for openSUSE, I'll use 2.8.x only for maintenance updates
(12:11:36 PM) cboltz: factory will get 2.9 when it's released
(12:11:38 PM) jjohansen: sarnold: as for when, bug fixes are always welcome before they are encountered
(12:12:24 PM) ***cboltz wouldn't invest too much time on checking all trunk commits for backport candidates
(12:12:32 PM) jjohansen: I would say 2.8.4 in a month, or two. Lets get some of the current 2.9/3.0 work off the table, and then we can scan through for 2.8 candidate patches
(12:12:36 PM) sarnold: jjohansen: more ,'can they wait a month or two?' :)
(12:12:42 PM) sarnold: yay
(12:13:01 PM) jjohansen: sarnold: hopefully yes, if we didn't completely bork 2.8.3
(12:13:11 PM) sarnold: no complaints yet..
(12:13:34 PM) jjohansen: alright lets plan for a 2.8.4 post 2.9 release
(12:13:44 PM) jjohansen: Now on to 2.9
(12:14:11 PM) jjohansen: This is planned as a snapshot of the 3.0 work, as 3.0 is dragging on far too long
(12:15:24 PM) jjohansen: it is 2.8 compatible unless you have out of tree patches to kernel, dbus etc. In which case you do gain access to some new rule types
(12:16:09 PM) jjohansen: sarnold has been working on beating the 3.0 code into something that can be done as a 2.9 release
(12:16:29 PM) jjohansen: sarnold: can you give a quick update of where we are at with this
(12:17:03 PM) sarnold: the library version bump was more painful than I expected, thogh much of that was learning debian packaging
(12:17:25 PM) jjohansen: how do we expect this to affect rpm?
(12:17:43 PM) sarnold: I've got one patch needed for the packaging that I still need to push upstream, and then we can set cboltz loose on finding out how bad it is for rpm.
(12:18:33 PM) sarnold: I've never done this kind of work with rpm before; I have a suspicion it will be slighlty better for the rpm world, but they may still also require no-change rebuilds to several packages to update to the new dependencies
(12:18:40 PM) sarnold: s/better/easier/
(12:18:45 PM) sbeattie: I imagine the rpm world has fewer dependencies on libapparmor
(12:18:49 PM) ***cboltz will just give sarnold write permissions in OBS to fix everything
(12:18:50 PM) sarnold: true
(12:19:03 PM) sarnold: cboltz: eeek :)
(12:19:53 PM) sarnold: anyway, our internal QRT test suite failed pretty badly on the packages I have made; sbeattie has been working on some fixes, but I suspect it'll require another day or two to either modify the QRT tests so they function correctly or modify apparmor to match the tests
(12:20:01 PM) jjohansen: I still question the library bump, it seems to be a consequence of libtool versioning and debian package interactions instead of a real need to bump the library
(12:20:45 PM) jjohansen: sarnold: so generally we shouldn't be changing behavior, at least not unless the kernel is newer
(12:20:58 PM) sarnold: you could be right but someone in calmer thinking came up with those rules and I don't want to question them when I'm not thinking at my best
(12:21:04 PM) jjohansen: so I would say we maybe farther away than that :(
(12:21:37 PM) sarnold: I'm usually too optimistic, this is true
(12:22:00 PM) jjohansen: sarnold: I'm not sure those rules where actually thought out (at least the interaction) and that it just happened and was accepted
(12:22:13 PM) jjohansen: but I digress
(12:22:35 PM) jjohansen: sarnold: okay do you think we can get a 2.9 out this week?
(12:22:54 PM) sarnold: jjohansen: I sure hope so, but I haven't started debugging any of the other issues
(12:23:01 PM) sarnold: s/of the other//
(12:23:07 PM) jjohansen: okay thanks
(12:23:17 PM) jjohansen: anyone else with 2.9 input?
(12:24:24 PM) sarnold: (I should mention in passing that the build-time upstream tests work fine on 2.8.95)
(12:24:53 PM) sarnold: I know there's been a flurry of patches to trunk lately, should I re-roll with those included?
(12:25:17 PM) sarnold: I'm inclined to say "yes", since I want to push that auto*whatever patch upstream and drop it from ubuntu packaging
(12:25:34 PM) jjohansen: sarnold: your call
(12:25:44 PM) sbeattie: I dunno about a flurry, but yes, a few. 
(12:25:54 PM) sbeattie: I'd probably pull them in.
(12:26:01 PM) sarnold: cool, thanks, I'll do that after lunch
(12:26:57 PM) jjohansen: cboltz: do you have any hard deadline when you need 2.9 (I know we are past suse freeze)
(12:27:23 PM) cboltz: I only know the beta1 deadline was some days ago ;-)
(12:27:36 PM) cboltz: (the SLE schedule seems to be top secret ;-)
(12:27:55 PM) cboltz: darix: are you allowed to tell us some details?
(12:28:00 PM) jjohansen: okay, well if you hear anything that should light a fire under us please kick sarnold ;-)
(12:28:10 PM) darix: it wasnt me
(12:28:19 PM) sarnold: :)
(12:28:31 PM) jjohansen: alright lets move onto 3.0
(12:28:56 PM) jjohansen: obviously we've pushed its release date out again (hence the 2.9 snapshot)
(12:29:15 PM) jjohansen: there will be some sort of alpha going up soon
(12:29:29 PM) jjohansen: but I think we have a few issues to resolve
(12:29:47 PM) jjohansen: first up cache versioning
(12:30:32 PM) jjohansen: - I've got some patches to update the parser to store off 3 bits off version info in the compiled policy headers
(12:30:46 PM) jjohansen: - policy version, parser abi version, kernel abi version
(12:31:27 PM) jjohansen: - this will allow the parser to peek at the header part of the compiled policy instead of just the time stamp
(12:31:56 PM) cboltz: jjohansen: just wondering - would it make sense to store "force-complain yes/no" as another flag?
(12:32:07 PM) cboltz: I mean - you are already changing the file layout...
(12:32:12 PM) jjohansen: however this change will only work with newer kernels, as the current kernel treats the entire field as the kernel version
(12:32:23 PM) jjohansen: cboltz: possibly
(12:32:38 PM) jjohansen: so force complain is one of the issues to discuss
(12:32:54 PM) jjohansen: the header is stored once per file
(12:33:07 PM) jjohansen: however a file could have multiple profiles in it
(12:33:47 PM) jjohansen: it is theoretically possible to force complain one profile within that set, but the current tool setup doesn't allow it
(12:34:01 PM) cboltz: a force-complain symlink can only link to the whole file, so... ;-)
(12:34:10 PM) jjohansen: that if -C to the parser or the symlink are applied at the file level (same as caching is done)
(12:34:24 PM) jjohansen: cboltz: yes
(12:34:35 PM) jjohansen: like I said theoretically
(12:35:04 PM) jjohansen: in actual fact you can cat binary files together, and the kernel will handle them as a single atomic load
(12:35:15 PM) jjohansen: that is all replacements happen, or none
(12:35:33 PM) jjohansen: at least with the 3.12 kernel
(12:36:09 PM) jjohansen: so one of the question is should we dedicate a bit to indicate force complain so caching can pick that up
(12:36:38 PM) jjohansen: understanding that it is possible (by going outside the current tool chain) to break this?
(12:36:57 PM) jjohansen: and is it a behavior we want going forward?
(12:37:09 PM) ***jjohansen knows where cboltz stands
(12:37:12 PM) sarnold: I've never cared for 'force complain' symlink thingies, but I also don't care enough to ... well, care. hehe.
(12:37:39 PM) jjohansen: sarnold: the force-complain and disable symlinks come about because of packaging
(12:38:13 PM) jjohansen: lets not have another fight about that atm, besides its not just the symlink but the -C flag to the parser
(12:38:17 PM) jjohansen: either can trigger it
(12:38:52 PM) sarnold: yeah, lunch is caling. i care more about lunch. :)
(12:39:27 PM) jjohansen: alright, so I will add force complain to the version # caching logic
(12:40:02 PM) sarnold: thanks
(12:40:05 PM) sbeattie: jjohansen: okay. I guess it'd be nice if we could re-use the cache blob when toggling between force-complain and enforce, but that's maybe a bit much.
(12:40:11 PM) jjohansen: again this will only apply to kernels that have been updated, when used with older v5 abi kernels the field will only be treated as the v5 abi #
sbeattie sbeattie_ 
(12:41:08 PM) cboltz: that reminds me - IIRC kshitij8's aa-complain currently adds flags=(complain) _and_ the force-complain symlink
(12:41:17 PM) jjohansen: sbeattie: unfortunately I don't see a way to do that atm, as force complain modifies the profile data by setting the complain flag on each profile
(12:41:18 PM) cboltz: we should decide on one of them (and/or make it a config option)
sbeattie sbeattie_ 
(12:41:51 PM) jjohansen: sbeattie: which means we can't disambiguate which where complain, and which where forced-complain
(12:42:14 PM) jjohansen: hrmmm, or since we are doing an abi break with v5 -> v6
(12:42:36 PM) jjohansen: we could actually interpret the force complain bit in the header
(12:42:46 PM) sbeattie: jjohansen: it also gets deeper into the blob structure I think, which means bits of it change and so can't be re-used directly.
(12:42:50 PM) jjohansen: and not set it on the profiles at all, and have the kernel do that
sbeattie sbeattie_ 
(12:43:09 PM) jjohansen: sbeattie: right, what do you think about ^
(12:43:34 PM) sbeattie: jjohansen: that seems like a reasonable solution.
(12:43:37 PM) jjohansen: of course then we need to clear the bit in the header, but that is much easier
(12:43:44 PM) sbeattie: yeah
(12:44:24 PM) jjohansen: alright, lets roll with that. For the moment I will leave detecting/clearing the force-complain bit in the header as a todo
(12:44:35 PM) jjohansen: next up dir structure for caching
(12:45:01 PM) jjohansen: so we want to be able to have the ability to store multiple cache sets
(12:45:18 PM) jjohansen: so we can boot between different kernels without having to recompile cache
(12:45:50 PM) jjohansen: it also will allow us to ship pre compiled policy, so that policy doesn't always have to be compiled at install
(12:46:14 PM) jjohansen: we need to determine what this dir structure should look like
(12:47:01 PM) jjohansen: are we going to tie to a specific kernel, a set of revision strings (say policy+parser abi+kernel abi) etc
(12:47:36 PM) ***cboltz wonders if shipping pre compiled policy is worth the effort (and what packaging fun it will cause)
(12:47:48 PM) jjohansen: please note regular rules about cache being invalid will still apply so, local updates still may force a policy recompile again precompiled policy
(12:48:13 PM) jjohansen: cboltz: Ubuntu does have use cases for it (think phone images)
(12:48:34 PM) jjohansen: policy compiles are rather painful on phones atm
(12:49:06 PM) jdstrand: cboltz: the reason why the symlink was introduced with complain was specfically to avoid changing the profile on disk
(12:49:23 PM) jjohansen: we will be working on improving compile times but caching is the way to improve performance the most for most of those cases
(12:49:35 PM) jjohansen: jdstrand: yep packaging issue
(12:49:48 PM) jdstrand: cboltz: if we change the profile, then deb packagers lose
(12:50:22 PM) jdstrand: cboltz: so my recommendation is to have the tools behave the same as they do now. if it makes sense to change things later, we can discuss
(12:50:52 PM) jjohansen: cboltz: also we just want to offer the option of shipping precompiled policy, it makes sense for images (livecds, phone images etc.) not so much for regular systems
(12:50:54 PM) tyhicks: jjohansen: I think we need to use the specific kernel version simply because 3.13.0-10.30-generic (Ubuntu) may, unfortunately, support different things than 3.13.0 (vanilla)
(12:50:57 PM) jdstrand: (same wrt to add the flag or using the symlink that is)
(12:51:08 PM) jjohansen: I expect regular packaging to avoid it
(12:51:31 PM) cboltz: what happens if the (packaged) cache file has a newer timestamp than the updated profile?
(12:51:49 PM) jjohansen: tyhicks: that is true, different versions of 3.13 could have different patches applied
(12:52:06 PM) jjohansen: we could use a hash of the features file for the dir name
(12:52:24 PM) cboltz: (and yes, I agree that this makes [IMHO: only] sense for livecds etc.)
(12:52:44 PM) tyhicks: jjohansen: why not something more human friendly, such as $(uname -r)?
(12:52:58 PM) jjohansen: cboltz: well generally that means that the cache is taken over the text file
(12:53:26 PM) jjohansen: tyhicks: that is a possibility, it does mean cache files can't be shared between kernels
(12:53:45 PM) jjohansen: I am ambivalent
(12:54:01 PM) tyhicks: hmmm
(12:54:20 PM) jjohansen: what do we care about more? cache being shared or a more friendly form?
(12:54:28 PM) cboltz: how likely is it that two kernels can use the same cache files?
(12:54:45 PM) tyhicks: I don't see it as being shared cache vs human friendly
(12:54:51 PM) jjohansen: cboltz: within a release very likely
(12:54:56 PM) tyhicks: cboltz has the right question
(12:55:19 PM) sbeattie: cboltz: in a released OS with periodic kernel updates, likely.
(12:55:42 PM) jjohansen: so think of it like this. kernels within an OS release have different version numbers, due to security updates etc
(12:55:48 PM) tyhicks: yes, those periodic kernel updates will be fine
(12:55:53 PM) sbeattie: tyhicks: I thought at the sprint, there was discussion of using symlinks across kernel versions, with something to do garbage collection
(12:55:53 PM) jjohansen: those kernels could all share cache
(12:55:54 PM) tyhicks: but what about booting into vanilla
(12:56:12 PM) tyhicks: the features file may be the same but support may be different
(12:56:24 PM) jjohansen: tyhicks: vanilla would have a different feature set, get a different hash
(12:56:43 PM) jjohansen: tyhicks: if the features file is the same it should be the same support
(12:56:50 PM) tyhicks: jjohansen: we would need to be 100% sure of that to use the hash technique
(12:57:00 PM) tyhicks: ok, then I'm happy with using the hash
(12:57:03 PM) jjohansen: tyhicks: if its not, its a bug
(12:57:34 PM) jjohansen: tyhicks: the name could be a little more friendly than a hash. it could be version + hash
(12:58:05 PM) tyhicks: jjohansen: if the load fails, do we fall back to compiling the policy and regenerating the cache?
(12:58:17 PM) jjohansen: we could make the version just a major kernel revision, or apparmor revision
(12:58:23 PM) jjohansen: just possibilities
(12:58:58 PM) cboltz: if it's about being friendly, I tend to use the kernel version
(12:59:00 PM) tyhicks: if we have some type of fallback, I'm happy with the hash method
(12:59:01 PM) jjohansen: tyhicks: so as I see it the dir, still contains a .features file that is exact matched and each file contains the new revision info
(12:59:13 PM) jjohansen: if either of those fail we fall back to compiling
(12:59:23 PM) jjohansen: oh and timestamps will be used as well
(12:59:25 PM) tyhicks: sounds good
(12:59:43 PM) jjohansen: one more thing, I have removed the cache timestamp check on the parser
(01:00:02 PM) jjohansen: this means shipping a new parser does not automatically cause a policy recompile
(01:00:13 PM) jjohansen: instead we now have a parser abi field
(01:00:19 PM) tyhicks: cboltz: on second thought, we don't need to worry about being human friendly... we're talking about a cache of binary policy here :)
(01:00:32 PM) jjohansen: if you need to force a recompile when a new parser is shipped that can be bumped
(01:00:52 PM) cboltz: tyhicks: tell this to "df -h" on my always-updated system ;-)
(01:01:16 PM) jjohansen: I think it is worth being somewhat human friendly
(01:02:20 PM) tyhicks: it would be easy to add a utility to delete old caches that don't match the current running kernel
(01:02:27 PM) jjohansen: something like 3.13+8f0ed59ec9b412b61feb6b88375f504e
(01:02:54 PM) jjohansen: would get us sharing within an updated kernel series and be quite helpful to a human
(01:03:29 PM) tyhicks: until Linus starts using git hashes for release versions
(01:03:35 PM) tyhicks: (/me is kidding ;)
(01:03:41 PM) jjohansen: yes, we should definitely add a tool to manage the cache
(01:04:06 PM) jjohansen: hehe, oh I wouldn't put it past him. In fact please don't suggest it :)
(01:04:28 PM) tyhicks: sbeattie: back to your mention of garbage collection... I don't really remember that discussion :/
(01:04:56 PM) jjohansen: I think it was on friday so /me is not surprised tyhicks doesn't remember
(01:05:11 PM) tyhicks: ok, as long as someone does
(01:05:44 PM) jjohansen: basically packaging has a problem with sharing the files
(01:06:15 PM) jjohansen: if we use symlinks to do sharing, then there needs to be something that can do garbage collection because symlinks are not reference counted
(01:06:34 PM) jjohansen: and of course hard links can't be used across partitions
(01:06:49 PM) tyhicks: ok
(01:07:00 PM) jjohansen: and I think packaging has an issue with hardlinks?
(01:07:30 PM) jjohansen: otherwise if we don't care about storing policy across partitions hardlinks could be used and they are reference counted
(01:07:59 PM) jjohansen: anyways I think that is a packaging detail, that is a higher level than the upstream apparmor discussion
(01:08:44 PM) cboltz: I'd guess that nobody splits the cache directory across multiple partitions ;-)
(01:09:22 PM) jdstrand: I'm not clear where the symlinks would be nor where their targets lie. I kinda thought it was all in /etc, so hard links could be used, but I also thought we'd symlink directories, and we can't hard link directories, so I'm not clear on what we are talking about :)
(01:09:31 PM) jjohansen: So I think the proposal is
(01:09:31 PM) jjohansen: we make the cache/ directory a tree, with versioned dirs under it. The versioned dirs still do the timestamp, version and .features file check
(01:09:31 PM) jjohansen: If a particular versioned dir is not present, the parser will fall back to the cache/ directory and check the policy stored there
(01:10:11 PM) jjohansen: jdstrand: I thought we where also talking about moving the cache out of /etc/ to /var/ or /lib/
(01:11:09 PM) jdstrand: that is an ongoing discussion for sure, but I thought cache/* would all be on the same partition
(01:11:14 PM) jjohansen: jdstrand: the symlinks was raised as one way of sharing policy cache, that way you could have a per kernel "dir" that actually pointed to the shared dir, because dirs can not be hardlinked
(01:11:34 PM) jdstrand: but again, you mention hardlinking, and I was thinking of dirs, so know I'm not sure what you are suggesting
(01:11:47 PM) jjohansen: and of course as soon as you use symlinks there needs to be garbage collection on the target once all the referring dirs are removed
(01:11:50 PM) jdstrand: s/know/now
(01:12:04 PM) jdstrand: sure
(01:12:38 PM) jdstrand: btw, I'm not advocating for the symlinks per se, I'm just saying I don't know what we are talking about with hard links
(01:12:45 PM) jjohansen: jdstrand: sorry yes, hard link doesn't work at the directory level, my bad for mentioning it. Basically that would work by having a per kernel dir, and then hardlinking to policy files
(01:12:57 PM) jjohansen: but I think a shared dir via hash is better
(01:13:15 PM) jdstrand: ok
(01:13:19 PM) jjohansen: jdstrand: the hardlink dir was one of the options raised is all
(01:13:44 PM) cboltz: without knowing the internal details, I'd guess that we don't need any {sym,hard}links with $kernelmajorversion-$hash directory layout
(01:14:48 PM) jdstrand: 'any'? you mean, we could get rid of the force-complain ones, for example?
(01:14:59 PM) jjohansen: again, I think this is a packaging end of sharing is something that doesn't have to be part of the upstream spec
(01:15:09 PM) cboltz: I was only talking about the cache dir ;-)
(01:15:15 PM) jjohansen: cboltz: right
(01:15:24 PM) jdstrand: ok, cause I was having trouble connecting the dots there :)
(01:15:55 PM) jjohansen: okay, lets run with that as the basics of what we are going to do, sarnold I hope you are taking notes ;-)
(01:16:00 PM) jjohansen: next topic
(01:16:21 PM) jjohansen: policy version #ing
(01:16:35 PM) jjohansen: I know we have hit this multiple times
(01:17:07 PM) jjohansen: basically policy files are going to pick up a version #
(01:17:30 PM) jjohansen: newer parsers will have to deal with this
(01:18:06 PM) jjohansen: the version # indicates new semantics, though not necessarily features supported
(01:18:20 PM) jjohansen: as that depends on the kernel as well
(01:18:26 PM) jjohansen: everyone is okay with this?
(01:19:08 PM) jjohansen: I'll take that as a yes, patches will come soon
(01:19:11 PM) jjohansen: moving on
(01:19:28 PM) jjohansen: we have issues with namespaces
(01:19:36 PM) jjohansen: lots of issues...
(01:20:32 PM) jjohansen: we are starting to hit attach_disconnected issues a lot more with systemd (which is being dumb, trying to circumvent security/selinux via passing file handles)
(01:20:39 PM) jjohansen: and and lxc
(01:20:52 PM) jjohansen: anyways these are things we have to deal with
(01:21:05 PM) jjohansen: as it stands there are a couple of things to deal with
(01:21:12 PM) jjohansen: 1. new namespace control
(01:21:32 PM) jjohansen: currently this entirely controlled by the cap_sys_admin capability
(01:21:54 PM) jjohansen: I am adding ability to control newns, netnet, newpid individually
(01:22:15 PM) jjohansen: Do we want to see this as new pseudo apparmor capabilities
(01:22:38 PM) jjohansen:   ie. capability newns newpid,
(01:22:46 PM) jjohansen: or as its own flag
(01:22:54 PM) jjohansen:   clone newns newpid,
(01:23:02 PM) jjohansen: as it isn't a true kernel capability
(01:24:06 PM) cboltz: making it a fake-capability will confuse users who read man 7 capabilities
(01:24:59 PM) sbeattie: I'd either see this as something like ns_capability or have them all prefixed with ns_ to indicate that they're different from 'real' kernel capabilities
(01:25:29 PM) jdstrand: I agree-- keep them separate from real capabilities
(01:26:06 PM) sbeattie: jjohansen: how are they currently mediated under DAC?
(01:26:08 PM) jjohansen: so which you prefer to see
(01:26:08 PM) jjohansen:   capability ns_newns, ns_newpid,
(01:26:08 PM) jjohansen: or
(01:26:08 PM) jjohansen:   ns_capablity newns,
sbeattie sbeattie_ 
(01:26:24 PM) jjohansen: sbeattie: cap_sys_admin,
(01:26:50 PM) jdstrand: since 'capability' says 'man capabilities' to me, I like 'ns_capablity'
(01:27:01 PM) jjohansen: there are reasons to want it to be more strict for apparmor though
(01:27:03 PM) jdstrand: assuming the spelling gets corrected
(01:27:27 PM) jdstrand: though, ns_capability admittedly looks a little strange to me
(01:28:25 PM) sbeattie: 'namespace capability newns newpid,' ?
(01:28:33 PM) jjohansen: well funny enough I don't read capability as linux caps, but thats just me
(01:28:43 PM) cboltz: are you sure we'll only have namespace-related sub-capabilities in the future?
(01:28:55 PM) jjohansen: no, its likely we will have others
(01:28:56 PM) cboltz: if not, we should use a keyword that does not contain "namespace" or "ns" ;-)
(01:29:09 PM) jjohansen: capability? :-P
(01:29:44 PM) cboltz: only if you get them added to capabilities(7) ;-)
(01:29:45 PM) sbeattie: "capability namespace" "capability kernel" "capability other_thing" ?
(01:30:19 PM) ***jjohansen isn't sure that capability should strictly map to man 7 capabilities
(01:30:26 PM) jjohansen: but what ever
(01:30:36 PM) ***cboltz foresees people searching for CAP_NAMESPACE and CAP_KERNEL
(01:31:02 PM) sbeattie: that way, saying something like 'capability namespace,' would grant all the namespace pseudo capabilities, but none of the other ones.
(01:32:45 PM) jjohansen: hrmm I suppose we could do something like that
(01:33:10 PM) sbeattie: (and similarly for other pseudo-capability domains)
(01:33:21 PM) jjohansen: so
(01:33:23 PM) jdstrand: jjohansen: it maybe shouldn't, but clearly there are apparmor users that feel it does, and so we should at least consider there might be confusion. if the confusion is worth the benefit of 'capability', then that's ok with me
(01:33:56 PM) jjohansen:  capability namespace=(newns, newpid),
(01:33:56 PM) jjohansen: vs.
(01:33:56 PM) jjohansen:   capability namespace newns newpid,
(01:34:36 PM) cboltz: what happens if someone decides to introduce CAP_NAMESPACE in the kernel? ;-)
(01:34:49 PM) jjohansen: jdstrand: right, its a matter of sorting out what capability should mean to apparmor and being clear and consistent on it moving forward
(01:36:35 PM) jjohansen: cboltz: that would be unfortunate
(01:36:59 PM) jjohansen: we could prefix with aa
(01:37:13 PM) cboltz: "unfortune"? that's why I don't like the idea of using "capability" ;-)
(01:37:31 PM) jjohansen: either
(01:37:31 PM) jjohansen:   aa_capability namespace,
(01:37:31 PM) jjohansen: or
(01:37:38 PM) jjohansen:   capability aa_namespace
(01:37:56 PM) ***cboltz prefers aa_capability namespace,
(01:38:14 PM) jjohansen: cboltz: well its not like we couldn't map around that, but yes it would be kind of ugly
(01:38:21 PM) jdstrand: I think I actually prefere 'capability aa_namespace' for some reason, but am ok with either
(01:38:45 PM) jdstrand: the aa_ made a big difference to me :)
(01:40:10 PM) jjohansen: any other votes?
(01:41:40 PM) sbeattie: I don't have an opinion one way or the other, I imagine as we get closer to landing the feature, we'll argue about it more.
(01:41:58 PM) sbeattie: (I know, ever the optimist I am)
(01:42:04 PM) mdeslaur left the room (quit: Quit: *** stack smashing detected ***).
(01:42:21 PM) jjohansen: okay I'll randomly choose one, moving on
(01:42:24 PM) kshitij8: aa_capability for me (if anyone's counting), it speaks distinctly that its APArmor's cap)
(01:42:40 PM) kshitij8: *apparmor
(01:43:07 PM) mdeslaur [~mdeslaur@166.62.243.58] entered the room.
(01:43:08 PM) jjohansen: kshitij8: thanks, and yes feel free to speak up
(01:43:17 PM) kshitij8: :)
(01:43:31 PM) jjohansen: alright the ugliness of attach_disconnected
(01:44:08 PM) jjohansen: do we make attach_disconnect apply to abstract socket, newns pids, etc
(01:44:39 PM) jjohansen: basically when crossing namespaces anything that has a newX clone flag can be disconnected
(01:45:19 PM) jjohansen: some (like pids) may have a mapping dependent on which ns's are talking
(01:45:47 PM) jjohansen: the long term plan is to deal with this via delegation and labels
(01:46:03 PM) jjohansen: but currently we have the very ugly attach_disconnected flag
(01:46:23 PM) jjohansen: opinions?
(01:47:08 PM) cboltz: what about making it a keyword like audit or deny?
(01:47:10 PM) cboltz: something like
(01:47:21 PM) ***jjohansen biases the discussion by expressing that he doesn't want to allow it, and keeping it only to filesystems
(01:47:26 PM) cboltz:     attach_disconnected /var/lib/nscd/* r,
(01:47:45 PM) jjohansen: cboltz: and what does that mean?
(01:48:17 PM) cboltz: well, the typical reason why I need attach_disconnected are nscd-related files
(01:48:20 PM) sbeattie: jjohansen: I don't think I have I grasp on the consequences thereof.
(01:48:31 PM) ***jdstrand either
(01:48:38 PM) cboltz: so IMHO it would make sense to be able to set the flag only for the affected files
(01:48:42 PM) jjohansen: the whole point of attach disconnected is that we don't have an actual complete path, we need some way to resolve what it should actually mean
(01:49:07 PM) jjohansen: so to be clear, in the kernel we get something like
(01:49:25 PM) jjohansen:   var/lib/nscd/foo
(01:49:50 PM) jjohansen: and we don't know where it belongs in the fs, because its mount point is not visible
(01:50:04 PM) jjohansen: it could be root ie. /var/lib/nscd/foo
(01:50:24 PM) jjohansen: but it could be
(01:50:24 PM) jjohansen:   /tmp/lxc-foo/var/lib/nscd/foo
(01:50:30 PM) jjohansen: or something else
(01:51:03 PM) jjohansen: that is the point does NOT have an actual name/location in the current namespace
(01:51:11 PM) jjohansen: we are talking to something outside of it
(01:51:42 PM) jjohansen: and since its not visible to the current ns. The current ns does not have a way to actually name it
(01:52:24 PM) jjohansen: eg.  ns1 creates abstract unix socket @foo, it then passes an end for that socket to a container task in a new namespace
(01:52:51 PM) jjohansen: in that namespace another task can create a different abstract unix socket @foo
(01:52:58 PM) sarnold: I've thought before that it'd be nice to be able to specify some place in a profile to fake-mount them: e.g. attach_to /AA_ATTACHED,  then rules could have e.g. /AA_ATTACHED/var/lib/nscd/foo  rw,
(01:53:24 PM) jjohansen: which is a name collision except for the fact the other socket doesn't exist in its namespace
(01:53:51 PM) jjohansen: this is why /me hates attach_disconnected, we can NOT achieve proper mediation when it is used
(01:53:59 PM) jdstrand: jjohansen: so, you said systemd is doing a lot of the fd passing so one end of the socket is in the namespace and the other in the 'host'?
(01:54:08 PM) jjohansen: yes
(01:54:15 PM) jjohansen: so is lxc
(01:54:22 PM) cboltz: BTW: how does attach_disconnected work today? (my guess is: prefix with "/")
(01:54:43 PM) jjohansen: cboltz: yes it just attaches the disconnected path to the root
(01:55:03 PM) jjohansen: we could add the ability to specify where to attach it
(01:55:07 PM) jjohansen: which I will get too
(01:55:23 PM) jjohansen: s/too/to
(01:55:26 PM) jdstrand: jjohansen: and do I understand our preference to be that we need to use the attach_disconnected global flag for the profile if the process is in the namaspace and has one of these passed fds? (as opposed to implementing some interim solution until delegation properly supports it)
(01:55:36 PM) jdstrand: s/our preference/your preference/
(01:56:07 PM) jjohansen: jdstrand: my preference is we never use attach_disconnected
(01:56:27 PM) jdstrand: that is the ultimate preference, no?
(01:56:32 PM) jjohansen: never, ever, ever, ever. Burn it with fire
(01:56:39 PM) cboltz: ... but programs using nscd disagree ;-)
(01:56:46 PM) jdstrand: but until we have proper delegation, won't we need it?
(01:56:54 PM) jjohansen: cboltz: understood, it is the only thing we have today
(01:56:59 PM) jjohansen: yes
(01:56:59 PM) ***cboltz needs attach_disconnected in every apache hat
(01:57:26 PM) ***jjohansen dies a little more
(01:57:46 PM) jdstrand: we are using attach_disconnected in the phone apps too, cause they need to reach into the android namespace for a file
(01:58:01 PM) jdstrand: I'm guessing that will go away though with the new namespace support
(01:58:07 PM) jjohansen: jdstrand: yes, hello lxc
(01:58:15 PM) ***jdstrand nods
(01:58:21 PM) jjohansen: jdstrand: uhm maybe
(01:58:39 PM) jjohansen: so lets dive into the other end of the namespace support
(01:59:00 PM) jdstrand: so, the specific question is what? should we implement some interim solution for ipc or just use attach_disconnected until we have proper delegation?
(01:59:50 PM) jjohansen: so newns it self is only half of the problem, the new ns actually has the same root
(02:00:37 PM) jjohansen: jdstrand: lets pause on ipc for just a minute
(02:01:00 PM) jjohansen: jdstrand: also understand adding attach_disconnected to ipc is work
(02:01:19 PM) jjohansen: that has not been done yet
(02:02:09 PM) jjohansen: so at least for filesystems we have the very real problem of chroot/pivotroot
(02:02:49 PM) jjohansen: what lxc does is it creates a newns and then does a pivot root moving all its tasks into the new root
(02:03:03 PM) jjohansen: so / -> /var/lxc-container/newroot/
(02:03:38 PM) jjohansen: however unless we transition profiles at this point, the profile now is aliasing all new lookups
(02:03:40 PM) jjohansen: ie.
(02:04:08 PM) jjohansen:    /foo/bar  pre pivot root   matches the rule /foo/bar r,
(02:04:53 PM) jjohansen: however post pivot root the file that was originally /var/lxc-container/newroot/foo/bar now is the file that matches that rule
(02:05:08 PM) jjohansen: this is really problematic from a profiling pov
(02:05:32 PM) jjohansen: as you need to mash pre and post permissions into one profile and they may alias each other
(02:05:57 PM) jdstrand: for the profiler trying to apply policy defined outside the ns to processes runnign in the ns?
(02:06:21 PM) jjohansen: jdstrand: no
(02:06:33 PM) jjohansen: though that is part of the problem
(02:07:04 PM) jjohansen: jdstrand: this is more generic, to be any profile being applied to any task at the point it makes this transition
(02:07:35 PM) jjohansen: so for lxc yes because the outside is apply policy to the container as part of its setup, but its more generic
(02:07:59 PM) jjohansen: anyways, this is why I added a transition rule for pivot_root and chroot
(02:08:11 PM) jjohansen:   pivot_root /foo -> profile2,
(02:10:53 PM) jjohansen: however this has problems
(02:10:53 PM) jjohansen: 1.  profile2 is not labeled on the open files. Causing revalidation, and of course those files don't exist in its ns, so we hit requiring attach_disconnected
(02:10:53 PM) jjohansen: 2. profile2 can only apply to the task calling pivot_root but, pivot_root may apply to the root of multiple tasks, which means those other tasks do not get properly transitioned
(02:10:53 PM) jjohansen: 3. The feed back from the lxc devs is that they HATE this mechanism and just want the current profile to properly apply
(02:10:53 PM) jjohansen: 4. Its got really issues with stacking :/
(02:11:42 PM) jjohansen: So the idea is when pivot root is applied we can apply the root change to a variable, that is then used as the base for lookups from that profile ns.
(02:12:16 PM) jjohansen: I am proposing this be the default behavior
(02:12:31 PM) jjohansen: this change is inconsistent with what we do now
(02:12:47 PM) jjohansen: however what we do now is broken, and requires attach disconnected
(02:13:48 PM) jjohansen: the other option is we make this variable setting optional, add require specifying it the rule
(02:13:52 PM) jjohansen: something like
(02:14:27 PM) jjohansen:   pivotroot /foo set=(@{root}),
(02:14:31 PM) sarnold: this variable would make sense for filesystem accesses; how well would it work for other ipc mechanisms? it'd be nice to use the same mechanism for them all.
(02:14:35 PM) jjohansen: or something of the sort
(02:15:23 PM) jjohansen: sarnold: they are all independent and the ipc mechanisms do not have the same concept of a hierarchial root that could be mapped to
(02:15:43 PM) jjohansen: eg. pivotroot, chroot always step to somewhere in the existing filesystem
(02:16:10 PM) jjohansen: so we can take a snapshot of that location and use that going forward (even if it does technically move)
(02:17:14 PM) jjohansen: however what is the hierarchy for @foo, there is no hierarchy for abstract sockets. And what of network addresses? 192.168.1.1 what is the mapping there?
(02:17:41 PM) jjohansen: there is indeed some mapping for network, but its more like all this addresses bridge to this single address
(02:17:51 PM) jdstrand: the variable setting sounds useful. it isn't clear what benefit there is to not always doing it. how much effort will this take?
(02:17:56 PM) jjohansen: and for pids its this range to this root range
(02:18:09 PM) sarnold: pids?? eek
(02:18:20 PM) jjohansen: jdstrand: I am working on it, it is required to get lxc working
(02:18:33 PM) jdstrand: ok
(02:18:52 PM) jdstrand: so, what is the question again?
(02:18:57 PM) jjohansen: jdstrand: the question is whether we default to it, and break current behavior OR have some syntaxtic sugar around it
(02:19:33 PM) jjohansen: sorry I felt a long winded explanation was inorder before I asked the question
(02:19:42 PM) jdstrand: so the syntactic sugar means we have current pivotroot behavious *unless* we give the variable?
(02:19:56 PM) jjohansen: yes
(02:20:13 PM) jjohansen: or what ever the syntax is we decide to roll with
(02:20:24 PM) jdstrand: if the current behavior wasn't so broken, I'd be very much for the sugar
(02:20:56 PM) jdstrand: but with it being broken, it is a harder decision. if we broke it, what would need to change-- anything that uses attach_disconnected?
(02:20:56 PM) jjohansen: the caveat is we have rolled out the current behavior, in a lot of places
(02:21:13 PM) jjohansen: see previous notes about apache hats, and phone profiles
(02:21:23 PM) jjohansen: and the variable change will break those
(02:22:00 PM) jjohansen: yes anything using attach_disconnected will likely need modification. Of course it depends on how wide the profile is
(02:22:36 PM) jdstrand: it sounds like this is primarily kernel side
(02:22:57 PM) cboltz: at least for nscd, maybe we could fix it for everybody in abstractions/nameservice?
(02:23:07 PM) jjohansen: most of the work is kernel side, yes. But it has a very real affect on policy. Hence the question
(02:23:17 PM) jdstrand: sure
(02:23:32 PM) jjohansen: if I could hide it completely in the kernel I wouldn't be asking
(02:23:35 PM) jdstrand: I was trying to think about all the cases
(02:24:26 PM) jdstrand: with the phone, we can regenerate the policy on first boot after flash, so in theory, it isn't a problem, so long as the cache is invalidated properly when trying different kernels
(02:24:39 PM) jdstrand: meh, but no, we want to remove the flag
(02:24:57 PM) sarnold: we might not be able to get rid of the flag right away.. :/
(02:25:49 PM) jdstrand: I think we can manage the phone to some extent
(02:26:15 PM) jdstrand: version 1.0 of the policy could continue to use attach_disconnected, and 1.1 would not
(02:26:31 PM) jdstrand: (I'm talking about our phone policy, not apparmor upstream policy)
(02:26:35 PM) jjohansen: sarnold: no we won't be able to get rid of the attach_disconnect flag, this actually deals with a different subclass of the problem
(02:26:56 PM) jdstrand: we'd need to synchronize our patch set for our reference kernels
(02:27:06 PM) jdstrand: oh, really?
(02:27:23 PM) jdstrand: so, the phone policy would still need attach_disconnected?
(02:27:45 PM) jjohansen: sadly yes, until delegation and labeling land
(02:28:04 PM) jjohansen: sorry if I didn't make that clear
(02:28:07 PM) jdstrand: so phone policy only uses it for 1 access iirc
(02:28:33 PM) jjohansen: there will still be cases of things not visible within the child namespace and no way to map them
(02:29:00 PM) jjohansen: this only deals with mapping sub ns post pivot root to original profile
(02:29:03 PM) jdstrand: /dev/socket/property_service rw,
(02:29:12 PM) jdstrand: that shows up as dev/socket/property_service
(02:30:45 PM) jdstrand: ok, so if attach_disconnected has to stay regardless I'm not clear on the policy that would need to change
(02:30:46 PM) jjohansen: cboltz: how bad would breaking current behavior be for you?
(02:31:20 PM) jjohansen: jdstrand: rules within the profile will need to change
(02:31:44 PM) jdstrand: but which rules?
(02:31:54 PM) jjohansen: currently post pivot root are getting aliased down so they may look like
(02:32:03 PM) jjohansen:    /etc/foo r,
(02:32:32 PM) cboltz: jjohansen: good question - based on the number of apache hats I have it sounds like quite some work
(02:32:33 PM) jdstrand: oh, you mean if /var/../foo/bar and /foo/bar got mashed together, we are demashing so we might have something that was /foo/bar need to change to /var/.../foo/bar?
(02:32:44 PM) cboltz: (which hopefully can be scripted)
(02:32:49 PM) jdstrand: sorry if I am being dense-- I don't profile this type of policy often
(02:32:58 PM) jdstrand: s/profile/write/
(02:32:58 PM) jjohansen: but with the root properly tracked, the now look like
(02:32:58 PM) jjohansen:   /var/lxc-container/newroot/etc/foo
(02:33:19 PM) jjohansen: jdstrand: yep thats it
(02:33:59 PM) jdstrand: ok, so, for the phone, we can manage it easily actually-- we leave attach_disconnected, and the have *both*:
(02:34:02 PM) jdstrand:   /dev/socket/property_service rw,
(02:34:13 PM) jdstrand:   /var/lxc/.../dev/socket/property_service rw,
(02:34:19 PM) jdstrand: iiuc
(02:34:38 PM) jdstrand: jjohansen: is that accurate ^
(02:35:25 PM) jdstrand: minimal policy would only have /var/lxc/.../dev/socket/property_service rw,
(02:35:25 PM) jjohansen: jdstrand: yes, that will make a broader profile than is needed but is all we can do automatically
(02:35:31 PM) jdstrand: but transition policy could have both
(02:35:55 PM) jdstrand: ok-- then we can manage the phone fine if we break it
(02:36:15 PM) jdstrand: between that ^ and our easyprof policy versioning
(02:36:22 PM) kshitij8_ [~kshitij@117.211.91.58] entered the room.
(02:36:26 PM) jdstrand: ok, good, progress!
(02:36:38 PM) jdstrand: now, what have the lxc guys said they want?
(02:36:52 PM) sarnold: ponies
(02:37:03 PM) jdstrand: do they care if we break things, or will they just handle it in their policy?
(02:37:10 PM) jjohansen: yes ponies, they just want it to magically work
(02:37:29 PM) jdstrand: and it sounds like it will, soon, with what you are doing
(02:37:30 PM) jjohansen: if they have to add some minor syntax to profiles to get it to work they will live with it
(02:37:36 PM) jdstrand: ok
(02:37:55 PM) jdstrand: so Ubuntu easyprof and lxc don't mind breaking
(02:38:07 PM) jdstrand: what about backport kernels?
(02:38:53 PM) jjohansen: oh *%$!!!!
(02:38:59 PM) ***jjohansen will add the sugar
(02:39:08 PM) jjohansen: moving on
(02:39:10 PM) jdstrand: heh
(02:39:17 PM) jdstrand: I should have clearly started with that :P
(02:39:36 PM) jjohansen: are there any other issues to discuss
(02:41:55 PM) cboltz: a small question about aa-complain
(02:41:57 PM) kshitij8 left the room (quit: Ping timeout: 480 seconds).
(02:42:25 PM) cboltz: currently it adds the complain flag _and_ creates a force-complain symlink
(02:42:31 PM) cboltz: one of them is obviously enough ;-)
(02:42:39 PM) cboltz: which one do you prefer?
(02:42:48 PM) cboltz: should we have a config option for that?
(02:43:56 PM) sbeattie: I'd prefer not to have a config option, we should just make a decision.
(02:44:17 PM) sarnold: I could see a command line option but even that seems .. well, it'd be two mechanisms to maintain
(02:44:27 PM) cboltz: let me summarize the pros and cons:
(02:44:31 PM) sbeattie: cboltz: does suse include the force-complain directory?
(02:44:48 PM) cboltz: the symlink is packaging-friendly, but breaks caching
(02:45:08 PM) cboltz: the flag is cache-friendly, but causes *.rmpnew files on the next update
(02:45:10 PM) jjohansen: no suse does not have the force-complain directory
(02:45:26 PM) cboltz: adding it is easy ;-)
(02:45:45 PM) jjohansen: sure
(02:46:15 PM) cboltz: (and IIRC there's an ubuntu patch that someone wanted to upstream which would let the Makefile create it)
(02:46:36 PM) jdstrand: I very much prefer we not modify the policy with the flag, otherwise deb packaging will regress
(02:47:12 PM) cboltz: jdstrand: it won't regress - the "old" aa-complain just added the flag
(02:47:53 PM) cboltz: the force-complain symlink basically is an improvement, but I don't like the "breaks caching" part too much
(02:47:54 PM) jdstrand: hmm
(02:47:59 PM) jjohansen: ubuntu might have had a patch for that
(02:48:06 PM) jdstrand: maybe not
(02:48:17 PM) jdstrand: we might have just used the symlink directory
(02:48:39 PM) jdstrand: and I might have been confusing aa-complain with aa-disable
(02:50:02 PM) jdstrand: so, aa-enforce would just remove the symlink in the force-complain directory if it existed
(02:50:09 PM) sbeattie: cboltz: I think with the versioning change that jjohansen wants, force-complain actually improves caching, in that we wouldn't need to recompile policy upon switching between enforce and complain, just load the blob with a different header.
(02:50:44 PM) cboltz: sbeattie: agreed, but I doubt we'll get that into 2.9 ;-)
(02:50:46 PM) jjohansen: yes it would fix it, once its implement
(02:50:52 PM) jjohansen: ed
(02:51:05 PM) sbeattie: jdstrand: well, not entirely. I think the tools will need to take into account manually setting the complain flag.
(02:51:08 PM) jdstrand: ok, I was wrong
(02:51:11 PM) jjohansen: cboltz: not without a new kernel no
(02:51:26 PM) jdstrand: sbeattie: no, I was saying current behavior
(02:51:51 PM) jdstrand: I wasn't clear. I was figuring out if I was wrong and if things would regress
(02:51:58 PM) jdstrand: I was and they would not
(02:52:19 PM) jdstrand: so I rescind my preference
(02:56:55 PM) jdstrand: ok, I remember now. aa-complain would set the flag in the profile. That is something the admin can do and so the admin is not surprised by a change in policy on upgrades (ie, that is policy compliant)
(02:57:28 PM) jjohansen: I suggest for the time being we keep the current behavior
(02:57:45 PM) jdstrand: there were times that as packagers we wanted to force-complain mode-- without having to modify the policy, so we introduced the force-complain directory
(02:57:45 PM) jjohansen: I don't want to rush on any changes around this
(02:57:52 PM) sarnold: keep both flags=(complain) -and- the symlink dir?
(02:57:57 PM) jjohansen: yes
(02:58:05 PM) jjohansen: they are both currently in use
(02:58:19 PM) cboltz: sarnold: allowing both is fine
(02:58:21 PM) jjohansen: removing one will cause breakage that has to be dealt with
(02:58:41 PM) cboltz: but letting aa-complain _do_ both is superfluous and gives us the disadvantages of both
(02:58:45 PM) jdstrand: I think cboltz isn't asking if we should drop one or the other (correct me if I am wrong), but if aa-complain should actually treat both equally, cause now it does not
(02:58:59 PM) jdstrand: I'm fine with that
(02:59:10 PM) sarnold: cboltz: we're stuck with the disadvantages of both as it is. gah.
(02:59:49 PM) cboltz: so the make the question clear: should aa-complain a) add the complain flag or b) create a force-complain symlink?
(03:00:13 PM) cboltz: (the "old" aa-complain used the a) method)
(03:00:22 PM) jdstrand: if we choose not to have aa-complain do both, then I'd like to have the policy load portions of userspace still honor the directory, at least for the next Ubuntu LTS
(03:00:44 PM) jjohansen: jdstrand: I'm not changing the parser, it supports both
(03:00:53 PM) jdstrand: ok good
(03:01:08 PM) jdstrand: I always thought modifying the profile was weird
(03:01:39 PM) ***jdstrand is not talking about packaging-- that is discussed already)
(03:02:03 PM) jdstrand: though I can see both sides of the argument
(03:02:45 PM) jjohansen: heh, and I always thought storing the meta data of state else where was weird but ;-)
(03:03:05 PM) jdstrand: I'm fine with either. it seems a not important change to introduce potentially a lot of work before the Ubuntu LTS-- I'm guessing changing it right before the release for suse is not ideal either
(03:03:57 PM) ***jdstrand votes for current behavior with the option to change once the kernel caching improvements are implemented if it makes sense to do so
(03:04:34 PM) sbeattie: that seems reasonable
(03:05:35 PM) cboltz: sounds like I should send a patch that comments out the symlink creation ;-)
(03:06:05 PM) cboltz: FYI: both methods work (I'm using force-complain symlinks on some installations, and also use the complain flag on others), so you don't need to be afraid of breaking anything
(03:06:19 PM) kshitij [~kshitij@117.211.91.58] entered the room.
(03:06:32 PM) jjohansen: cboltz: then I wouldn't even bother changing it
(03:06:55 PM) jjohansen: it seems like a none issue atm
(03:07:04 PM) jjohansen: the parser supports both
(03:07:15 PM) jjohansen: yes its a little confusing that either could be used
(03:07:34 PM) jjohansen: but we have that issue already, as people can manually do either
(03:08:16 PM) jjohansen: alright anything else
(03:10:43 PM) sbeattie: I don't have anything.
(03:12:31 PM) ***jdstrand doesn't have anything
(03:12:35 PM) kshitij8_ left the room (quit: Ping timeout: 480 seconds).
(03:12:50 PM) jjohansen: alright I propose we skip the march meeting. 2 weeks between meetings just seems too short, and have the next meeting Tuesday April 8 @20:00 UTC
(03:13:02 PM) jdstrand: sounds good
(03:13:05 PM) sarnold: good idea
(03:13:17 PM) jjohansen: okay good enough for me
(03:13:21 PM) jjohansen: thanks everyone
(03:13:25 PM) jjohansen: meeting adjourned
``` 
