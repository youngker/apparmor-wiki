 ```
(11:01:34 AM) cboltz: meeting time ;-)
(11:02:32 AM) jjohansen1: hey cboltz
(11:03:03 AM) sarnold: aren't you about three weeks early?
(11:03:33 AM) jjohansen1: maybe, cboltz do you have anything for the agenda?
(11:03:57 AM) cboltz: I think so, but nothing too big ;-)
(11:04:05 AM) jjohansen1: okay
(11:04:19 AM) cboltz: first idea - should we change all "#include" to "include" in the profiles we ship?
(11:04:26 AM) jjohansen1: sbeattie, sarnold, jdstrand: meeting time
(11:04:43 AM) jjohansen1: cboltz: okay you hve the floor
(11:05:00 AM) jjohansen1: +1
(11:05:41 AM) cboltz: well, my line above more or less says it all ;-) - and since we already changed all profiles for the "include if exists <local/...>" it wouldn't additional more rpmnew files ;-)
(11:05:45 AM) sarnold: +1
(11:06:07 AM) cboltz: ok, looks like I should prepare a MR later ;-)
(11:06:32 AM) jjohansen1: yeah, I don't think we are going to get anyone else today
(11:07:16 AM) cboltz: speaking of MRs - it would be nice if someone can review 562 (not too big, only +67/-88)
(11:07:49 AM) jjohansen1: cboltz: yeah, I will get to your outstanding merges, you want 562 prioritized?
(11:09:05 AM) cboltz: currently it's the only open one (well, at least the only one with a chance to get merged - 557 looks more like the beginning of a can of worms I'll leave for you ;-)
(11:09:19 AM) jdstrand: tbh, I can't recall why we've waited so long to move away from #include. was it to ease backports?
(11:09:35 AM) cboltz: I'd guess "because nobody did it" ;-)
(11:09:48 AM) jjohansen1: jdstrand: nope include has been supported since 2008
(11:10:25 AM) jjohansen1: well there was the argument that
(11:10:42 AM) jjohansen1: #include gave clues to how it behaved
(11:10:59 AM) jjohansen1: as it is similar to the C include mechanism
(11:11:25 AM) jjohansen1: but different enough to also make it frustrating for those who know that
(11:11:26 AM) sarnold: "not making backports hard" is my guess
(11:11:50 AM) jjohansen1: sarnold: but again, supported since apparmor 2.3
(11:12:08 AM) jjohansen1: doh, /me is stupid
(11:12:12 AM) sarnold: jjohansen1: oh sure, just from the mechanical aspect of making minimal diffs harder to prepare, etc
(11:12:16 AM) jjohansen1: yeah, the profiles themselves
(11:12:23 AM) jjohansen1: yep
(11:12:29 AM) jdstrand: I recall that now. that is pretty weak. though, I can say that if our profiles have 'include <abstractions/foo>', an admin might try to comment that out with '#include ...'
(11:12:54 AM) cboltz: I have a feeling that backporting from current master to anything earlier will be more interesting[tm] than from 2.13 to 2.12 or 2.11 in various areas, so making it (also) harder for profiles doesn't matter too much ;-)
(11:13:23 AM) jjohansen1: jdstrand: yeah that is possible, at some point we should make #include default to issuing a warning
(11:14:01 AM) cboltz: right, but not in 3.0
(11:14:25 AM) jjohansen1: right
(11:14:32 AM) jjohansen1: maybe 3.1 this fall
(11:14:35 AM) jdstrand: jjohansen1: yeah, I can see the bugs coming in for it being noisy :\
(11:14:39 AM) jdstrand: but yeah
(11:15:14 AM) cboltz: with 3.1 in fall, when will we release 3.0? ;-)
(11:15:46 AM) earthundead left the room (quit: Remote host closed the connection).
(11:15:59 AM) jjohansen1: jdstrand: well I was thinking its a good target for the revised --warn and --Werror, make it possible in the parser, and set it in the config so its easy to change
(11:16:18 AM) jjohansen1: cboltz: at this rate? next year
(11:16:32 AM) jjohansen1: but you have to have ambitions
(11:16:45 AM) jjohansen1: really, this month
(11:17:06 AM) jjohansen1: we could possibly cut something as early has the end of the week
(11:17:41 AM) jdstrand: jjohansen1: that would be nice. it sounds like a requirement for warning by default. I don't necessarily think this should block moving to 'include', but without that infra, it does beg the question why we are adding a point of friction now when #include isn't hurting anyone
(11:17:41 AM) jjohansen1: it will depend on how long it takes me to fix the dfa backend permission compile
(11:18:00 AM) jdstrand: did I just recall the historical objection?
(11:18:00 AM) jjohansen1: with the other interrupts/priorities that come in
(11:18:25 AM) jjohansen1: jdstrand: possibly
(11:18:46 AM) jdstrand: wasn't that nice of me :)
(11:18:58 AM) jjohansen1: jdstrand: I would also say we aren't necessarily targeting the move from #include to include for the 3.0 release
(11:19:06 AM) jjohansen1: :)
(11:19:35 AM) jdstrand: :)
(11:19:37 AM) jjohansen1: cboltz: can we move on to the next topic
(11:20:07 AM) cboltz: well, changing all profiles and abstractions is a boring mass-change - I can probably do that later or tomorrow, so I do indeed expect the move to 'include' in 3.0 ;-)
(11:20:42 AM) cboltz: and you already answered my next question, which would have been the 3.0 release date
(11:21:39 AM) jjohansen1: the current hold up on my end is getting a fix to the network patch which requires some backend dfa work
(11:22:11 AM) jjohansen1: sadly I have just had higher priority interrupts the last couple of weeks
(11:22:21 AM) cboltz: network reminds me me to two abi questions:
(11:22:40 AM) cboltz: a) I guess aa-genprof should include an abi rule in new profiles - which one?
(11:23:03 AM) cboltz: b) what will happen with network rules in profiles without abi rule on the openSUSE kernel?
(11:23:11 AM) jjohansen1: I would assume we start with the only one we are shipping
(11:23:52 AM) jjohansen1: cboltz: so the 3.0 abi, cheats a bit in that it includes support for both v7 and v8 networking
(11:24:22 AM) jjohansen1: the parser checks the policy abi and the kernel abi to figure out what it is actually going to use
(11:24:43 AM) cboltz: yes, that's the easy part
(11:24:53 AM) cboltz: what about profiles that _don't_ have an abi rule?
(11:24:54 AM) jjohansen1: so it should just work on openSUSE kernel
(11:26:35 AM) jjohansen1: hrmmm good point, those will actually not have networking
(11:26:42 AM) jjohansen1: which is bad for ubuntu too
(11:27:08 AM) jjohansen1: we could include out of tree v7 network support in the default features abi
(11:27:35 AM) jjohansen1: that wouldn't break upstream kernels, avoiding our big concern
(11:27:50 AM) jjohansen1: but still support those using out of tree patches
(11:29:10 AM) cboltz: sounds like a good idea
(11:29:50 AM) jjohansen1: okay I will get a patch together for that
(11:29:56 AM) jjohansen1: next topic
(11:30:30 AM) jjohansen1: cboltz: anything?
(11:30:43 AM) cboltz: no, I had enough ;-)
(11:31:02 AM) jjohansen1: okay, so I will go with 3.1 planning
(11:31:14 AM) jjohansen1: the current goal is to drop it this fall
(11:31:43 AM) jjohansen1: make it a more limited release in some ways
(11:31:56 AM) jjohansen1: with things planned for 3.0 that just didn't make it
(11:32:04 AM) jjohansen1: currently on the list are
(11:32:23 AM) jjohansen1: --Werror  to turn warnings into errors
(11:32:35 AM) jjohansen1: policy overlay support
(11:33:23 AM) jjohansen1: better systemd integration, which is yet to be determined partly by the current upstream systemd discussion
(11:33:51 AM) jjohansen1: but I would assume some rudimentary tooling to help build an early boot cache
(11:34:19 AM) jjohansen1: v8 abi changes for af_unix
(11:34:51 AM) jjohansen1: the fd interface api
(11:35:00 AM) jjohansen1: (libapparmor)
(11:35:44 AM) jjohansen1: kill, user, unconfined flag modes
(11:35:56 AM) jjohansen1: possibly audit control flags
(11:36:30 AM) cboltz: just wondering - what's the blocker for kill mode?
(11:36:35 AM) jjohansen1: extended rule permission qualifier support (eg. prompt, access, ...)
(11:37:04 AM) jjohansen1: we could land the current patch which supports current kernels
(11:37:32 AM) jjohansen1: but, the kernel side is not always correct, at best we could call it experimental
(11:38:08 AM) jjohansen1: also current kernels have a bug, where named unconfined profiles
(11:38:10 AM) jjohansen1: eg.
(11:38:23 AM) jjohansen1:    systemd (unconfined)
(11:38:36 AM) jjohansen1: actually have their mode hidden, so you get
(11:38:44 AM) jjohansen1:    systemd (-)
(11:39:10 AM) jjohansen1: not a huge deal, and I am not opposed to landing the current patch and making it as experimental
(11:39:39 AM) jjohansen1: the other thing I would like to add is control of the signal
(11:39:59 AM) jdstrand: jjohansen1: that is quite the list!
(11:40:04 AM) jjohansen1: eg.
(11:40:04 AM) jjohansen1:   profile test flags=(kill kill.signal=stop)
(11:40:10 AM) jjohansen1: jdstrand: sort of
(11:40:37 AM) jjohansen1: most of it already exists in some form, we just aren't trying to land it in 3.0
(11:41:13 AM) jjohansen1: I actually have something like 50 branches with stuff that is WIP
(11:41:54 AM) jjohansen1: there are a couple of other things
(11:42:00 AM) ***cboltz already thought he has too many pending branches
(11:42:04 AM) jjohansen1: I would like to finally fix the x dominance
(11:42:44 AM) jjohansen1: increase the number of named x transitions
(11:43:11 AM) jjohansen1: and possibly land nested hats
(11:43:17 AM) jjohansen1: (parser side)
(11:43:33 AM) jjohansen1: err, I guess not hat, nested children
(11:43:48 AM) jjohansen1: the change_hat api doesn't allow for nested hats
(11:44:42 AM) cboltz: actually the tools are slowly getting closer to making nested children possible - but I won't promise a date yet because there's still some rewrite necessary before
(11:44:56 AM) cboltz: at least the "dependency chain" of needed changes is getting shorter
(11:46:46 AM) jjohansen1: right
(11:47:28 AM) jdstrand: ETOOMANYBRANCHES :)
(11:47:39 AM) jjohansen1: hahaha yeas
(11:48:56 AM) jjohansen1: cboltz: python tooling wise there isn't a lot for you to support, I will try to get WIP MRs up in the next month or so giving you time to actually work on the tooling for them
(11:49:21 AM) cboltz: I'm afraid reality disagrees ;-)
(11:49:42 AM) cboltz: currently the why aa-logprof etc. stores profiles internally only allows parent + child
(11:50:03 AM) cboltz: so to allow nested childs, I have to completely change the internal storage
(11:50:11 AM) jjohansen1: hahaha, oh I know you have a huge queue already
(11:50:45 AM) cboltz: + changing profile parsing to maintaining a stack
(11:51:00 AM) jjohansen1: and I know nested children are a nightmare, I did say "possibility" I know its something you are working towards
(11:51:16 AM) jjohansen1: and its something I am working on making the parser full support
(11:52:08 AM) jjohansen1: if it helps we will qualify it as an extremely unlikely possibility
(11:52:44 AM) cboltz: that would remove the motivation to do it right ;-)
(11:53:07 AM) jjohansen1: well if its motivation you want
(11:53:27 AM) cboltz: actually I already have a rough plan in /dev/brain, I "just" need to actually implement it step by step
(11:53:34 AM) jjohansen1: action: cboltz will deliver nested children profiles for 3.1
(11:53:35 AM) jjohansen1: :)
(11:54:05 AM) jjohansen1: jdstrand: do you have anything to add?
(11:54:22 AM) cboltz: another action: jjohansen1 will deliver everything he mentioned above for 3.1 in fall 2020
(11:54:24 AM) cboltz: ;-))
(11:55:01 AM) jjohansen1: well that is the plan :)
(11:55:35 AM) cboltz: we'll see if reality agrees...
(11:56:01 AM) jjohansen1: reality is a cruel mistress
(11:56:19 AM) jjohansen1: alright I think that is it
(11:56:37 AM) cboltz: side topic from me: do you have some   aa-status --json   example output with the new modes (unconfined, mixed, kill) so that the YaST team has something to work with?
(11:57:00 AM) jjohansen1: I would like to skip the July meeting unless something urgent comes up
(11:57:08 AM) sarnold: omg it's june already
(11:57:22 AM) jjohansen1: cboltz: yes I can provide some example output
(11:57:31 AM) cboltz: ok, thanks
(11:57:32 AM) jjohansen1: do you want it compressed or pretty printed
(11:58:25 AM) cboltz: just grepped the YaST code, it uses     status_output = command_output("/usr/sbin/aa-status", "--pretty-json")
(11:59:00 AM) jjohansen1: ack
(12:00:22 PM) cboltz: if you want, you can send it as a reply to the YaST mail I sent some weeks ago
(12:00:30 PM) jjohansen1: okay
(12:01:12 PM) jjohansen1: next meeting Tuesday August 11 @18:00 UTC
Unknown command.
(12:01:27 PM) jjohansen1: meeting adjourned
 ```
