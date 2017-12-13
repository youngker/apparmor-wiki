```
(01:02:09 PM) jjohansen: kshitij8, kozdincer welcome
sbeattie sbeattie_ 
(01:02:17 PM) kozdincer: hi 
(01:02:28 PM) kshitij8: Hello.. 
(01:03:04 PM) jjohansen: sbeattie, jdstrand, tyhicks, sarnold, mdeslaur, cboltz: meeting time
(01:03:12 PM) mdeslaur: \o
(01:03:14 PM) jdstrand: hi!
(01:03:19 PM) tyhicks: hello
(01:03:22 PM) cboltz: hi
(01:03:51 PM) jjohansen: Alright lets get started
(01:04:00 PM) jjohansen: First up development status
(01:04:17 PM) jjohansen: I think we have enough progress to talk about rolling an alpha3 release
(01:04:47 PM) jjohansen: the kernel has been fairly stable and we actually have some of the 3.0 features available to play with
(01:05:32 PM) jjohansen: I was thinking of waiting until we fix the default profile bug, and maybe make the dbus changes we have been discussing
(01:05:53 PM) jjohansen: tyhicks: what is your take on waiting for alpha3 until the dbus changes happen?
(01:06:15 PM) tyhicks: jjohansen: by dbus changes, you mean the policy language changes?
(01:06:25 PM) jjohansen: the only downside I see to release alpha3 with the current dbus, is people see the older syntax
(01:06:42 PM) jjohansen: tyhicks: policy language and encoding
(01:07:03 PM) tyhicks: hmm
(01:07:13 PM) jjohansen: well I guess with encoding things could break on people if they don't upgrade both kernel and compiler at the same time
(01:07:20 PM) jjohansen: but we are taling alphas here
(01:07:22 PM) tyhicks: I suppose it would be ok if we made it clear that the language and encoding *will* change
(01:08:27 PM) jjohansen: anyone have objections to doing that?
(01:08:36 PM) sbeattie: not from me
(01:08:50 PM) cboltz: no objections
(01:09:13 PM) jjohansen: good enough, I will try to get an alpha3 rolled at the end of the week
(01:09:35 PM) jjohansen: One to 2.8.2
(01:09:45 PM) jjohansen: s/one/on/
(01:10:31 PM) jdstrand: well
(01:11:07 PM) jdstrand: I don't strongly object, but we are close to finalizing the dbus bits (I think), so I wonder why we roll it right before we get it fixed
(01:11:44 PM) cboltz: I begin to fetch patches from the 2.8 branch for the openSUSE rpm (for example the python3 stuff), so I'd like to have a 2.8.2 ;-)
(01:11:46 PM) jjohansen: well I am not opposed to holding off on the alpha, but I don't want to do it too long
(01:11:49 PM) jdstrand: seems like we'd have less churn if we waited a couple weeks
(01:12:15 PM) jjohansen: yes
(01:12:24 PM) jdstrand: I guess it depends on when we think the dbus syntax will be finalized. I thought we had some good discussions and just need to pick something
(01:12:35 PM) jjohansen: yes
(01:12:44 PM) jjohansen: and then implement and test
(01:13:06 PM) jdstrand: right, but aiui, that bit won't take terribly long
(01:13:07 PM) tyhicks: that's not the only thing left to finalize w/ dbus :/
(01:13:08 PM) jjohansen: it should will be done before the end of the month
(01:13:21 PM) tyhicks: we also need to revamp the userspace query api
(01:13:43 PM) tyhicks: which will drive changes into the current libapparmor patches
(01:13:48 PM) jdstrand: tyhicks: sure, but we are rolling something and saying 'test it' for people. seems since we are close with syntax, we get it then roll
(01:14:08 PM) jdstrand: but, if now is better, fine
(01:14:35 PM) tyhicks: good point - it would be better to get testing of the new syntax
(01:15:14 PM) tyhicks: either way, we'll have to a draw a line somewhere and cut an alpha release
(01:15:36 PM) jjohansen: alright, we will wait on the alpha until the syntax change is in, the query interface can wait until alpha4
(01:15:42 PM) jjohansen: sound reasonable
(01:15:45 PM) tyhicks: sounds good
(01:15:48 PM) sbeattie: okay
(01:15:52 PM) ***jdstrand nods
(01:16:09 PM) jjohansen: okay back to 2.8.2
(01:16:18 PM) jjohansen: So 2.8 is going to remain our current release for a while yet, and we have accumulated several fixes, it is looking like time to roll a 2.8.2
(01:16:18 PM) jjohansen: I was thinking targeting the end of the month?
(01:16:36 PM) jjohansen: cboltz: does that suit your needs?
(01:16:55 PM) cboltz: I don't have a release date in sight, so I'm flexible
(01:17:17 PM) cboltz: I'll "just" push 2.8.2 packages to the update channel
(01:18:03 PM) cboltz: end of the month sounds fine, but if it's some days later or earlier, it's also ok
sbeattie sbeattie_ 
(01:18:27 PM) sbeattie: is there anything else from trunk that needs to go into 2.8 that hasn't made it yet?
sbeattie sbeattie_ 
(01:18:49 PM) sbeattie: Is there anything ubuntu or suse are carrying that ought to go in?
(01:18:51 PM) jjohansen: sbeattie: hrmmm, I'm not sure. I think we will have to check
(01:19:01 PM) jjohansen: probably
sbeattie sbeattie_ 
(01:19:29 PM) sbeattie: okay. I can take actions to check trunk and ubuntu packaging for things for 2.8
(01:19:33 PM) jjohansen: again we will have to check
(01:19:44 PM) cboltz: sbeattie: most of the patches in the openSUSE rpm are already in bzr 
(01:20:08 PM) cboltz: (I tend to send them to the mailinglist when they are ready because I don't want to carry 100 patches around ;-)
(01:20:18 PM) jjohansen: cboltz: if there is anything not in bzr, that makes sense to upstream, can you forward it to the list
(01:20:27 PM) jjohansen: :)
(01:20:36 PM) cboltz: I know ;-)
(01:21:20 PM) jjohansen: okay, that gives us about 2 weeks. So if there is something you want in 2.8.2 get the patch in!
(01:21:29 PM) jjohansen: alright lets move on
(01:22:03 PM) jjohansen: We have applied to be part of GSoC through the opensuse project
(01:22:28 PM) jjohansen: we have two proposals to replace the utils genprof/logprof
(01:23:27 PM) jjohansen: and we have invited proposal authors Kshitij Gupta, and Kaan Özdinçer to join us today, so they can answer questions 
(01:23:41 PM) jjohansen: their proposals if you missed them are
(01:23:48 PM) jjohansen: Kshitij Gupta - http://www.google-melange.com/gsoc/proposal/review/google/gsoc2013/kshitij8/1
(01:23:57 PM) jjohansen: Kaan Özdinçer - http://www.google-melange.com/gsoc/proposal/review/google/gsoc2013/kozdincer/4001
(01:25:11 PM) jjohansen: in addition to the proposal we have asked them to profile an application to get experience with the current tools and policy
(01:25:11 PM) jjohansen: Kshitij - profiled inkscape
(01:25:11 PM) jjohansen: and
(01:25:11 PM) jjohansen: Kaan - profiled mtr
(01:25:24 PM) jjohansen: welcome Kshitij and Kaan
(01:25:40 PM) kozdincer: thanks sbeattie for review
(01:26:04 PM) sbeattie: you're welcome
(01:26:05 PM) kozdincer: hi
(01:26:16 PM) kshitij8: thanks jjohansen for the introduction
(01:26:46 PM) kshitij8: hello to everyone
(01:27:22 PM) jjohansen: so while people are perusing your proposals :) do you have any questions you would like to ask about the project?
(01:27:45 PM) jdstrand: kshitij8's seems incredibly ambitious, with the online repo
(01:28:40 PM) ***jdstrand doesn't have a question atm, just wanted to mention that
(01:28:47 PM) kshitij8: jjohansen I had some queries regarding the apparmor3 , is additional syntax going to be added
(01:29:44 PM) jjohansen: kshitij8: yes, though that doesn't necessarily have to affect your proposals as apparmor 3 will not roll out until after GSoC is done
(01:29:49 PM) kshitij8: the documentation at the moment has many to be done things, its not clear what has been incorporated and whats not, besides how much change does 3 include
(01:30:03 PM) jjohansen: the release time frame for 3.0 is september
(01:30:35 PM) jjohansen: so 3.0 will keep the same basic syntax
(01:30:42 PM) jjohansen: it will expand upon it
(01:30:58 PM) jjohansen: there will be new rule types and new permissions
(01:31:13 PM) jjohansen: an example would be the dbus rules we where discussing earlier
(01:31:34 PM) kshitij8: will there be backward compatibility?
(01:31:39 PM) jjohansen: the syntax is not entirely settle on atm
(01:32:07 PM) jjohansen: kshitij8: yes some form of backwards compatibility is planned
(01:32:12 PM) kshitij8: and has anybody here also worked on the YaST module for apparmor?
(01:32:15 PM) jjohansen: again we are talking extension
(01:32:33 PM) ***jjohansen sadly raises his hand
(01:32:46 PM) jdstrand: heheh
(01:32:48 PM) cboltz: usually the profiles are always backward compatible (as in: old profiles work with newer AppArmor version)
(01:32:54 PM) kshitij8: I saw much of the code used in the YaST configuration to be same
(01:33:18 PM) cboltz: the only exception is when a new set of rules was added - for example the network rules, an old profile for ping won't work anymore ;-)
(01:33:29 PM) kshitij8: is there any specific reason why it doesnt use the command-line tools we already have?
(01:34:27 PM) jjohansen: kshitij8: the YaST tool was a wrapper around the command line tool, with a dedicated interface to have input output to YaST instead of the console
(01:34:41 PM) jjohansen: basically it allowed the command line tool to have a gui
(01:34:46 PM) kshitij8: I understand, I was only wondering that it'd be better if the new tools were as compatible with 3 as possible.
(01:34:53 PM) jjohansen: yes
(01:35:22 PM) sbeattie: well, there was an abstraction layer IIRC because when the apparmor yast bits were written, yast wasn't open source, yet, IIRC.
(01:35:36 PM) jjohansen: and from a YaST perspective I am not sure it wouldn't be better to have the YaST module launch an external gui tool instead of cram the tool into YaST
(01:35:40 PM) sbeattie: (but my memory is fuzzy on the whole thing)
sbeattie sbeattie_ 
(01:35:54 PM) jjohansen: sbeattie: yep your right
(01:36:04 PM) jjohansen: though I think some of that got removed
(01:36:36 PM) kshitij8: so how much of it would be break by switching to python?
(01:37:01 PM) jjohansen: kshitij8: we expect to break the YaST tool
(01:37:13 PM) jjohansen: it will need to be updated separately
(01:37:41 PM) jjohansen: again it may be that the YaST component just launches the apparmor tool when a button is clicked
(01:37:52 PM) jjohansen: instead of integrating it into YaST
(01:37:55 PM) kshitij8: a new abstraction layer for the python based tools?
(01:38:17 PM) cboltz: kshitij8: just FYI: the YaST developers tend to move away from YCP and prefer more "common" languages like python or perl
(01:38:37 PM) jjohansen: kshitij8: having an interface to YaST is a valid direction for development
(01:38:47 PM) cboltz: that doesn't mean you _have to_ drop the YCP code, but you shouldn't invest too much time to learn the YCP syntax ;-)
(01:38:55 PM) jjohansen: it was one of the options offered after completing a base tool
(01:39:23 PM) kshitij8: i feel i would like to pursue that direction
(01:39:56 PM) kshitij8: because getting a separate ui may complicate things.
(01:41:04 PM) jjohansen: kshitij8: I think, given time lines that having the tool connect/integrate/whatever with YaST is probably a better choice than the online repo you proposed
(01:41:18 PM) ***jdstrand agrees
(01:41:22 PM) jjohansen: the online repo was ambitious
(01:41:22 PM) cboltz: ideally you create a "backend" and can then write relatively "small" frontends for it
(01:41:28 PM) jjohansen: ye
(01:41:31 PM) cboltz: YaST would be the first frontend
(01:41:34 PM) jjohansen: s/ye/yes/
(01:41:45 PM) cboltz: but it should be easy to create other frontends
(01:41:51 PM) kshitij8: i too agree.. the online repo will take much more effort, while this needs to be addressed first
(01:42:06 PM) kshitij8: because a broken yast side app will be bad
(01:43:19 PM) jjohansen: kshitij8: okay, can you still edit your proposal or is that locked down now?
(01:43:27 PM) kshitij8: getting separate interface modules for command-line and yast would be code and provide more flexible.
(01:43:51 PM) kshitij8: no its locked, i cant edit it now.
(01:44:09 PM) jjohansen: alright, thats okay
kees kensington kov kozdincer kshitij8 
(01:44:26 PM) cboltz: I can enable editing again if you want
(01:45:03 PM) kshitij8: yes please cboltz . I feel I need to make a few updates to the proposal
(01:45:04 PM) jjohansen: kshitij8, kozdincer: note that if we get a slot and you get choosen, I think it is acceptable to change the follow on direction
(01:45:48 PM) kozdincer: ok
(01:45:55 PM) kshitij8: yes
(01:45:56 PM) jjohansen: there is a an evaluation phase about midway, and you will have a better grasp of what is involved and where you may want to take things
(01:46:39 PM) jjohansen: alright does anyone have questions for kshitij8, or kozdincer
(01:46:40 PM) kshitij8: agreed
(01:47:12 PM) cboltz: kshitij8: I just enabled editing for your proposal
(01:47:38 PM) cboltz: kozdincer: do you also want to edit something in your proposal?
(01:48:11 PM) kozdincer: now or future?
(01:48:13 PM) kshitij8: Thanks. I will make necessary changes to the proposal as soon as possible.
(01:48:28 PM) kozdincer: i want to make some changes too.
(01:48:38 PM) cboltz: kozdincer: now would be best ;-)
(01:48:52 PM) kozdincer: :)
(01:48:56 PM) cboltz: kozdincer: editing is now also enabled for you
(01:49:03 PM) kozdincer: thanks
(01:51:11 PM) jjohansen: hrmmm, so we seem to be a quite bunch today, without any questions
(01:52:07 PM) jjohansen: kshitij8, kozdincer: do you have any more questions for us?
(01:52:07 PM) cboltz: kozdincer: you were quite silent compared to kshitij8 ;-) - do you have any questions or are you just happy with everything? ;-)
(01:52:33 PM) kozdincer: no question
(01:52:56 PM) cboltz: BTW: I'm working on enabling the python3 bindings in the libapparmor package for openSUSE
(01:53:03 PM) kshitij8: nothing at the moment.
(01:53:04 PM) cboltz: so that you can use libapparmor from python
(01:53:40 PM) cboltz: (for example, it contains a function that parses the audit.log and gives you a nice struct for each log entry)
(01:54:18 PM) jjohansen: alright kshitij8, kozdincer thankyou for coming, feel free to stick around I will try to get the meeting wrapped up quick
(01:54:22 PM) kshitij8: that'd be usefull
(01:55:01 PM) cboltz: you might want to check what else libapparmor offers ;-)
(01:55:03 PM) kozdincer: it was nice
(01:55:51 PM) kshitij8: yes, i havent yet seen that. seems it will save us much time and energy.
(01:56:02 PM) kshitij8: with the bindings ofcourse.
(01:57:20 PM) cboltz: kozdincer: just curious - do you have C or C++ knownledge?
(01:58:28 PM) kozdincer: not much experienced but while i was packaging for pardus. i made some patches for softwares which written with c/c++
(01:58:53 PM) cboltz: that's probably enough ;-)
(01:59:16 PM) cboltz: you probably won't need C or C++, but it might be helpful if you need to understand a detail in libapparmor
(01:59:37 PM) kozdincer: yes
(01:59:59 PM) kozdincer: absolutely
(02:00:41 PM) jjohansen: alright so we have had some discussion around dbus rules and the default profile
(02:01:14 PM) jjohansen: we need to settle on the dbus syntax, is everyone generally happy with the proposal for grouping
(02:01:28 PM) jdstrand: +10
(02:01:47 PM) jdstrand: I prefer peer=()
(02:01:56 PM) cboltz: jjohansen: can you give a short summary, please? I didn't have time to read the last ~50 mails yet :-/
(02:02:06 PM) jdstrand: I still like subj=(), but not strongly opinionated on it
(02:02:16 PM) jjohansen: cboltz: sure
(02:02:39 PM) jjohansen: Basically we want dbus and networking to have the same general format
(02:03:08 PM) jjohansen: the proposal is to have a syntax that groups the peer address info together to make it clear
(02:03:30 PM) jjohansen: dbus name=address.com aquire,
(02:03:47 PM) jjohansen: dbus peer(name=address.com) send,
(02:04:03 PM) jjohansen: err s/peer/peer=/
(02:04:37 PM) jjohansen: peer() was actually proposed, but I counter proposed the peer=()
(02:04:49 PM) jjohansen: or argued for it anyways
(02:04:55 PM) sbeattie: heh.
(02:05:26 PM) jjohansen: yeah as if the discussion was really that clean :)
(02:05:33 PM) jdstrand: hehe
(02:05:57 PM) jjohansen: anyways thats the basics of grouping
(02:06:12 PM) sbeattie: Note that that is a particularly confusing example, because "dbus name=address.com aquire," allows the process to grab the address.com name, while "dbus peer(name=address.com) send," grants the ability to send a message to a peer that has acquired the address.com name.
(02:06:30 PM) jjohansen: the other part is whether the subject address is implicit, or whether we require it to be grouped as well
(02:06:40 PM) sbeattie: You'd be unlikely to write those two rules together in the same profile.
(02:06:50 PM) jjohansen: yes
(02:07:00 PM) jjohansen: sorry
(02:07:26 PM) cboltz: "peer" (with or without the =) looks good and seems to be a good way to avoid the words "sender" and "receiver" ;-)
(02:07:41 PM) jjohansen: so
(02:07:41 PM) jjohansen: dbus name=address.com acquire,
(02:07:41 PM) jjohansen: vs.
(02:07:41 PM) jjohansen: dbus subj=(name=address.com) acquire,
(02:08:22 PM) cboltz: I'd say keep it simple ;-)
(02:08:54 PM) jjohansen: cboltz: and what is keeping it simple? :)
(02:08:57 PM) jdstrand: well, is simple clear or compact?
(02:09:02 PM) jdstrand: I vote clear :P
(02:09:19 PM) jjohansen: haha
(02:09:29 PM) jdstrand: but again, if other feel strongly against, I won't block on it. peer=() is the big win
(02:09:42 PM) cboltz: how would a rule look that allows sending from (local) example.com to peer address.com?
(02:09:44 PM) jjohansen: so one thing to note is profile rules are always written from the pov of the subject
(02:09:57 PM) sbeattie: What I kind of wanted to do was to take some of the test dbus profiles that we've written and rewrite the policy in the various proposed schemes.
(02:10:41 PM) jjohansen: that is a good idea
(02:10:55 PM) jdstrand: jjohansen: true, but in other rules there wasn't a concept of peer, so... /shrug
(02:11:18 PM) jjohansen: jdstrand: well not yet
(02:11:27 PM) sbeattie: (It hinders us in coming to a decision in this meeting, however :) )
(02:11:45 PM) jjohansen: thats fine as long as people follow up on the ml
(02:12:21 PM) jjohansen: I'll rework some of the test profiles to the proposed syntax and send it to the list
(02:12:24 PM) jdstrand: the point I'm making is that I'm slightly unconfortable about implying things in syntax, but !subj=() can be handled in documentation just as easily
(02:12:46 PM) jdstrand: we can always vote/decide on the list too
(02:13:00 PM) jjohansen: sure, but we already are implying subject in other rules
(02:13:29 PM) jjohansen: anyways, just vote on what you like when I post out the examples
(02:13:52 PM) jjohansen: and if you don't, I'll poke you in irc
(02:14:01 PM) jjohansen: alright on to the default profile
(02:14:13 PM) jdstrand: I'm pretty comfortable with being shot down, so I'll watch out for the emails
(02:14:18 PM) jjohansen: the proposal is to completely replace the unconfined profile with it
(02:14:48 PM) jjohansen: that is everything boots in the default profile in the unconfined state
(02:14:57 PM) jjohansen: and the default profile can be replaced
(02:15:19 PM) jjohansen: this does mean u/Ux mean transition to default not unconfined
(02:15:34 PM) jjohansen: but we have been trying to discourage their use anyways
(02:16:13 PM) kozdincer left the room (quit: Quit: Page closed).
(02:16:25 PM) jjohansen: note: that u/Ux still mean will result in unconfined unless the default profile has been replaced and the admin is trying to enforce a total system policy
(02:16:29 PM) kozdincer [~oftc-webi@78.172.225.103] entered the room.
(02:16:37 PM) sbeattie: what happens when the default policy is removed? Does that mean we fall back to an implicit unconfined policy?
(02:16:43 PM) cboltz: sounds good so far - but there are some devils in the details which we need to solve ;-)
sbeattie sbeattie_ 
(02:17:19 PM) jjohansen: sbeattie: no, we special case its removal. It means default reverts to its unconfined state
(02:18:04 PM) jjohansen: cboltz: there are a few but we can work them out
(02:18:17 PM) sbeattie: well, okay, same effect, different implementation, though I guess that means if you then reload a non-wideopen default policy it gets re-applied everywhere.
(02:18:37 PM) sbeattie: (which is good, not a complaint from me)
(02:18:41 PM) jjohansen: well, to everything in the default profile, yes
(02:18:55 PM) jdstrand: jjohansen: "the proposal is to completely replace the unconfined profile with it" - do you mean "the proposal is to support completely replacing the unconfined profile witht he default profile"?
(02:19:04 PM) jjohansen: and yes if you apparmor teardown, and then replace default everything is confined
(02:19:27 PM) jjohansen: jdstrand: yes
(02:19:27 PM) jdstrand: ie, defaults are same as always, but we support this other mode for people to use if they desire
(02:19:30 PM) jdstrand: ok
(02:19:31 PM) sbeattie: jjohansen: that's what I meant, yes, thanks for the clarifications.
(02:19:46 PM) jdstrand: mode wasn't the best word there...
(02:19:49 PM) jdstrand: anyway
(02:20:24 PM) jjohansen: right, I suppose I should note, that unconfined is now a mode. And can be set on any profile
(02:20:47 PM) jjohansen: the default profile will only have minor special casing around its removal
(02:20:59 PM) jjohansen: so that it is always available
(02:21:27 PM) jjohansen: okay that sounds like we are in agreement
(02:21:31 PM) jjohansen: next up
(02:21:38 PM) jjohansen: when to have the next meeting?
(02:21:44 PM) jdstrand: jjohansen: so, the ux/Ux bit I think needs some more discussion
(02:21:48 PM) sbeattie: will it be nameable, such that a 'Cx -> defaultname' will be the equivalent of a Ux rule?
(02:22:26 PM) sbeattie: (unless you happen to name it differently? Is that on the table as part of the proposal?)
(02:22:32 PM) jjohansen: okay, so I don't want to just disable ux/Ux because that will break older policy
(02:22:41 PM) ***jdstrand agrees
(02:22:45 PM) sbeattie: +1
(02:22:47 PM) jjohansen: nor do I want to have an unconfined and default profile
(02:22:58 PM) jjohansen: allowing ux/Ux to escape a system policy
(02:23:14 PM) jdstrand: hold on
(02:23:49 PM) jdstrand: by that statement you mean you don't want someone to use Ux while the default profile is in place to go truly unconfined?
(02:23:59 PM) jjohansen: yes
(02:24:14 PM) jdstrand: ok, I understand the statement and think I agree
(02:24:23 PM) cboltz: IMHO, Ux should switch to the default profile - that would be the best-matching way in a "confine everything" situation
(02:24:28 PM) jjohansen: that is default completely replaces the unconfined profile
(02:24:39 PM) jdstrand: right, that is what I was saying on the list
(02:24:43 PM) jjohansen: cboltz: exactly
(02:25:36 PM) jdstrand: I guess I'd like to understand exactly what Ux means in the context of the default profile because our transition is at the exec
(02:25:40 PM) jdstrand: ie,
(02:25:45 PM) jdstrand: profile default {
(02:25:46 PM) jjohansen: the current situation (both default, and unconfined) needs to be fixed. That is why I started that thread. And I think we are in agreement on the fix
(02:25:52 PM) jdstrand:   profile foo {
(02:26:01 PM) jjohansen: okay
(02:26:04 PM) jdstrand:     /bin/bar Ux,
(02:26:05 PM) jdstrand:   }
(02:26:07 PM) jdstrand: }
(02:26:42 PM) jjohansen: this means that /bin/bar will be transitioned to the "default" profile
(02:26:58 PM) jdstrand: right, so if I had:
(02:27:01 PM) jdstrand:  audit file,
(02:27:08 PM) jjohansen: and the "default" profile is unconfined, unless the policy author has defined policy for it
(02:27:10 PM) jdstrand: then /bin/bar's file accesses are logged
(02:27:29 PM) jdstrand: ('audit file,' in the default profile of course)
(02:27:32 PM) jjohansen: yes
sbeattie sbeattie_ 
(02:28:04 PM) jdstrand: ok, that makes sense
(02:28:06 PM) jjohansen: now to sbeattie's question
(02:28:15 PM) jdstrand: and will things like PUx still work as expected?
(02:28:38 PM) jjohansen: yes, well unless you define an attachment for the default profile
(02:28:46 PM) jjohansen: as discussed on the list
(02:29:05 PM) jdstrand: I read the list, let me reread
(02:29:34 PM) jjohansen: jdstrand: specifying an attachment could mean that a profile is always found
(02:30:18 PM) jjohansen: for PUx the behavior would actually be the same as the profile found would be the default profile, instead of falling back to the default profile
(02:30:28 PM) jjohansen: but the behavior for Pix is different
(02:30:54 PM) jjohansen: it is not recommended that you define an attachment for the default profile unless you really know what you are doing
sbeattie sbeattie_ 
(02:31:12 PM) jjohansen: back to sbeattie's question
(02:31:19 PM) cboltz: jjohansen: Pix sounds like one of those small devils I mentioned ;-)
(02:31:23 PM) jjohansen: yes the default profile's name is visible
(02:31:37 PM) sbeattie: Oh, for example if you have "/usr/bin/foo Pix," but "profile default /usr/bin/* {"
(02:31:40 PM) jjohansen: cboltz: not really it behaves like any other profile with a defined attachment
(02:31:54 PM) jdstrand: jjohansen: ok, yes, that gets back to the best practices we can recommend in the docs
(02:31:56 PM) sbeattie: it would trigger the P portion of the Pix rule.
(02:32:01 PM) ***jdstrand is satisfied
sbeattie sbeattie_ 
sbeattie sbeattie_ 
(02:32:06 PM) jjohansen: sbeattie: yes
(02:32:14 PM) sbeattie: (which makes sense)
(02:32:44 PM) jjohansen: it provides consistency and greater flexibility but allows you to shoot your self in the foot
(02:32:47 PM) ***sarnold returns from lunch (sorry, kinda got excited at the end of the vuds session..)
(02:33:09 PM) jjohansen: you can transition to the default profile using Px now
(02:33:16 PM) jjohansen:  /bin/foo px -> default,
(02:33:26 PM) jjohansen: where we could not do this with unconfined
(02:33:58 PM) jjohansen: you can't use cx, as default isn't a child profile
(02:33:58 PM) sbeattie: ah, sorry, yeah Px -> default, not Cx.
(02:34:21 PM) jjohansen: so one more question
(02:34:32 PM) jjohansen: should we provide the ability to name the default profile?
(02:34:42 PM) jjohansen: so at boot some one could do
(02:34:50 PM) jjohansen: apparmor.init_profile=unconfined
(02:35:00 PM) jjohansen: and the default profile will be named unconfined
(02:35:01 PM) sbeattie: (and should 'default' be the default name?)
(02:35:14 PM) jjohansen: well yes
(02:36:05 PM) cboltz: If it doesn't cause too much work, being able to select the profile at boot would be nice
(02:36:26 PM) jjohansen: its minimal
(02:36:27 PM) ***jdstrand was wondering how it would be beneficial to rename it
(02:36:29 PM) sbeattie: Would it? I'm trying to think of a use case for it?
(02:36:59 PM) cboltz: for example, it would be very helpful while playing with changes on the default profile if you could just boot with apparmor.init_profile=previous_working_default ;-)
(02:37:11 PM) jjohansen: backwards compat of somwone seeing unconfined in ps -Z
(02:37:25 PM) cboltz: (which would be more secure than booting into unconfined)
(02:37:27 PM) sarnold: jdstrand: I thought that "default" might give the wrong impression -- it confines init, and init cannot easily be moved to any other profile. but if someone wanted "every process that doesn't have a more specific profile" to execute in a "default" profile, having them _share_ that profile with init felt very very wrong
(02:37:53 PM) sbeattie: jjohansen: well, you could get than by naming the default profile 'unconfined' :)
(02:37:53 PM) sarnold: jdstrand: I thought that the name gives the wrong connotation if someone wants to provide a "profile everything_else /** { } " sort of profile
(02:38:13 PM) sbeattie: s/than/that/
(02:38:21 PM) jdstrand: I guess it depends on how you think about implementing system policy
(02:38:35 PM) jjohansen: yep
(02:38:59 PM) jjohansen: having the default profile named unconfined is all well and good when its actually unconfined
(02:39:02 PM) jdstrand: it isn't init's profile per se, it is a way to confine init, but I can see it could be split out in which cause 'default' might not be ideal
(02:39:08 PM) jjohansen: but is confusing when not unconfined
(02:39:09 PM) sarnold: "init" sounds nice to me, but it'll doubtless end up applied to _other_ processes as well, since getting policy loaded early enough for other processes to get their own domains might be difficult, too.
(02:39:20 PM) jdstrand: s/cause/case/
(02:39:57 PM) jjohansen: another possibility is to default to "unconfined" and allow a renaming replace to give it a different name for policy
(02:40:09 PM) jjohansen: note: renaming replace does not work yet
(02:40:17 PM) sbeattie: hehe
(02:40:18 PM) jdstrand: sarnold: funny, you and I have very different ideas on how we would profile an entire system :) I guess that is a case for allowing renaming it
(02:40:19 PM) sarnold: so I proposed allowing a new name to be passde on kernel command line to make it easy to (a) have 'default' mean something like 'the fall-back profile to use if no more specific profile exists'
(02:41:10 PM) sarnold: (b) as cboltz notes, allowing init's profile to be swapped to known-good previously working, is just a nice side effect :)
(02:42:47 PM) jjohansen: sarnold: it is not init's profile! I can't do that, it is not possible
(02:42:56 PM) sbeattie: (well, disabling apparmor at boot would be as effective at letting you recover from a too restrictive init/default policy)
(02:43:07 PM) jjohansen: it is the profile available from boot that init happens to be confined by
(02:43:20 PM) sarnold: jjohansen: 'init' was just a name chosen at random.. :)
(02:43:35 PM) sarnold: happenstance for my fingers to hit those keys! :)
(02:43:45 PM) jjohansen: sarnold: I just want to be very clear it is NOT inits profile, we can not separate it like that
(02:43:46 PM) cboltz: sbeattie: yes, but if you have to do it on a production machine, the previous known-working is more secure than unconfined ;-)
(02:44:24 PM) jjohansen: renaming the profile at boot is easy, so I am happy to provide that
(02:44:40 PM) jjohansen: just for those odd cases where someone may want it
(02:44:49 PM) jdstrand: if that is the case, it seems reasoble enough to support it
(02:44:53 PM) jjohansen: I am more concerned with what the default name is
(02:45:05 PM) jjohansen: and how we setup policy
(02:45:36 PM) jjohansen: if we choose "unconfined", then it looks good until the profile is replaced
(02:45:39 PM) sbeattie: jjohansen: hrm, here's a question, would it be possible to have init have a differently named policy (e.g. 'really_unconfined') and a default policy?
(02:45:48 PM) jdstrand: default seems ok. otoh, 'system' could also maybe work
(02:45:55 PM) sarnold: I can appreciate why one might not want it to be called 'unconfined', but having 'default' be, by default, an unconfined-mode profile, is almost counter-intuitive..
sbeattie sbeattie_ 
(02:46:22 PM) sarnold: 'system'. I kinda like that.
(02:46:35 PM) sarnold: very WinNT-ish. :)
(02:46:46 PM) jjohansen: sbeattie: theoretically? yes, but then I have to go special casing all the exec paths or build a basic profile into the kernel with different transitions
(02:46:51 PM) jdstrand: that was *not* what I was going for :P
(02:48:06 PM) jjohansen: basically it is not feasible/reasonable to have init be its own separate profile. If you want that stick some policy in your initrd, and load early policy
(02:48:21 PM) ***jdstrand nods
(02:48:26 PM) jjohansen: then init will have a different profile than its children, as you desire
(02:48:38 PM) sbeattie: jjohansen: how do I change the policy on init?
sbeattie sbeattie_ 
(02:48:47 PM) jjohansen: sbeattie?
(02:49:15 PM) sbeattie: jjohansen: or are you saying to load policy before init?
(02:49:20 PM) sarnold: do we envision a time when you can change the profile on a specific task after it's been running? will that interface come back in th efuture/
(02:49:39 PM) jjohansen: with the default profile. Its just a matter of packing a compiled policy into the initrd and having init load it, before real system startup happens
(02:49:41 PM) jdstrand: fyi, I don't have too much time left. if voting is to be had (and you want my vote), it'll need to happen soon or this'll need to go to the list
(02:49:48 PM) sbeattie: Imagine that I want a somewhat restrictive 'default' policy, but that it would be too restrictive for init/
(02:50:20 PM) jjohansen: sarnold: that interface may come back but it may not
(02:50:27 PM) sarnold: jjohansen: ack
sbeattie sbeattie_ 
(02:51:37 PM) jjohansen: sbeattie: so it depends on how your initrd is setup, you have your early init process, and it launches a few tasks etc. All of these are in the "system" profile
(02:52:09 PM) jjohansen: you load policy, as loose as you want, but that defines the transitions that you want to happen
(02:52:10 PM) sarnold: sbeattie: I envision that as being a usecase for apparmor.init_profile=early_boot -- provide the "profile early_boot { # something here? }" and then provide a "profile default /** { # restrictive policy here }"  -- once that latter policy gets loaded, it'll be applied to new processes...
(02:52:36 PM) jjohansen: then really init happens, and policy follows transitions defined in the profiles
(02:52:50 PM) jjohansen: you can then replace to tighter profiles as needed
(02:53:55 PM) jjohansen: the default "system" profile provides a means of providing a default confinement and even confining init without having to have policy loaded before init starts
(02:54:13 PM) jjohansen: otherwise what you have to do in your initrd
(02:54:32 PM) jjohansen: is load policy, and have init re-exec itself to get a profile attachment
(02:54:59 PM) jdstrand: for crystal clarity
(02:55:09 PM) jjohansen: I don't want to hard code special casing for init into the kernel
(02:55:16 PM) jjohansen: jdstrand: I know you have got to go
(02:55:24 PM) jjohansen: there will be no vote today
(02:55:44 PM) jjohansen: it is obviously this needs more discussion
(02:56:03 PM) jjohansen: I will write this up in a more detail explanation and send to the ml
(02:56:05 PM) sbeattie: jjohansen: I think we've pretty well hit consensus, actually/
(02:56:13 PM) jjohansen: really?
(02:56:29 PM) sbeattie: and are just hitting a couple of corner cases.
(02:56:41 PM) jjohansen: okay? so what is the name?
(02:56:50 PM) jjohansen: "system"?
(02:57:05 PM) sbeattie: heh, okay, we can vote on that on the list.
(02:57:08 PM) jdstrand: if we use apparmor.init=default (or whatever), the kernel boots, /sbin/init is started, then sometime later init can replace the 'default' profile and it could very well have /sbin/init in it. i'd have to reexec init to get that confinement, but if I used 'profile default /**' then init wouldn't have to be re-exec'd, correct?
(02:57:09 PM) cboltz: jjohansen: guess why I proposed to have the name as boot parameter ;-)
(02:57:22 PM) sbeattie: But the overall direction seems acceptable.
(02:58:22 PM) jjohansen: jdstrand: no, we don't attach to existing tasks. that attachment only applies on exec, so init would have to re-exec itself
(02:58:26 PM) sbeattie: jdstrand: no, reloading the default policy will get it applied to existing processed.
(02:58:37 PM) sbeattie: bah s/processed/processes/
(02:59:06 PM) jjohansen: jdstrand: gah maybe I misunderstood your Q
(02:59:29 PM) sbeattie: jjohansen: it may be that I didn't understand jdstrand's Q
(02:59:51 PM) jdstrand: ok, I thought that apparmor.init=default meant that there was some sort of redefined policy in place such that when we replaced the default profile, it worked like 'apparmor -r' on a process that was already confined
(03:00:02 PM) jdstrand: s/redefined/predefined/
(03:00:39 PM) jdstrand: ie, init already is attached to default
(03:00:46 PM) jdstrand: when the kernel starts it
(03:00:50 PM) sarnold: jdstrand: init would already be confined by 'default' -- reloading the 'default' policy would immediately cause new opens, execs, etc. to be mediated by policy, but /sbin/init ix, would only be needed to support 'telinit u' -- not for the attachment..
(03:01:00 PM) jdstrand: so loading the default profile is like a replace
(03:01:15 PM) jdstrand: ok, that is what I thought
(03:01:50 PM) jjohansen: jdstrand: to be clear lets say we start with a default profile named "system"
(03:01:50 PM) jjohansen: init is created and "system" is attached to it
(03:01:50 PM) jjohansen: init runs and all its children inherit "system"
(03:01:50 PM) jjohansen: at some later point I reload policy
(03:01:50 PM) jjohansen: case 1. I reload the "system" profile - init is now confined by the system policy
(03:01:50 PM) jjohansen: case 2. I reload policy with an "init" profile and a "system" profile. init is not confined by the "init" profile no matter what attachment is specified, unless init is re-execed
(03:02:04 PM) jdstrand: ok, so I do have to re-exec even if specifying /**, that's fine
(03:02:29 PM) jdstrand: jjohansen: yes, thanks
(03:02:32 PM) jjohansen: yes the attachment is only done during an exec
(03:02:43 PM) jjohansen: Now: I will throw a wrench in
(03:02:49 PM) ***sbeattie cries
(03:02:51 PM) jjohansen: there is the renaming replace
(03:03:03 PM) ***sarnold cries
(03:03:10 PM) jjohansen: it was designed to get rid of null-XXX learning profiles
(03:03:25 PM) jjohansen: it will be possible to rename/replace a profile at the same time
(03:03:37 PM) jjohansen: so "system" could be renamed to init
(03:03:44 PM) jjohansen: s/init/"init"
(03:04:11 PM) jjohansen: however this rename happens to all processes confined by "system" at the point of the renaming replace
(03:04:25 PM) jjohansen: so it does not necessarily apply to just init
(03:04:37 PM) jjohansen: nor does its attachment get run
(03:04:52 PM) jjohansen: as part of the replacement
(03:05:17 PM) jjohansen: this is because the "exec" path is only reliable at the time of exec
(03:06:46 PM) jdstrand: I don't think we should name the profile "init" at all costs :)
(03:06:52 PM) jjohansen: the only way to have init have a different profile than the rest of default is to have the init profile defined early enough, by either
(03:06:52 PM) jjohansen: 1. special casing in the kernel
(03:06:52 PM) jjohansen: 2. having a dummy stub profile compiled into the kernel
(03:06:52 PM) jjohansen: 3. loading stub (or full) policy early in the initrd
(03:06:59 PM) jjohansen: +1
(03:07:25 PM) sarnold: so, "system"? :)
(03:07:50 PM) jdstrand: jjohansen: right, understood
(03:08:16 PM) jdstrand: it actually is rather difficult, because, upstart for one can re-exec on upgrades
(03:08:28 PM) jjohansen: yes
(03:08:48 PM) jdstrand: so trying to account for that in a robust/reliable/predicatable matter is tricky
(03:08:53 PM) jjohansen: why how come my upstart just broke?
(03:08:54 PM) jdstrand: anyhoo, that isn't this conversation
(03:08:59 PM) jdstrand: I like "system"
(03:09:06 PM) jdstrand: jjohansen: hehe, yes :)
(03:09:59 PM) jdstrand: heh, 'predicatable'
(03:10:16 PM) jdstrand: jjohansen: maybe take to the list?
(03:10:32 PM) jjohansen: jdstrand: yes I will write it up and take it to the list
(03:10:43 PM) jjohansen: and here I was hoping this would take 3 min
(03:10:56 PM) jjohansen: alright do we need a meeting next month?
(03:11:04 PM) jdstrand: yeah, the idea is way cool and I think everyone here recognizes that
(03:11:17 PM) jdstrand: the problem is wrapping one's head around all of the different corner cases
(03:11:32 PM) jjohansen: if so, our schedule has it on the 11th
(03:12:07 PM) jdstrand: that works for me
(03:12:13 PM) sbeattie: yes, we should have a meeting next month
(03:12:42 PM) jjohansen: alright, next meeting june 11th, @20:00 UTC here in #apparmor
(03:12:46 PM) jjohansen: thanks everyone for coming
(03:12:58 PM) sbeattie: hopefully, we'll have alpha 3.0 by then and 2.8.2 released.
(03:13:01 PM) tyhicks: the 11th is good for me
(03:13:07 PM) jjohansen: meeting adjourned
```
