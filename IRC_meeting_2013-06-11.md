```
(01:02:43 PM) jjohansen: jdstrand, sbeattie, mdeslaur, sarnold, tyhicks, darix, kshitij8, cboltz: meeting time
(01:02:52 PM) tyhicks: hello
(01:02:53 PM) sbeattie: jdstrand: but we set you out to get cheetos before we all voted on it!
(01:03:01 PM) sbeattie: heya
(01:03:06 PM) kshitij8: hello 
(01:03:16 PM) cboltz: hello
(01:03:19 PM) mdeslaur: \o
(01:03:28 PM) darix: jdstrand: where would be the fun when you had the chance to agree first. then nobody would have it assigned again:p
(01:05:22 PM) jdstrand: hehe
(01:05:34 PM) jjohansen: alright, so our agenda (if you could call it that is at http://wiki.apparmor.net/index.php/MeetingAgenda)
(01:06:19 PM) jjohansen: So welcome back kshitij8, who is our GSoC student
(01:06:39 PM) sbeattie: woo, welcome kshitij8!
(01:06:54 PM) kshitij8: well thanks jjohansen for having me here
(01:07:51 PM) jjohansen: kshitij8: I realize its really early into the project can, you give a quick overview of the last week or so
(01:08:34 PM) kshitij8: with regards to the status of GSoC project, we have wiki page for the same at wiki.apparmor.net/index.php/Profile_Management_Tools
(01:09:47 PM) kshitij8: yes, since this is the community bonding period I havent yet gotten deeply into the coding and have got the basic set-up in place
(01:10:39 PM) kshitij8: we now have a launchpad repo for dedidacted for the project at https://code.launchpad.net/apparmor-profile-tools
(01:11:00 PM) kshitij8: at the moment nothing much in there though
(01:11:13 PM) sbeattie: cool
(01:11:55 PM) kshitij8: the coding period starts next week then I can expect some reviews from everyone I hope
(01:12:12 PM) jjohansen: kshitij8: so what do you plan on poking at this week?
(01:12:42 PM) kshitij8: besides these minor formalities we have had some active discussion on the mailing list about certain parts of the project and hopefully thats going to continue
(01:13:28 PM) kshitij8: this week im gonna dive into the coding part and then take on the bigfish next week
(01:13:47 PM) kshitij8: which will be the real challenge in the project..
(01:14:40 PM) kshitij8: i suppose thats all on my end for now 
(01:15:25 PM) jjohansen: kshitij8: okay, thanks. Please just ping us if you hit anything
(01:15:50 PM) cboltz: kshitij8: about the configparser module - did I understand you right (in your mail) that writing the configfile will drop all comments?
(01:15:51 PM) kshitij8: yes i'll be sure to do that. :-)
(01:16:04 PM) jjohansen: and yes active discussions should continue on the ml
(01:16:23 PM) kshitij8: cboltz yes it does
(01:16:30 PM) cboltz: nice :-(
(01:17:04 PM) cboltz: I'd recommend that you open a bugreport against configparser about it
(01:17:04 PM) kshitij8: I could look into a fix for keeping the comments
(01:17:19 PM) darix: +1 on keeping comments
(01:17:24 PM) cboltz: if you can include a patch, that's even better of course ;-)
(01:17:24 PM) jjohansen: +1
(01:17:47 PM) cboltz: but in general, one of the advantages of using existing code is: there is someone you can complain to ;-)
(01:18:02 PM) kshitij8: okay i'll see what I can do.. otherwise we can manually preserve the comments..
(01:18:27 PM) kshitij8: the problem however is managing comments with the edits I suppose
(01:19:30 PM) cboltz: let the code remember something like
(01:19:32 PM) jjohansen: well yes, that is a problem, but I'd rather just preserve comments that are out of date than drop comments all altogether
(01:19:43 PM) cboltz: "this comment was above the foo= setting in [bar] section"
(01:20:22 PM) kshitij8 left the room (quit: Remote host closed the connection).
(01:21:55 PM) jjohansen: well its seems we have lost kshitij8 so lets move on for now
(01:22:24 PM) jjohansen: The 2.8.2 release - it should happen soon
(01:22:40 PM) jjohansen: does anybody have patches they would like to get in?
(01:22:54 PM) sbeattie: bah, sorry about the delay on 2.8.2
(01:23:01 PM) jjohansen: I have an alias fix patch I need to finish debugging that I would like to hit
(01:23:11 PM) kshitij8 [~kshitij8@117.197.19.164] entered the room.
(01:23:19 PM) sbeattie: I was just starting to review trunk patches to see if there was anything to nominate.
(01:23:20 PM) cboltz: probably kshitij8's patch for logprof (avoid duplicate globbing proposals)
(01:23:33 PM) cboltz: welcome back, kshitij8 
(01:23:35 PM) kshitij8: sorry for the interruption im facing some connectivity issues today. did I miss something?
(01:23:48 PM) sarnold: cboltz: I don't recall seeing that patch, got a reference handy?
(01:24:04 PM) sbeattie: ... but I got sidetracked looking at cboltz's ruby installpath patch.
(01:24:13 PM) cboltz: kshitij8: we just talked about the 2.8.2 release, you missed maybe 5 lines
(01:24:31 PM) tyhicks: I don't have anything for 2.8.2
(01:24:35 PM) kshitij8: okay.. anything im expected to respond to?
(01:25:19 PM) cboltz: sarnold: it's in lp / bugs, let me search for the direct link
(01:25:40 PM) kshitij8: https://bugs.launchpad.net/apparmor/+bug/1180230
(01:26:30 PM) jjohansen: kshitij8: no nothing, important for you while you where away
(01:26:53 PM) csteipp_ [~csteipp@216.38.130.165] entered the room.
sbeattie sbeattie_ 
(01:27:34 PM) jjohansen: alright, so what should we shout for as a release date for 2.8.2? End of Month, Start of July?
(01:28:01 PM) sbeattie: jjohansen: are you going to do the release or am I?
(01:28:11 PM) jjohansen: We have a few patches to pull in, my patch that I need to finish debugging, anything else?
sbeattie sbeattie_ 
(01:28:14 PM) kshitij_ [~kshitij8@117.197.19.94] entered the room.
(01:28:33 PM) cboltz: given our delays, write Jun 20 on your calendar - that should result in something like end of month ;-)
(01:28:36 PM) jjohansen: sbeattie: well that depends on when we release, doesn't it? :)
(01:28:36 PM) sbeattie: heh
sbeattie sbeattie_ 
(01:29:02 PM) sbeattie: yes, that's why I'm asking, I'm on holiday the next two weeks
(01:29:19 PM) jjohansen: sbeattie: I am fine doing it if you want. Or do you think we can do it by eow?
(01:30:07 PM) sbeattie: I dunno, I have a bit of other stuff on my plate that I need to complete by eow
(01:30:44 PM) jjohansen: alright that is a NO
(01:31:43 PM) jjohansen: I will do the release and we will shoot for no later than July 1 as the release date, but Ideally we can get it out by June 21
(01:31:53 PM) sbeattie: okay.
(01:31:56 PM) jjohansen: so if you have a patch please get it in
sbeattie sbeattie_ 
(01:32:08 PM) sbeattie: (sorry about that, I should have pushed the release earlier)
(01:32:34 PM) onuonga [~oftc-webi@41.212.123.247] entered the room.
(01:32:36 PM) jjohansen: sbeattie: will you have time to do the trunk review this week, or do I need to pencil it in for next week
sbeattie sbeattie_ 
(01:32:48 PM) sbeattie: jjohansen: I'll take care of it this week
(01:32:55 PM) jjohansen: sbeattie: np, just like I should have gotten my patch out weeks ago
sbeattie sbeattie_ 
(01:33:02 PM) jjohansen: sbeattie: okay thanks
(01:33:10 PM) onuonga: sorry i had problems with my power supply company I am back
(01:33:19 PM) kshitij8_ [~oftc-webi@117.197.19.121] entered the room.
kshitij_ kshitij8 kshitij8_ 
kshitij8 kshitij8_ 
(01:33:31 PM) jjohansen: cboltz: are you going to pickup kshitij8's patch or should I
(01:33:44 PM) cboltz: feel free to do it ;-)
(01:33:48 PM) jjohansen: onuonga: welcome
(01:33:52 PM) jjohansen: cboltz: okay
(01:33:54 PM) kshitij8 left the room (quit: Ping timeout: 480 seconds).
(01:34:09 PM) jjohansen: Alright moving on, 3.0 dev status
(01:34:09 PM) ***sbeattie shakes a fist at lp for never notifying him about bug 1180230
(01:34:10 PM) kshitij8_: I'd recommend using the later of the two patches
kshitij_ kshitij8_ 
(01:34:29 PM) jjohansen: kshitij8_: okya, thanks
(01:34:56 PM) kshitij__ [~kshitij8@117.197.19.121] entered the room.
(01:35:12 PM) jjohansen: so we still haven't result the dbus/ipc syntax issues that we delayed the alpha for and I don't see them being fixed until at least the eom
(01:35:42 PM) jjohansen: we have also seen significant changes to the default profile design, which again will not get done until at least eom
(01:36:07 PM) jjohansen: do we want to release the current state as an alpha, or are we going to push it back again?
(01:36:34 PM) jdstrand: -1 for pushing current state
(01:36:55 PM) jdstrand: I feel we are quite close on the dbus syntax
(01:37:12 PM) kshitij_ left the room (quit: Ping timeout: 480 seconds).
(01:37:32 PM) jdstrand: so it feels like busy work to push an alpha now
(01:37:53 PM) onuonga left the room (quit: Quit: Page closed).
sbeattie sbeattie_ 
(01:38:23 PM) jjohansen: sbeattie, tyhicks: ?
(01:38:40 PM) tyhicks: I agree w/ jdstrand - there's not enough value from pushing an alpha at this point
(01:38:45 PM) sbeattie: waiting is fine w/me
(01:38:55 PM) jjohansen: I will agree it is extra work, and anyone playing with it will have to rework things
(01:39:23 PM) jjohansen: alright. We will push back the alpha again, until after we get the syntax fixed
(01:39:33 PM) jdstrand: if someone is really keen on playing with it, the code is in the ppa
(01:39:34 PM) jjohansen: lets try real hard to get it fixed soon
(01:39:49 PM) jdstrand: +100 :)
(01:39:53 PM) jjohansen: jdstrand: true enough
(01:40:00 PM) sbeattie: jjohansen: is there anything more you wanted from me (or anyone else) on the default profile stuff?
sbeattie sbeattie_ 
(01:40:17 PM) cboltz: what about a date limit - if we can't decide on the syntax until next week, then let's do the alpha release with the "old" syntax ;-)
(01:40:37 PM) jjohansen: sbeattie: not yet. I need to do some work on it and figure out possbile ways to deal with the changes to the ref counting etc
sbeattie sbeattie_ 
(01:41:16 PM) jjohansen: sbeattie: I'll try to get a mail out today/tomorrow so you have time to respond to it before running away
(01:41:49 PM) sbeattie: okay, cool.
(01:41:54 PM) jjohansen: cboltz: even if we decide the syntax we have to rework the code
(01:42:35 PM) jjohansen: I'm fine with a date limit but its got to be more than a week, and then the argument that we are close and its just extra work is even stronger
(01:42:49 PM) jjohansen: but I do agree we can't keep pushing off the alpha forever
(01:43:34 PM) jdstrand: I don't foresee a real problem
(01:43:51 PM) cboltz: I know that changing the syntax needs code changes, and the week was my way to avoid that you say "years" ;-)
(01:44:02 PM) tyhicks: I think we're trying to solve a problem that we don't really have
(01:44:05 PM) jdstrand: we are getting real close now and the implementation changes won't be horrendous, and it is prioritized very high
(01:44:24 PM) jdstrand: tyhicks: agreed
(01:46:01 PM) jjohansen: alright, so or deadline is next, meeting (1 month) if we don't have it done by then I promise to release what ever we have as an alpha
(01:46:57 PM) tyhicks: that's fine with me - I fully expect it to be ready by then
(01:47:29 PM) ***cboltz agrees
(01:47:35 PM) ***jjohansen too
(01:47:38 PM) sbeattie: +1
(01:47:51 PM) jdstrand: +1
(01:48:02 PM) jjohansen: carried, shall we move on?
(01:48:22 PM) sbeattie: please
(01:48:24 PM) jjohansen: Next meeting second tuesday of July at 20:00 UTC
(01:48:38 PM) jjohansen: that would be the 9th
(01:48:56 PM) jjohansen: any objections?
(01:49:16 PM) sarnold: wfm
(01:50:12 PM) sbeattie: no objection here
(01:50:18 PM) jjohansen: alright I'll consider that carried
(01:50:18 PM) jjohansen: Moving on, anyone else have anything they would like to bring up?
(01:50:18 PM) jjohansen: darix: ?
(01:50:27 PM) cboltz: we'll also have weekly meetings for GSoC every tuesday at 19:00 UTC
(01:50:35 PM) cboltz: (if this time is ok for everybody)
(01:51:09 PM) kshitij8_: cblotz on freenode or oftc?
(01:51:22 PM) cboltz: on oftc, in this channel
(01:51:27 PM) darix: jjohansen: naw i am fine. thanks
(01:52:11 PM) cboltz: (those meetings will probably mostly cover technical topics)
(01:52:36 PM) cboltz: of course, if you have questions you can always ask here, not only in the meetings
(01:52:38 PM) kshitij8_: cboltz I'll update the same on the wiki page. :-)
(01:52:39 PM) jjohansen: darix: ah and here I thought you would take the opportunity to formally assign jdstrand his wi :)
(01:52:50 PM) ***jdstrand has not agreed to that
(01:52:54 PM) jdstrand: :P
(01:52:54 PM) jjohansen: hehe
(01:53:35 PM) jjohansen: for the record, darix has asked for changehat support for php fpm
(01:53:54 PM) jjohansen: and partial profile redefinition 
(01:54:07 PM) jjohansen: alright, I don't have anything more
(01:54:14 PM) jjohansen: thanks for coming
(01:54:21 PM) jjohansen: meeting adjourned
(01:54:21 PM) darix: partial redefinition is needed for supporting things like
(01:54:29 PM) darix: /some/path r(1),
(01:54:29 PM) jjohansen: oops :)
(01:54:58 PM) darix: so once the counter reaches 0 you would remove the "allow read from the the profile"
(01:55:09 PM) jdstrand: interesting
(01:55:18 PM) ***jdstrand is not signing up for that
(01:55:47 PM) cboltz: jdstrand: you can't count backwards? ;-)
(01:55:50 PM) sbeattie: heh, I proposed something like that many, many moons ago.
(01:55:51 PM) darix: jdstrand: but but it would solve so many problems where you would need changehat support now!
(01:56:04 PM) sarnold: hehe, yeah, there's exactly one person who can code that, and it's a pity for darix that jjohansen can't be bribed with beer :)
(01:56:04 PM) jjohansen: jdstrand: oh come now, I know how much you want to get into the kernel code
(01:56:15 PM) jdstrand: cboltz: if it is so easy, I humbly request you submit said patch :)
(01:56:36 PM) darix: so cboltz will submit the patch to count backwards
(01:56:47 PM) darix: and jdstrand strand will implement the handle that rewrites the profile part
(01:56:56 PM) jdstrand: haha
(01:57:05 PM) cboltz: jdstrand: no, I won't break the kernel ;-)
(01:57:13 PM) jdstrand: let me just pencil that in to my todo list
(01:57:23 PM) darix: in theory you could do lots of that in the parser.
(01:57:27 PM) jdstrand: huh, the pencil break and the paper teared
(01:57:28 PM) jjohansen: actually its a nightmare, as we separate access (open) from revocation
(01:57:29 PM) jdstrand: weird
(01:57:44 PM) jdstrand: s/break/broke/
(01:57:55 PM) darix: you create a deny rule for the 0 case
(01:58:09 PM) sarnold: denies don't make it into the kernel..
(01:58:11 PM) darix: and then swap that in for the rule while the counter is above 0
(01:58:28 PM) jdstrand: sarnold: really, then how does audit deny work?
(01:58:41 PM) darix: sarnold: if I have a deny rule what gets passed into the kernel?
(01:58:43 PM) sarnold: jdstrand: by just being a regular 'not in profile' rule :) hehe
(01:58:50 PM) sarnold: darix: beats me. :(
(01:59:00 PM) jdstrand: sarnold: sorry, how does it not get logged?
(01:59:02 PM) darix: sarnold: you know i dont beat people usually.;)
(01:59:08 PM) jjohansen: jdstrand: explicit denies aren't currently carried into the kernel. The lack of the allow permission carries the denied information
(01:59:26 PM) jdstrand: right, but something is carrying to log or not, no?
(01:59:32 PM) jjohansen: yes
(01:59:44 PM) cboltz: and how does audit know that it shouldn't log things that are deny'd?
(01:59:45 PM) jjohansen: currently we carry, allow, audit, quiet
(02:00:12 PM) jjohansen: quiet is don't log a denial
(02:00:22 PM) cboltz: ah, ok
(02:00:29 PM) jjohansen: the expanded permission set carries a lot more information
(02:01:09 PM) jdstrand: let me back up. darix suggesting using a deny rule. a deny rule says to not log. and audit deny rule seems to tbe the default that would need anything to express it (the lack of the rule)
(02:01:30 PM) jjohansen: allow, subtree, audit, deny, quiet, kill, prompt, complain, stop, cond
(02:02:20 PM) jdstrand: well, I don't actually need to know this atm (/me doesn't want to waste people's time)
(02:02:21 PM) jjohansen: my suggestion was to wait for variable support and use a variable
(02:02:36 PM) jjohansen: so that the allow was conditional on a counter variable
(02:02:58 PM) jdstrand: well, darix is in luck then. variable support is already planned :)
``` 
