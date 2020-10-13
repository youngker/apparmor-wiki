 ```
(11:01:01 AM) cboltz: meeting time ;-)
(11:01:49 AM) jjohansen: hey cboltz
(11:01:53 AM) jjohansen: yep
(11:02:59 AM) jjohansen: jdstrand, sbeattie, sarnold: meeting time
(11:03:37 AM) jjohansen: I don't know who will have time to make it today
(11:03:58 AM) sarnold: oof
(11:04:31 AM) jjohansen: I know, its never convenient
(11:04:35 AM) cboltz: maybe you shocked everybody by having "TODO" on the agenda - better don't remind me to my TODO list ;-)
(11:05:32 AM) jjohansen: haha
(11:05:42 AM) jjohansen: lets get started
(11:06:12 AM) jjohansen: I see the meeting agenda is still TODO, and I don't have anything
(11:06:27 AM) ***sbeattie is lurking
(11:06:36 AM) cboltz: any news about 2.13.5?
(11:07:00 AM) ***sbeattie assumes that's what jjohansen's latest flurry of activity is about.
(11:07:02 AM) jjohansen: yes, I was going to mention that. We are working on it, and hopefully it will release today
(11:07:12 AM) cboltz: :-)
(11:07:15 AM) jjohansen: there are still some testing failures to work through
(11:07:44 AM) sbeattie: also, can we do a final 2.10 release and make 2.11 the oldest version we'll backport to?
(11:08:13 AM) jjohansen: I have no objection to do that
(11:08:21 AM) jjohansen: cboltz?
(11:08:27 AM) cboltz: no objections
(11:09:28 AM) sbeattie: excellent, thanks. I'd like to keep our maintenance burden merely at absurd levels, not preposterious.
(11:09:40 AM) jjohansen: okay, lets schedule 2.10 as the next release to work on, and see if we can't get it out in the next week
(11:10:11 AM) cboltz: "can't"? ;-)
(11:11:18 AM) jjohansen: hehehe, hey I am setting the bar high
(11:12:33 AM) jjohansen: while we are on releases, what about 2.12 and 2.11
(11:12:48 AM) jjohansen: I don't have plans to cut a release on those yet
(11:13:33 AM) jjohansen: ideally I'd like to develop a time table we could point to for planned releases, especially on the maintenance side
(11:13:52 AM) jjohansen: with older releases getting longer between releases
(11:14:22 AM) jjohansen: I don't have a concrete proposal, just something to think about
(11:15:05 AM) sbeattie: yeah, I agree. 
(11:16:05 AM) jjohansen: For 3.0 we thankfully so far haven't had any critical issues, there have been several minor bug fixes so (if that holds) I am looking at maybe 3 months out for the first point release
(11:16:25 AM) jjohansen: so that would be early January
(11:16:29 AM) jjohansen: any objections
(11:16:43 AM) sbeattie: that would be fine.
(11:17:37 AM) jjohansen: lets move on
(11:17:40 AM) cboltz: assuming no critical bugs pop up, fine
(11:18:18 AM) jjohansen: yes, it is assuming not critical bugs, if there is something critical found we will try to get a release out within the week
(11:19:03 AM) jjohansen: does anyone have anything else they want to discuss?
(11:19:13 AM) sbeattie: AFAIK, we have not split off a 3.0 branch, correct? Does anyone have anything big to land soonish that would merit doing so?
(11:19:41 AM) jjohansen: sbeattie: we have, and upstream dev and 3.0 have already diverged
(11:19:52 AM) sbeattie: oh!
(11:20:02 AM) ***sbeattie does git spelunkery.
(11:20:43 AM) jjohansen: nothing big so far, but cboltz's patch around tests using whole message went to dev and has not been picked back to 3.0
(11:21:11 AM) jjohansen: I have the aa-load work to post out some time soon, when I time to get back to it
(11:21:39 AM) jjohansen: and there is a patch to allow you to specify a root for attach_disconnected
(11:21:53 AM) jjohansen: that one I might even consider for 3.0
(11:22:28 AM) cboltz: what will the syntax for that look like?
(11:22:29 AM) jjohansen: there is the whole set of fd based api calls, that should be refresshed and pushed sooner than later
(11:22:58 AM) jjohansen: cboltz: good question, that is being tinkered with still
(11:23:16 AM) jjohansen: there are a couple of options
(11:23:27 AM) jjohansen: attach_disconnected=/foo/bar
(11:23:38 AM) jjohansen: or
(11:23:48 AM) jjohansen: disconnected_root=/foo/bar
(11:24:11 AM) jjohansen: so whether we split the flag
(11:24:27 AM) jjohansen: I should note for audit we decided to split the flag
(11:24:52 AM) jjohansen: so not audit=all
(11:25:12 AM) jjohansen: but audit, and then another flag with a value of how to audit
(11:25:29 AM) jjohansen: I need to look back at what the value was
(11:25:40 AM) jjohansen: err not value but the new flag
(11:25:55 AM) jjohansen: audit.mode I think
(11:26:26 AM) cboltz: when doing that lookup, please also make sure there are a few samples in the parser simple_tests to ensure the tools don't break
(11:27:12 AM) jjohansen: cboltz: don't worry there will be tests/samples that you can work from
(11:27:34 AM) jjohansen: there are a couple other ones coming as well
(11:27:55 AM) jjohansen: kill.signal=stop
(11:27:55 AM) cboltz: for attach_disconnected, maybe attach_disconnected.path= (if you really want a separate flag, it should at least have the same prefix)
(11:28:13 AM) cboltz: but I'd also be fine with   attach_disconnected=/foo/bar
(11:28:19 AM) jjohansen: that wfm too
(11:29:07 AM) sarnold: something that used to be boolean-ish turning into an arbitrary-value-ish thing feels more complex than introducing a new name
(11:29:08 AM) jjohansen: well, I think we want to keep a pattern, audit and kill for various reasons are going witht eh kill.signal=X
(11:29:34 AM) jjohansen: so I think we should follow the pattern with attach_disconnected
(11:30:10 AM) jjohansen: it also makes it easy to add a separate var if for some reason in the future we ever deem it necessary
(11:30:28 AM) jjohansen: kill.foo=bar
(11:31:20 AM) jjohansen: I'd like to land some of these sooner than later
(11:32:02 AM) jjohansen: the attach_disconnected one is actually supported in the kernel since 3.17? (iirc)
(11:32:27 AM) jjohansen: it doesn't fix attach_disconnected's problems but it makes it slightly less bad
(11:32:42 AM) jjohansen: and since we aren't landing delegation soon ...
(11:33:01 AM) jjohansen: anything else to discuss?
(11:34:55 AM) cboltz: nothing from me
(11:35:16 AM) jdstrand: oh, root for attach_disconnected. interesting
(11:35:30 AM) ***jdstrand skimmed backscroll but is in and out
(11:35:55 AM) jjohansen: jdstrand: its one of the many patches that got cut from 3.0, just trying to get it out the door
(11:36:28 AM) jdstrand: jjohansen: so, is that a 3.0 point release or a 3.1 item? (I was thinking 3.1, but then wasn't sure from backscroll)
(11:37:38 AM) jjohansen: jdstrand: currently scheduled for 3.1 but is something we might consider cherry-picking back if requested by a distro
(11:38:10 AM) jjohansen: the need for attach_disconnected is a sore spot, and anything that makes it slightly less bad ...
(11:38:57 AM) cboltz: IIRC we labeled 3.0 as short lived release, so I'd hope that it only ends up in rolling release distros, not in something with LTS
(11:39:19 AM) cboltz: therefore I'd also say that we don't really need to backport that feature
(11:39:35 AM) cboltz: (unless the time to 3.1 is similar as the time to 3.0 ;-)
(11:39:44 AM) jjohansen: right, ideally we won't but like I said if a distro requested it, maybe
(11:39:46 AM) das_j: What is the ac mask in this denial? name="/dev/random" pid=3834438 comm="Catalina-utilit" requested_mask="ac" denied_mask="ac"
(11:39:46 AM) das_j: I can only see 'a' in the man apge
(11:39:55 AM) das_j: oh sorry it's the meeting
(11:40:04 AM) jjohansen: das_j: append and create
(11:40:23 AM) das_j: jjohansen: But I though those were mutually exclusive?
(11:40:42 AM) jjohansen: create can not currently specified in the language separate from write but the kernel does separate them
(11:41:08 AM) das_j: oh so I need two rules, one for a and one for w?
(11:41:35 AM) jjohansen: das_j: no, at least not from a permission request pov, there is the whole append to the existing file OR create a new one to append to
(11:41:35 AM) cboltz: no, w includes a
(11:42:11 AM) das_j: okay, so w it is. I don't even care anymore why my confluence wants to write to /dev/random
(11:42:18 AM) das_j: Thank you :)
(11:42:26 AM) sarnold: ha
(11:42:35 AM) jjohansen: note create only gives permission to create a file that doesn't exist, not to remove an existing file
(11:43:46 AM) jjohansen: cboltz: time line wise for 3.1 I would like something more like 6 months from 3.0
(11:44:36 AM) jjohansen: we will cut it with what has landed, it doesn't have the same time together that abi, networking, etc had
(11:45:13 AM) jjohansen: moving on
(11:45:23 AM) cboltz: ok, then I'll refrain from jokingly saying "so in 2 years?" ;-)
(11:45:51 AM) jjohansen: I propose we skip the November meeting, unless someone comes up with something that must be discussed
(11:46:04 AM) jjohansen: that would make the next meeting tuesday December 8
(11:47:14 AM) cboltz: ok
(11:48:49 AM) jjohansen: alright with that meeting adjourned
 ```
