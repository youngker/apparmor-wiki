
(12:10:36 PM) jjohansen: kees, jdstrand, sarnold, sbeattie, tyhicks: if you can a quick meeting in #apparmor is happening
(12:10:49 PM) tyhicks: hello
(12:11:49 PM) jjohansen: alright so lets get started
(12:12:02 PM) jjohansen: sbeattie has been pulling together the patches for 2.8.1
(12:12:05 PM) ***sbeattie is here
(12:12:41 PM) jjohansen: cboltz: is there anything else that you would like to see pulled into 2.8.1?
(12:13:15 PM) sbeattie: (or anyone else, for that matter)
(12:13:27 PM) ***jdstrand is here
(12:13:31 PM) cboltz: I didn't check all trunk commits, but I hope it's complete
(12:13:58 PM) cboltz: and I need it this week if possible ;-)
(12:14:31 PM) sbeattie: I walked through the trunk commits, which was where I got my list of nominations from.
(12:15:03 PM) sbeattie: there were some others around python3 support that I didn't nominate.
(12:15:45 PM) sbeattie: but yeah, I'm relatively satisfied with where the 2.8 branch is.
(12:16:08 PM) kees: \o
(12:16:18 PM) jjohansen: sbeattie: you finished pulling in what you could from ubuntu to trunk first?
(12:16:49 PM) jjohansen: hey kees, was that an I'm here or I'd like to comment on 2.8.1
(12:17:06 PM) kees: sorry, wanted to show I was here. 2.8.1 is looking good to me.
(12:17:43 PM) jjohansen: okay thanks
(12:18:10 PM) sbeattie: jjohansen: yeah, I looked through the ubuntu patches as well, and nominated some of what I saw. Several are just cherrypicked stuff that's already upstream.
(12:18:20 PM) jjohansen: cboltz: okay, I will add the log parsing patch and roll a 2.8.1 candidate 1
(12:18:54 PM) jjohansen: sbeattie: thats what I thought and thanks
(12:18:58 PM) cboltz: :-)
(12:19:14 PM) jjohansen: alright lets move on
(12:19:32 PM) cboltz: (not sure if we need a "candidate" - just name it 2.8.1 ;-)
(12:20:24 PM) jjohansen: cboltz: heh I guess we can do that, and if any show stoppers show up we can roll a 2.8.2
(12:20:38 PM) cboltz: exactly
(12:20:42 PM) sbeattie: well, it'd be useful to do some smoke-testing so we don't have to release a quick 2.8.2
(12:21:21 PM) sbeattie: but, eh, if that's the way you guys want to go with it, that's cool, too.
(12:21:44 PM) jjohansen: sbeattie: I don't really care, either way works for me
(12:22:20 PM) jjohansen: alright so, I haven't done the wiki update request yet, sorry
(12:22:31 PM) jjohansen: it keeps slipping my mind
(12:23:01 PM) jjohansen: now onto 3.9
(12:23:08 PM) jjohansen: s/3.9/3.0/
(12:23:28 PM) jjohansen: so no alpha has been released yet. Its coming this week
(12:23:51 PM) ***jjohansen is going to combine what was to be alpha1 and alpha2 into a very late alpha1
(12:25:04 PM) jjohansen: the dbus patches are looking alright so I will be including them in the alpha, with the caveat that if you want dbus support you need to build your own patched dbus
(12:25:20 PM) sbeattie: jjohansen: would it be easier on you if I did the 2.8.1 release, so you ca focus on the alpha?
(12:25:29 PM) sbeattie: s/ca /can /
(12:25:51 PM) jjohansen: sbeattie: sure, I am more than willing to hand off any work you want to take
(12:25:55 PM) jjohansen: :)
(12:26:03 PM) sbeattie: hehe
(12:26:12 PM) tyhicks: I've been running the dbus patches for almost a month now with pretty good success
(12:26:17 PM) jjohansen: sbeattie: even better do you want to take the alpha and I'll take 2.8.1 :)
(12:27:22 PM) jjohansen: yeah, and jdstrand has been having fun with them too. I've done some testing but nothing extensive
(12:27:38 PM) jdstrand: they are wonderful
(12:27:42 PM) sbeattie: jjohansen: well, whatever gets you back to doing productive development the soonest, I guess.
(12:28:06 PM) jdstrand: I am literally giddy when I see apparmor dbus denials. it is the strangest thing :)
(12:28:07 PM) jjohansen: cboltz: later in the week if your around I will probably poke you for some help on the opensuse build service
(12:28:36 PM) cboltz: I'll help you where I can
(12:28:38 PM) jjohansen: sbeattie: no, no you mis understood, I was offering to let you do the development :)
(12:28:44 PM) sbeattie: hah
(12:29:43 PM) jjohansen: with the 3.0 stuff running so late, and 2.8.1 pull back a lot of what is currently in trunk, I don't see a need for 2.9
(12:29:47 PM) jdstrand: I might add that others running the ppa packages and working on profiling with dbus would be welcome. I've got some ideas for some abstractions that I need to send to the list
(12:29:54 PM) jjohansen: To me its just not worth it
(12:29:56 PM) sbeattie: jjohansen: +1
(12:30:52 PM) jjohansen: cboltz: ?
(12:30:58 PM) cboltz: I agree - release 2.8.1 and then focus on 3.0 ;-)
(12:31:49 PM) jjohansen: okay the last item I have is the alpha3, now alpha2 target date
(12:32:17 PM) jjohansen: I was thinking two week from alpha1 so the 25th or so
(12:32:56 PM) sbeattie: seems okay, if perhaps a little ambitious.
(12:33:24 PM) jjohansen: well I was thinking I'd rather multiple smaller steps than another big one
(12:33:38 PM) jjohansen: unfortunately labeling is just a big step
(12:33:55 PM) jdstrand: I may have missed this-- will alpha1 contain the dbus patchset?
(12:34:05 PM) jjohansen: jdstrand: yes
(12:34:17 PM) sbeattie: jjohansen: yeah, that's okay. Just worried about using up time spinning releases
(12:35:15 PM) jjohansen: sbeattie: yeah that is a bit of an issue, but I think there are enough changes queued that it will be worth it
(12:35:28 PM) jjohansen: not sure what we will get in but ...
(12:35:37 PM) sbeattie: okay
(12:35:49 PM) jjohansen: we can reevaluate for alpha3
(12:36:16 PM) jjohansen: All right does anyone have anything else they would like to raise?
(12:37:14 PM) jdstrand: my aa-sandbox patches never got committed. I know we have a policy of committing after a week, but I didn't feel comfortable doing that in this case
(12:37:43 PM) sarnold: jdstrand: they were confined to aa-sandbox, right?
(12:38:16 PM) jdstrand: well, it adds apparmor/sandbox.py
(12:38:20 PM) jjohansen: oh shoot yes, how did that drop off my radar
(12:38:26 PM) jjohansen: jdstrand: please commit
(12:38:35 PM) sarnold: jdstrand: feels like it'd be worth committing :)
(12:39:03 PM) jjohansen: they are isolated, and won't break anything if there are issues, and its better that they are there than not
(12:39:19 PM) jdstrand: it works reasonably well. I've since done a few cleanups and there are a few todo's that I can look at at maybe later in the week
(12:39:28 PM) jdstrand: that said, I use it regularly
(12:39:46 PM) jdstrand: ok
(12:40:06 PM) sbeattie: yeah, please commit.
(12:40:08 PM) jdstrand: I'd also really like to have it in there so I can break out some common code between the python utils
(12:40:10 PM) jjohansen: jdstrand: right, I wouldn't even worry about the todos, for this week. We are talking alpha1 so there is time
(12:40:17 PM) jdstrand: (apparmor/common.py)
(12:40:36 PM) jdstrand: easyprof and sandbox would benefit, and I am rewriting aa-notify too
(12:40:43 PM) sarnold: yay
(12:40:46 PM) ***jdstrand adds to list
(12:41:37 PM) jdstrand: I'll get it in later this week
(12:41:52 PM) jdstrand: thanks
(12:42:01 PM) cboltz: jdstrand: what are your exact plans for aa-notify?
(12:42:35 PM) cboltz: IMHO having it split into one part running as root and another for displaying messages on the desktop would be nice
(12:42:49 PM) jdstrand: cboltz: right now, same functionality (like we did with aa-status), except I wanted to add '-f' for logfiles and it would support multiple logfiles
(12:42:57 PM) sarnold: cboltz: that'd be nice.
(12:43:07 PM) jdstrand: this is useful for dbus, since denials are in kern.log and syslog
(12:44:01 PM) jdstrand: well, it only runs with privs a very short time, but it could be priv separated
(12:44:06 PM) jdstrand: (properly)
(12:44:27 PM) sarnold: jdstrand: heh, I think my reading of that code shows that it gains and drops over and over and over again...
(12:44:33 PM) jdstrand: that would slow the rewrite down significantly
(12:44:44 PM) sarnold: so while it might not be priv'd for very long, it holds it indefinitely...
(12:44:52 PM) jdstrand: sarnold: it does if it has to reload a logfile, yes
(12:45:10 PM) jdstrand: sarnold: hence the 'properly'
(12:45:13 PM) jdstrand: :)
(12:45:29 PM) sarnold: :)
(12:46:07 PM) jdstrand: but if I am going to do some dbus service or something, that would take much more design and effort than I currently have time for
(12:46:35 PM) jdstrand: (and I think the other stuff we have targeted is much more important atm)
(12:46:51 PM) jdstrand: don't get me wrong, I'd like to see it properly privseparated
(12:47:19 PM) cboltz: then just rewrite it "as is", but keep my wish in mind so that implementing it later isn't too hard ;-)
(12:47:19 PM) jdstrand: I could write an apparmor profile for it...
(12:47:32 PM) cboltz: *lol*
(12:47:44 PM) jdstrand: cboltz: yes, I fully agree, that is something we should do
(12:47:59 PM) jdstrand: I think it should be on the Raodmap. I'll add it if it isn't already there
(12:48:07 PM) cboltz: "we don't trust our own tools"
(12:48:16 PM) jdstrand: I don't trust anything :P
(12:48:39 PM) cboltz: seems you are even more paranoid than I am...
(12:49:07 PM) jdstrand: well, I don't actually have a profile for it atm, so it is lip service atm ;)
(12:50:52 PM) jjohansen: alright, anything else?
(12:52:22 PM) cboltz: not before 2.8.1 is released ;-)
(12:52:49 PM) sbeattie: not from me
(12:52:59 PM) jdstrand: hmmm, the apparmor wiki errored out when I committed. something about varnish
(12:53:05 PM) jdstrand: jjohansen: nope
(12:53:20 PM) jjohansen: okay, so I propose the next meeting be held Feb 5 same time, same place
(12:53:27 PM) jjohansen: jdstrand: hrmm strange
(12:53:59 PM) jjohansen: jdstrand: I have been doing agenda updates to the wiki without problems during the meeting, so try again
(12:54:56 PM) jdstrand: worked that time
(12:54:59 PM) jdstrand: weird
(12:56:07 PM) jjohansen: alright thanks everyone, meeting adjourned
(12:56:14 PM) jdstrand: thanks jjohansen :)
(12:56:34 PM) sarnold: thanks jjohansen :)
(12:57:26 PM) mdeslaur: thanks!
(12:57:38 PM) sbeattie: thanks jjohansen 
```
