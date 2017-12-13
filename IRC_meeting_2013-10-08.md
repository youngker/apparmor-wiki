```
(01:31:01 PM) jjohansen: jdstrand_, sbeattie, mdeslaur, sarnold, kees, cboltz, tyhicks: a short apparmor meeting if you can spare the time
(01:31:38 PM) mdeslaur: sure
(01:32:05 PM) sarnold: ack (with both meanings :)
(01:32:15 PM) jdstrand_ is now known as jdstrand
(01:32:28 PM) jjohansen: Alright lets get started
(01:32:46 PM) ***kshitij8 considers volunteering for looking into translation issue.
(01:33:49 PM) jjohansen: since kshitij8 already brought it up, we need to fix/revert some translations that where done by suse about 4 years ago
(01:34:06 PM) jjohansen: it seems that they are uhmm less than correct
(01:35:38 PM) jjohansen: kshitij8: you are welcome to look into this, I am not sure how much of a rabbit hole it will be
(01:36:30 PM) jjohansen: other wise, its going to have to go onto the to do list
(01:37:06 PM) kshitij8: I'm fine as long as I don't need to learn many languages to get it done.
(01:37:12 PM) cboltz: kshitij8: I'm quite sure you'll know how to solve merge conflicts after doing it ;-)
(01:37:40 PM) cboltz: you don't need to understand the languages - you "just" have to use "patch -R"
(01:38:02 PM) cboltz: and, biggest problem, fix any merge conflicts (by removing the translated text, so that translators get a second chance)
(01:38:54 PM) cboltz: bzr diff -r1224..1225   will give you the patch to use with   patch -R
(01:39:45 PM) cboltz: I already fixed the de.po file, so you can just skip it
(01:40:22 PM) cboltz: that's it
(01:40:30 PM) cboltz: I hope it's at easy as it sounds ;-)
(01:41:14 PM) kshitij8: I hope too. :-)
(01:41:54 PM) kshitij8: look-out for a "sos: translation" on the ml otherwise. ;-)
(01:42:16 PM) cboltz: ;-)
(01:43:12 PM) cboltz: next topic?
(01:44:03 PM) jjohansen: alright so we need to figure out how/when to land the new set of tools from kshitij8
(01:44:09 PM) cboltz: jjohansen: maybe you should give a short update on what apparmor version etc. will be in the next ubuntu release
(01:44:18 PM) jjohansen: right
(01:44:22 PM) cboltz: because knowing that is a precondition ;-)
(01:44:27 PM) jjohansen: so aa3 didn't happen
(01:45:05 PM) jjohansen: the next ubuntu release is shipping with aa3 alpha4, and a heavily patched 2.8 userspace
(01:45:37 PM) jjohansen: aa3 is going to be delayed a bit
(01:46:00 PM) jjohansen: how much is yet to be determined, but we do need to land a beta soon
(01:46:24 PM) cboltz: can you define "heavily patched 2.8 userspace"? (does it mean "current 2.8 branch?")
(01:46:40 PM) jjohansen: cboltz: sadly no
(01:47:23 PM) jdstrand: 2.8 + dbus mostly, but with a bunch of bug fixes from trunk
(01:47:33 PM) jjohansen: it started with either a 2.8 base and has been cherry-picking patches and adding ubuntu specific ones
(01:48:17 PM) tyhicks: everything appropriate has been flowing upstream to trunk
(01:48:19 PM) jdstrand: we seriously need to get the Ubuntu package cleaned up
(01:48:22 PM) jjohansen: right so it has the dbus stuff, some of the 3.0 patches to work with the new kernel, the cache patches, ...
(01:48:55 PM) jjohansen: yes, and we need to verify everything appropriate has actually been pushed back up to trunk
(01:49:08 PM) jjohansen: there have been a lot of patches flying around lately
(01:49:22 PM) mdeslaur: DANGER: flying patches
(01:49:23 PM) tyhicks: true - I do have a couple recent ones that I need to send to the list
(01:50:00 PM) jjohansen: I know there have been some easy prof patches that might need to flow up, etc too
(01:51:54 PM) jjohansen: any ways
(01:52:28 PM) jjohansen: I think there is time to land the genprof, et al into aa3
(01:52:41 PM) jjohansen: jdstrand, sbeattie, sarnold^?
(01:52:59 PM) sarnold: sure
(01:53:07 PM) ***sarnold <-- always the optimist
(01:53:14 PM) jjohansen: with how broken the old genprof has become I don't really see this as a problem
(01:53:26 PM) sarnold: new broken vs old broken, might just be a wash :)
(01:53:35 PM) jjohansen: exactly :)
(01:53:54 PM) sarnold: at least new broken would be fun and exciting and kshitij8 it still around and interested, so the sooner the better, probably.
(01:54:06 PM) jdstrand: I don't think there is anything easyprof, but who knows, we are at the 76 patch mark :P
(01:54:11 PM) jjohansen: and if Mr. Murphy would look the other way for once, it might actually be better
(01:54:17 PM) sarnold: jdstrand: 76? wow.
(01:54:30 PM) jdstrand: there is an ubuntu-helpers that I didn't send up yet, but that is, well, ubuntu specific anyway
(01:54:42 PM) jdstrand: sarnold: I think there may be some holes in there
(01:54:59 PM) sarnold: jdstrand: ah yes, a good BASIC programmer leaves some line numbers blank :)
(01:55:05 PM) jdstrand: not many-- 71 patches :)
(01:55:09 PM) sarnold: haha
(01:55:16 PM) jdstrand: just enough to keep things interesting
(01:55:26 PM) ***cboltz wonders how many people follow the "golden rules of bad programming" - and I only talked about 50 patches...
(01:55:27 PM) sarnold: jjohansen: do you have a date in mind for aiming for 3.0?
(01:55:42 PM) jdstrand: as for genprof-- you mean kshitij8's work?
(01:55:54 PM) jjohansen: jdstrand: yes
(01:55:56 PM) sarnold: jdstrand: yes
(01:56:07 PM) jdstrand: cboltz: believe me, it wasn't the intent. we want aa3
(01:56:11 PM) jjohansen: sarnold: beta or prebeta in November
(01:56:18 PM) sarnold: jjohansen: makes sense
(01:56:21 PM) jdstrand: desperately
(01:57:01 PM) jdstrand: I'm ok with pushing genprof-- I haven't personally tested them, but I'd love to get all the duplicated code out of the various python utilities
(01:57:13 PM) jdstrand: and genprof is the first step in that direction
(01:57:38 PM) jjohansen: prebeta because I know there will be some features missing, its just a matter of determining whether we cut them or roll them in late
(01:58:07 PM) jjohansen: we will just have to see where we are at
(01:58:31 PM) cboltz: jdstrand: kshitij8's work contains more aa-* tools - I didn't check, but we can probably remove all perl tools
(01:58:50 PM) jdstrand: that's awesome
(01:59:04 PM) cboltz: well, as soon as we have a new yast module - before that, we'll need to keep the perl modules
(01:59:05 PM) kshitij8: I've had my little break. I plan to get the tools in better shape as soon as I can.
(01:59:32 PM) jdstrand: aa-easyprof can start using the library, and then w can have Ubuntu's click-apparmor do the same
(01:59:34 PM) sarnold: indeed, I can't wait to remove the 28-odd megabytes of perl requirements from our tool chain -and- get something easier to modify in the process :)
(01:59:53 PM) kshitij8: and the yast module. The JSON-Ruby-Python idea was really interesting.
(02:00:18 PM) jdstrand: kshitij8: to be clear, I don't think moving aa-easyprof to the libraries is a requirement for acceptance into trunk
(02:00:33 PM) jjohansen: cboltz: we can start by moving those tools to deprecated, they need to stay in for at least one release as deprecated
(02:00:34 PM) jdstrand: kshitij8: that said, I certainly wouldn't be opposed to patches that did that :)
(02:01:17 PM) jjohansen: of course distros are free to patch away deprecated :)
(02:01:51 PM) kshitij8: when is the next release expected for aa2 (just to get an approx timeline for tools)?
(02:02:20 PM) jdstrand: (btw, I said Ubuntu's click-apparmor, but click and click-apparmor are not Ubuntu-specific-- it is just the only place that has them now)
(02:02:30 PM) jjohansen: kshitij8: we expect an aa2.8.3 release before year end, your tools won't go into that
(02:02:52 PM) jjohansen: jdstrand: right that another thing we should land upstream
(02:03:01 PM) sarnold: it'll be mostly profile fiddling and maybe translation fixes, as I can think of 2.8.3 now..
(02:04:14 PM) kshitij8: that seems okay to me. I'll have a lot of time to get the tools in better shape and test them too (hopefully have a YAST-UI in place too.)
(02:05:09 PM) ***kshitij8 bumps up the translation fix work on To-Do list.
(02:07:06 PM) jjohansen: sarnold: there have been a few bug fixes besides that, and the plan has always been to do a quick 2.8.3 with those
(02:07:50 PM) jjohansen: the real question is whether a 2.8.3 before 3.0 is enough, or whether we should roll a 2.9 with mount and dbus
(02:08:16 PM) jjohansen: I inclined not to add that extra release because its more work
(02:08:18 PM) sarnold: 2.9 clearly labeled as an experiment with feedback desired would be nice ...
(02:08:34 PM) jdstrand: those are conflicting :P
(02:08:37 PM) sarnold: .. but something that we can stop supporting when we release 3.0, and just do 2.8 and 3.0....
(02:08:42 PM) jdstrand: I'm inclined to agree with jjohansen 
(02:08:47 PM) cboltz: counter-question: is there a distribution that would need a 2.9 schedule-wise?
(02:09:02 PM) jjohansen: cboltz: suse?
(02:09:16 PM) jdstrand: why wouldn't 3.0 be ready in time?
(02:09:31 PM) jjohansen: arch, and gentoo don't they are rolling releases
(02:09:36 PM) cboltz: I'd say no - 13.1 RC1 will be released in two days (with 2.8.2 + patches from 2.8 branch)
(02:09:37 PM) jdstrand: isn't it going to be ready for beta in a couple/few weeks?
(02:09:43 PM) jjohansen: any ubuntu derived distro wouldn't
(02:10:08 PM) cboltz: so we have some months to aim for 13.2
(02:10:22 PM) jjohansen: cboltz: okay, I just wasn't sure of the schedule there
(02:10:54 PM) jjohansen: basically I am not aware of any distro that would need a 2.9 release
(02:11:04 PM) jdstrand: maybe I am conflating IPC with aa3
(02:11:26 PM) jjohansen: sarnold: we already have an experimental release called saucy that will require more support than I like
(02:11:43 PM) sarnold: jjohansen: fair point :)
(02:11:43 PM) jjohansen: so I am not inclined to add a generic 2.9
(02:11:57 PM) jjohansen: jdstrand: nope, they really are tied
(02:12:22 PM) ***cboltz wasn't brave enough to push aa3 alpha to opensuse
(02:12:26 PM) jjohansen: think of it as the wider rule set with some new semantics
(02:12:36 PM) jjohansen: cboltz: was wise
(02:13:58 PM) jjohansen: oh and debian won't need a 2.9 because they won't patch the kernel so what is in userspace just needs to work with upstream kernels
(02:14:21 PM) jjohansen: alright I would say that is no to 2.9
(02:14:42 PM) jjohansen: next question who is going to handle landing kshitij8 new code
(02:14:59 PM) jjohansen: cboltz: is this something you could/would do?
(02:15:49 PM) cboltz: can/should we import the bzr history, or do you just want the final result?
(02:17:03 PM) jjohansen: I am happy with just the final result and history going forward
(02:17:44 PM) jjohansen: but I could see someone wanting history/provenance
(02:17:51 PM) kshitij8: Can we wait a couple of weeks before the import? I'd like to get some more work done before that.
(02:18:45 PM) jjohansen: kshitij8: yes, I'd say you could hold off until early November if you would like
(02:19:12 PM) jjohansen: but not much more than that as we need to be getting ready for a beta if possible
(02:19:19 PM) jdstrand: if you do a merge, it shows as one commit, but you can dig infor the history if needed
(02:19:19 PM) kshitij8: I think that would be ideal time-frame for me.
(02:21:55 PM) jjohansen: alright, the only thing I ask for in the merge is any existing tool/file being replaced is moved to deprecated so we can carry in the next release if someone finds they need it
(02:22:46 PM) cboltz: jdstrand: indeed, merge sounds like the best option
(02:23:09 PM) cboltz: this also means I won't do it because I don't know too much about "bzr merge" ;-)
(02:24:47 PM) ***cboltz wonders why everybody is silent now
(02:25:13 PM) jjohansen: cboltz: alright when your ready poke sarnold (because he is not here to defend himself) and we will get it merged
(02:25:17 PM) jjohansen: :)
(02:25:25 PM) cboltz: ;-)
(02:28:44 PM) jjohansen: alright does anyone have anything else?
(02:29:09 PM) mdeslaur: beware of flying patches
(02:29:12 PM) mdeslaur: that's all
(02:30:11 PM) jjohansen: hehe, no kidding I had a couple of patches smack me recently, caused a headache that lasted days
(02:30:39 PM) mdeslaur: sarnold got attacked by some patches last week, it was like a scene out of a Hitchcock movie
(02:31:29 PM) cboltz: jjohansen: you are supposed to read the patches - don't print them to make paper planes!
(02:31:29 PM) jjohansen: and here I thought it was more the zombie patchocalypse
(02:31:59 PM) jjohansen: cboltz: ah shoot but my dog like snatching those and chewing them
(02:32:57 PM) ***cboltz wonders if "ACK" means "the dog liked them"
(02:33:00 PM) kshitij8: can I add to the patch flood with some profile cleanups? (similar to the one cboltz pushed a last week)
(02:33:27 PM) jjohansen: oh one more thing, next meeting November 12 (2nd tuesday) 20:00 UTC. Any objections?
(02:33:37 PM) cboltz: kshitij8: yes, of course
(02:33:41 PM) jjohansen: kshitij8: please
(02:33:55 PM) kshitij8: :-)
(02:36:28 PM) jjohansen: alright with no objections to the next meeting time, I think its time to
sbeattie sbeattie_ 
(02:36:58 PM) jjohansen: meeting adjourned
```
