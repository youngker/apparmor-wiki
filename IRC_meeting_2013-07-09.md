```
(01:00:58 PM) jjohansen: sbeattie, sarnold, jdstrand, tyhicks, mdeslaur, cboltz, darix, kees: meeting time
(01:01:08 PM) mdeslaur: \o
(01:01:12 PM) jdstrand: hi!
(01:01:12 PM) cboltz: I'm (still) here ;-)
(01:01:12 PM) jjohansen: kshitij8: ^
(01:01:26 PM) kshitij8: \o
(01:02:03 PM) tyhicks: hello
(01:02:18 PM) darix: and there goes the idea of finally calling it a day :p
(01:02:19 PM) ***sbeattie waves
(01:03:13 PM) jjohansen: alright shall we start? Meeting agenda such as it is http://wiki.apparmor.net/index.php/MeetingAgenda#IRC_Meeting.2C_20:00_UTC.2C_July_09_2013
(01:03:36 PM) jjohansen: First up kshitij8 do you want to give your GSoC status report
(01:04:22 PM) kshitij8: yes, first up a major roadblock I have atm is that there are no yast bindings for python3.
(01:04:45 PM) kshitij8: I tried to contact yast-devs on ml, am yet to hear back from them.
(01:05:31 PM) cboltz: I had a short look into this, and got at least configure running
(01:05:38 PM) kshitij8: rest, going by current pace I'll have logprof tool ready by 27th and then testing and brushup
(01:05:50 PM) kshitij8: so seemingly Im on schedule.
(01:05:52 PM) jjohansen: yeah, not good. However the yast component was a secondary goal, if needed we could switch that to another goal (yes yeast would be nice to have)
(01:06:07 PM) darix: kshitij8: i doubt until suse changes the default python3, this will get much priority.
(01:06:09 PM) jjohansen: kshitij8: that is great to here
(01:06:10 PM) kshitij8: yast bindings exist for python2
(01:06:23 PM) jjohansen: s/here/hear
(01:06:25 PM) darix: you could try to propose that for factory maybe. ;)
(01:06:50 PM) jjohansen: kshitij8: are you still trying to make the new logprof bi-lingual?
(01:07:17 PM) jjohansen: if so it could use python2 on suse until python3 bindings are available
(01:07:21 PM) kshitij8: its if bilingual as of now, except for the configparser thing which i'll resolve in a day or two
(01:07:46 PM) cboltz: FYI: OBS home:cboltz:branches:YaST:Head/yast2-python-bindings builds with py3, has the updated configure and now fails with another error ;-)
(01:07:58 PM) jjohansen: okay, so lets keep that in mind as a potential option
(01:08:28 PM) jjohansen: cboltz: your full of good news
(01:08:32 PM) kshitij8: yes thats a possibility I had in mind too. getting to python2 for yast
(01:08:55 PM) cboltz: jjohansen: the not-so-good news is that it now fails in *.cc files which I'm not able to fix
(01:08:58 PM) kshitij8: I wonder how hard would a port would be for py3
(01:09:17 PM) darix: cboltz: yast bindings are not swig?
(01:09:18 PM) kshitij8: cboltz you did port libapparmor to python3?
(01:09:45 PM) cboltz: darix: doesn't look so
(01:10:14 PM) cboltz: kshitij8: "port" is a too big word - basically I just had to change the BuildRequires to py3
(01:10:28 PM) jjohansen: cboltz: thats about what I expected
(01:11:07 PM) jjohansen: cboltz: I think as long as kshitij8 can keep the new logprof bi-lingual py2 is probably the way to go
(01:11:09 PM) kshitij8: maybe somebody from yast-devs can fix that if its not that hard.
(01:12:00 PM) jjohansen: yes, lets let the yast devs deal with the bindings, if they are there great, if not we use a fall back either, py2 or a different secondary goal
(01:12:29 PM) kshitij8: sure, yast via py2 is great.
(01:12:34 PM) jjohansen: kshitij8: do you have anything else you would like to discuss wrt GSoC?
(01:12:42 PM) jjohansen: cboltz: same Q?
(01:12:51 PM) kshitij8: none that I can think of atm.
(01:13:01 PM) cboltz: there are various places in the code that hardcode utf-8
(01:13:10 PM) cboltz: does this make sense, or should we default to the system charset?
(01:13:17 PM) cboltz: (see my review mails for details)
(01:13:42 PM) darix: cboltz: utf-8 for what?
(01:13:57 PM) kshitij8: for reading files, writing to files
(01:14:05 PM) kshitij8: and also output from subprocesses
(01:14:52 PM) cboltz: IIRC also for output printed to the user (correct?)
(01:15:18 PM) darix: as long as you define that all those files need to be utf-8 encoded ... you should be fine
(01:15:28 PM) kshitij8: i think, its the default in python3.
(01:15:29 PM) darix: and it would probably a sane approach
(01:15:54 PM) kshitij8: having profiles and conf in utf-8 seems fair to me
(01:16:25 PM) ***cboltz wonders if he's the only one not using utf-8 yet
(01:16:35 PM) darix: cboltz: yes
(01:16:38 PM) kshitij8: what encoding do you use cboltz?
(01:16:56 PM) cboltz: iso-8859-15 aka de_DE@euro
(01:17:36 PM) cboltz: (but I can "decode" typical utf-8 double-bytes while reading on the screen, so don't worry about me ;-)
(01:17:37 PM) kshitij8: just wondering, if encoding has anything to do with filename too?
(01:18:09 PM) cboltz: yes, but I'd hope that the filenames we use are plain ascii ;-)
(01:18:15 PM) darix: kshitij8: yes
(01:18:43 PM) darix: cboltz: wait for those people outside the US/europe
(01:18:47 PM) kshitij8: then we can have a problem caused due to program/application names
(01:19:19 PM) kshitij8: coz prrofile names have them in their names too..
(01:19:25 PM) cboltz: which opens another question - which chars should we allow/replace for the profile filenames (when creating a new profile)?
(01:19:30 PM) darix: jjohansen: how does the kernel handle different encodings in filenames?
(01:19:50 PM) jdstrand: I don't think we can use utf-8 for profile names
(01:19:56 PM) jjohansen: filenames are all over the map, some user use utf8, others extended encodings like cboltz you really can't rely on ascii
(01:20:06 PM) kshitij8: on lines of what cboltz said, the issue for custom profile names?
(01:20:45 PM) jjohansen: darix: it doesn't. It uses a byte stream and only mandates / and \0.  So drivers do conversions (like FAT) and utf8 is generally assumed
(01:21:24 PM) jjohansen: darix: there is a reason we have tried to stay away from two much in pattern characters in the language
(01:21:55 PM) kshitij8: btw I haven't done anything explicitly that mandates utf-8 for filenames, and internal code supports unicode.
(01:22:19 PM) kshitij8: so I think filenames should be handled properly.
(01:22:46 PM) jjohansen: kshitij8: nope
(01:23:02 PM) jjohansen: kshitij8: files really need to be byte streams
(01:23:11 PM) jjohansen: err file names
(01:23:26 PM) jjohansen: there are several encodings that are not valid utf8
(01:23:33 PM) jdstrand: jjohansen: I can't have something like 'profile θ {}' though, right?
(01:23:52 PM) darix: kshitij8: i have a profile with "/srv/ä r," with the file saved as utf-8. if cboltz loads the profile it wouldnt match /srv/ä encoded in iso-8859-15. 
(01:24:00 PM) darix: encodings are fun :p
(01:24:24 PM) jjohansen: jdstrand: well uh sort of, the first character is limited, but for example /fooθ is entirely possible
(01:24:48 PM) kshitij8: internal python workings should resolve that with native encodings I guess. because people do use python for all different file names? and encodings?
(01:25:01 PM) jjohansen: yea
(01:25:09 PM) jdstrand: fwiw, I've limited profile_name in easyprof to be *way* less than that
(01:25:27 PM) darix: kov: in python 2 you needed u'foo' for unicode/utf-8 string literals didnt you?
(01:25:41 PM) kshitij8: yes darix
(01:25:43 PM) darix: err sorry kshitij8 
(01:25:49 PM) jjohansen: jdstrand: well profile names that don't specify an attachment (don't begin with /) are much more limited
(01:26:02 PM) jdstrand: warning-- trying to stay bilingual with these types of strings can be rough
(01:26:51 PM) jdstrand: jjohansen: right, I didn't limit the attachment, jut the profile name
(01:27:47 PM) jjohansen: alright, kshitij8 can you add this to your things to look into more
(01:28:23 PM) jjohansen: darix: did you want to stick around for the whole meeting or should we jump to the piece your interested in now
(01:28:24 PM) ***kshitij8 writes encoding is fun
(01:28:41 PM) darix: how long is the whole meeting?
(01:29:06 PM) jjohansen: not sure, it could be short it could drag on
(01:29:17 PM) sarnold: Is "ewigkeit" the right word? :)
(01:29:19 PM) darix: in any case i wouldnt mind doing it now and still stick around to the end
(01:30:25 PM) kshitij8: jjohansen, are profiles with custom names allowed?
(01:30:27 PM) jjohansen: alright, so darix wanted to discuss the ipc syntax email I sent out, specifically implied vs. explicit rules for being able to communicate with/create objects with the subjects labeling
(01:30:33 PM) jjohansen: kshitij8: yes
(01:30:45 PM) kshitij8: how do you resolve it then?
(01:31:17 PM) jjohansen: kshitij8: lets discuss that later
(01:31:23 PM) kshitij8: let darix's topic be discussed I'll wait :)
(01:32:16 PM) jjohansen: so darix do you want to list your objections to the implied rules
(01:32:22 PM) darix: my point was simply: i dont like to have implied permissions that process can use anon unix domain sockets with itself.
(01:32:28 PM) jjohansen: are there any I missed in my email?
(01:32:50 PM) darix: jjohansen: i am sorry i was busy with work. if you send another mail i havent read it yet. 
(01:33:05 PM) jjohansen: darix: yeah I sent a little addendum
(01:33:38 PM) jjohansen: basically as I understand it darix wants to be able to capture the behavioral semantics of the application in the profile
(01:33:44 PM) darix: given that exploits got more common lately which just patch the running process without changing files on disk. i want apparmor to be more strict and dont have implied rules like this. so I will notice if the behavior changes
(01:34:04 PM) jjohansen: this allows reducing the attack vector some, and allows detecting behavior that is not expected
(01:34:07 PM) darix: if we want to make it easier for people to allow this behavior. we can have abstractions.
(01:35:05 PM) jjohansen: it will be fairly easy to add the behavior of what was going to be done with the implied rules, it requires only one or two rules so includes aren't really needed
(01:35:51 PM) darix: jjohansen: well if the rules are basically the same in all profiles, which use the behavior, then let's use DRY
(01:35:57 PM) sarnold: I liked the implied rules; it feels like a nice feature from SMACK to steal. :)
(01:35:57 PM) darix: :)
(01:36:01 PM) jjohansen: on the other hand it does require adding perms to profiles to be able to deal with objects of your own labeling
(01:36:55 PM) jjohansen: darix: well that is just it, they may be the same in most profiles but, obviously not in the ones you want
(01:37:11 PM) jjohansen: and undoing them is a lot more effort than adding them
(01:37:23 PM) tyhicks: I don't like the idea of the implicit rules if the mediator is in userspace (dbus, display manager, etc.)
(01:37:33 PM) darix: jjohansen: i thought about a simple thing like abstractions/anon-unix-domain-sockets-with-self
(01:37:42 PM) jjohansen: sarnold: so it is simple but it doesn't take much to add them to the profile yourself
(01:37:47 PM) tyhicks: It allows for a compromised process to leak data outside of its address space
(01:38:34 PM) jjohansen: tyhicks: that assumes the rule isn't actually encoded, it would have to be so you can deny parts of it
(01:39:46 PM) jjohansen: I'm on the fence, its a nice simplification, but it doesn't gain us much, basically removes 1 rule from the profile, and looses/reduces the ability to encode certain expected behavior
sbeattie sbeattie_ 
(01:39:53 PM) jjohansen: sbeattie, jdstrand?
(01:40:43 PM) tyhicks: jjohansen: I don't follow.. I'll walk through what I am talking about a little more...
(01:41:04 PM) sarnold: jjohansen: label equality -> grants access just feels like something that's now so deeply baked into the kernel code, I don't see how we'd special case around it for just a handful of things like unix domain sockets...
(01:41:30 PM) jdstrand: sorry, got pinged elsewhere
(01:41:52 PM) jjohansen: sarnold: see option 1 of no create on implicit
(01:41:52 PM) tyhicks: if 'dbus label=@{self},' is implied in all profiles, then a compromised process could open dbus connection(s) and send data across dbus back to itself
(01:42:01 PM) tyhicks: another process could be eavesdropping on the bus
(01:42:07 PM) tyhicks: and capture the data sent
(01:42:58 PM) jjohansen: assuming dbus allows the other process to eves drop on the compromised process
(01:43:13 PM) tyhicks: right
(01:43:24 PM) jjohansen: so I don't know who read the email I sent out
(01:43:36 PM) tyhicks: but that's an implementation detail specific to DBus
(01:44:08 PM) jjohansen: yes
(01:44:45 PM) tyhicks: my point is that the confinement around the process is leaky with implied rules and could allow a compromised process to leak data to other userspace processes
(01:44:55 PM) jjohansen: trusted helpers would still be expected to query policy as they currently do
(01:46:04 PM) jjohansen: hrmm, I'm not sure I see it, but I will give it does open more attack vectors
(01:46:07 PM) jdstrand: I'm on the fence too
sbeattie sbeattie_ 
(01:46:35 PM) jjohansen: cboltz, sbeattie?
(01:46:41 PM) sarnold: tyhicks: in the case of dbus, that sounds like abug in dbus that needs to be fixed.. in the case of X, that sounds like a design problem from the very beginning that we'll never get to fix..
(01:46:56 PM) cboltz: I'm undecided ;-)
(01:47:05 PM) sbeattie: jjohansen: in some sense, isn't the issue because we don't have a permission allowing or denying anonymous unix sockets at all?
(01:47:16 PM) tyhicks: sarnold: but what if the confined app has no business of ever connecting to dbus, X, etc., in the first place
(01:47:17 PM) sbeattie: at least for unix domain sockets.
sbeattie sbeattie_ 
(01:47:23 PM) jjohansen: sbeattie: but we do
(01:47:25 PM) jdstrand: jjohansen: you said to implement the implied rules in actual policy is one or two rules?
(01:47:29 PM) cboltz: do you have a rough number about the percentage of processes/binaries that would need this rule?
(01:47:46 PM) jjohansen: cboltz:  I don't
(01:47:56 PM) sarnold: tyhicks: ah, so you'd like to avoid adding "deny dbus, deny X," etc rules ... makes sense..
(01:48:24 PM) jjohansen: and even if they needed it, it could be limited to specific types, like pipe and unix domain instead of being wide open
sbeattie sbeattie_ 
(01:49:06 PM) jjohansen: sbeattie: or I should say we will have unix domain socket permissions
(01:49:15 PM) sarnold: well, I liked the implicit, but option #1 Partial implicit feels reasonable too.
(01:49:39 PM) jjohansen:   unix rw name=\0foo,
(01:49:42 PM) darix: tyhicks: that was my objection too basically. if i dont expect app A to use those kind of sockets i want to apparmor to tell me if it tried.
(01:49:56 PM) jjohansen:   unix rw,
sbeattie sbeattie_ 
(01:50:39 PM) tyhicks: darix: yes - no need to implicitly grant access to communication mechanisms if the application never intended to use any of those mechanisms
(01:51:03 PM) jjohansen: sbeattie unix domain will overlap with fs still, but the anonymous and abstract sockets will always be specified via a unix ... rule
sbeattie sbeattie_ 
(01:51:31 PM) tyhicks: sarnold: the problem with #1 Partial Implicit is that not everything has a "create"
(01:51:34 PM) sbeattie: jjohansen: right, but the anonymous doesn't have a name at all.
(01:51:45 PM) sarnold: tyhicks: oh?
(01:51:50 PM) jjohansen: sbeattie to communicate with another unix socket via label
(01:51:50 PM) jjohansen:   unix rw peer=(label=foo)
sbeattie sbeattie_ 
(01:52:01 PM) tyhicks: sarnold: yes - dbus, for example
(01:52:08 PM) tyhicks: (that could be added, though)
(01:52:39 PM) jjohansen: sbeattie right, rules that don't specify a name or specify they are only anonymous apply
(01:52:58 PM) jjohansen:   unix rw peer=(label=foo)
(01:53:15 PM) jjohansen:   unix rw anonymous,
(01:53:33 PM) jjohansen: or what ever the exact syntax for anonymous will be
(01:53:55 PM) jjohansen: same idea for pipe, except they are always anonymous
(01:54:58 PM) sbeattie: I lean ever so slightly towards not making it implicitly allowed; for example we don't implicitly allow a binary to re-exec itself.
(01:55:26 PM) sbeattie: I'm not sure how to make the syntax of it work.
(01:56:41 PM) jjohansen: alright, so I think that is at least 3 for not being implicit, 2 on the fence, 1 in favor, and a few abstaining
(01:57:22 PM) jjohansen: so I think we will try with explicit and see how it impacts policy and revisit in a month or two
sbeattie sbeattie_ 
(01:57:49 PM) jjohansen: moving on sbeattie, sarnold 2.8.2 status?
(01:57:55 PM) sbeattie: (I worry about applying this to pipes and file descriptors generally, as it will mean capturing a lot of behavior)
(01:58:25 PM) sarnold: jjohansen: I'd like Message-ID: <20130709003645.GA1428@hunt> to be included before building a 2.8.2
(01:59:04 PM) jjohansen: sarnold: what needs to be done to include it?
(01:59:12 PM) sarnold: jjohansen: someone else to ack it :)
(01:59:33 PM) sbeattie: sarnold: I'd like some evidence that someone has tried the patch and the @options is the right thing.
(02:00:18 PM) sarnold: sbeattie: yeah, makes sense.. lazyness:1 sarnold:0
sbeattie sbeattie_ 
(02:01:03 PM) jjohansen: sbeattie: sheesssh, keeps setting the bar high with his demands for tests ...
(02:01:16 PM) cboltz: sbeattie: kshitij8 said @options is correct
(02:01:31 PM) cboltz: (but I have to admit I didn't test it)
(02:03:53 PM) sarnold: so, aside from spinning up a vm to test out this patch, are there any other patches I've overlooked for 2.8.2? I think cboltz collected (and merged, right?) just about everything that was still outstanding..
(02:04:01 PM) kshitij8: I had tried a previous version and this a neater version for the same thing.
(02:04:46 PM) cboltz: sarnold: no guarantees given ;-)
(02:04:47 PM) jjohansen: sarnold: cboltz collected a few but I don't believe he did the log check, where we compare dev and 2.8
(02:04:59 PM) sbeattie: sorry, I meant to do that.
(02:05:10 PM) sarnold: jjohansen: your (very nice) description of the alias work mostly made sense with what I read, but I'd still like to suggest that'd be a 2.8.3 fix instead, even if in a week or two..
(02:06:01 PM) jjohansen: sarnold: yes I don't want to delay 2.8.2, we should have some tests for the alias work before it goes in
(02:06:21 PM) jjohansen: cboltz: can distro patch it in if he wants until 2.8.3 is released
(02:07:12 PM) jjohansen: cboltz: so the plan is to get it into the 2.8 tree after 2.8.2 is released but 2.8.3 could still be several months away
sbeattie sbeattie_ 
(02:07:37 PM) jjohansen: Alright sarnold, sbeattie: can you get 2.8.2 released this week?
(02:07:41 PM) sbeattie: well, if we're going to release a 2.8.3 soonish because of that fix, then I think we can skip the dev bzr log review and move on to the 2.8.2 release process
(02:07:53 PM) sbeattie: jjohansen: yes
sbeattie sbeattie_ 
(02:07:59 PM) cboltz: I'm not sure if I want to take the risk to get the alias patch into an update for all released openSUSE versions ;-)
(02:08:00 PM) jjohansen: sbeattie: that is an option
(02:08:14 PM) jjohansen: cboltz: fair enough
(02:08:14 PM) cboltz: but maybe I'll include it in factory only (which would also fix my system ;-)
(02:08:42 PM) sarnold: spending time in factory would help ease my conscience :)
(02:08:55 PM) jjohansen: okay lets move onto 3.0 status
(02:09:36 PM) jjohansen: we have what I would consider alpha3 sitting in ppas, and upstream ubuntu in the case of the kernel
(02:10:09 PM) jjohansen: it has an abi and language that will change based on the current dbus/ipc discussion
(02:10:26 PM) jjohansen: should we do a wider release of this alpha
(02:11:54 PM) sbeattie: is there anyone outside of ubuntu that would test it?
(02:12:27 PM) kshitij8: sbeattie: the patch work for me. :)
sbeattie sbeattie_ 
(02:13:13 PM) jjohansen: sbeattie: that is a good question
(02:13:51 PM) kshitij8: *works
(02:14:11 PM) cboltz: I can package it for openSUSE, but I'm not too keen on pushing an alpha into factory
(02:14:14 PM) tyhicks: we're no longer blocked on the dbus syntax so now the end of that work is in sight
(02:14:18 PM) cboltz: so the number of testers will be quite limited
(02:14:41 PM) darix: especially with 13.1 forming there atm
(02:15:12 PM) sbeattie: cboltz: that's what I expect, so I'm kind of thinking that waiting for the dbus final syntax to land is a better time for an alpha release.
(02:15:50 PM) darix: cboltz: if the packaging isnt such a big issue i would still vote for packaging them in the devel project
(02:16:54 PM) jjohansen: darix: I think with the other issue involved with this alpha it may be worth waiting for the next one
(02:17:11 PM) jjohansen: I'd like to target the next alpha for 1 month
(02:17:12 PM) cboltz: darix: shouldn't be too hard (but in a :unstable project, not in the devel project)
(02:17:20 PM) jjohansen: so start of august time frame
(02:18:22 PM) jjohansen: and we are looking at beta being the 1st of september end of august
(02:19:17 PM) cboltz: that sounds like an interesting race - openSUSE 13.1 beta is planned for sept 19
(02:19:40 PM) cboltz: any guesses about the final release date?
(02:20:06 PM) jjohansen: ubuntu's beta1 is september 5
(02:20:36 PM) jjohansen: cboltz: we need a stable release before the end of september
(02:20:55 PM) cboltz: sounds good :-)
(02:20:58 PM) jjohansen: unfortunately we have slipped at least 2 months on our planned schedule for 3.0
(02:21:13 PM) jjohansen: we wanted to go into beta at latest by the start of august
(02:21:27 PM) jdstrand: with tyhicks finalizing the dbus bits, maybe wait til after he is done?
(02:21:30 PM) jjohansen: but dev has just taken a lot longer than planned
(02:21:46 PM) jdstrand: jjohansen: would we be able to push that alpha into Ubuntu 13.10 then?
(02:22:07 PM) jjohansen: jdstrand: yes, we will put the revised dbus in the next alpha, I would recommend anyone who wants to try an alpha wait until the next one
(02:22:40 PM) jjohansen: jdstrand: we can push a new kernel, alpha into ubuntu when ever we are ready
(02:22:48 PM) jdstrand: ok
(02:23:13 PM) jdstrand: seems fairly logical for the alpha to wait at least until tyhicks is done with dbus bits
(02:23:24 PM) ***tyhicks agrees
(02:23:33 PM) jjohansen: so if we want to do an alpha before august great, but we need to have another one by the start of august
(02:24:29 PM) tyhicks: adjusting the dbus patches will be my sole focus starting tomorrow
(02:25:21 PM) jjohansen: okay lets move on the
(02:25:38 PM) jjohansen: kshitij8: you wanted to discuss profile abstractions around interpreters
(02:25:47 PM) jjohansen: you have the floor
(02:26:36 PM) jdstrand: I don't have a major opinion on alpha now and alpha in august. I'd like to get the dbus bits into Ubuntu this month if that is feasible, so if an alpha release makes sense for that, that's cool. but we can discuss that elsewhere
(02:26:45 PM) kshitij8: no, I'll pass. I see the idea of avoiding x permissions in abstractions is convincing. :)
(02:28:47 PM) darix: maybe the profile parser could warn if it finds such things?
(02:29:30 PM) sarnold: darix: <abstractions/perl> already has them :(
(02:30:01 PM) darix: sarnold: i actually thought about profiles for scripts
(02:30:09 PM) darix: e.g. if it would make sense to have something like 
(02:30:12 PM) sbeattie: I think the parser shouldn't be concerned, but we've had the idea of having a lint tool for other things, this seems like it could go on the list.
(02:30:16 PM) darix: /usr/bin/foo {
(02:30:31 PM) darix:  /usr/bin/ruby px -> real_script
(02:30:39 PM) darix: ...
(02:30:40 PM) darix: }
(02:30:40 PM) darix: }
(02:30:59 PM) darix: so the real ruby app cant exec ruby itself afterwards
(02:31:26 PM) darix: still have to test how much "fun" that is
(02:31:37 PM) jdstrand: that's interesting
(02:32:08 PM) darix: normal profiles for scripts have rix on the interpreter
(02:32:16 PM) darix: jdstrand: hm?
(02:32:18 PM) sarnold: darix: it might not be able to just ruby -e 'bad things here' but they could do execve(bad things here) themselves... 
(02:32:31 PM) sarnold: darix: or just do the bad syscalls themselves..
(02:32:45 PM) jdstrand: darix: I was simply saying that your idea was thought-provoking
(02:32:50 PM) sbeattie: darix: I suspect many shell scripts will do /$() so will need to have ix on /bin/sh anyway. 
(02:33:05 PM) sbeattie: (well, on whatever shell they are)
(02:33:27 PM) darix: sbeattie: there are always corner cases where you cant get around it. but if possible. i try to lower the amount of board x flags :)
(02:34:07 PM) sbeattie: nope, it's an interesting approach.
(02:34:19 PM) darix: s/board/broad/
(02:34:27 PM) ***darix adds it to the weekend todo
(02:34:57 PM) sbeattie: (I'm not sure the current exec lsm hook would support that...)
(02:35:25 PM) darix: sbeattie: if you see me here angry on the weekend you will know. ;)
(02:35:52 PM) sbeattie: darix: I avoid the kernel code for good reasons... :)
(02:35:57 PM) jjohansen: as implemented I don't think it does, there are things we could do to track it better, and its possible the behavior can change with changes to the bprm code without even touching the lsm :/
(02:36:43 PM) jjohansen: alright shall we move on
(02:36:55 PM) sbeattie: yep
(02:37:08 PM) jjohansen: next meeting is scheduled for tuesday, August 13, @20:00 UTC
(02:37:19 PM) jjohansen: anyone object
(02:37:46 PM) cboltz: looks good
(02:38:28 PM) jdstrand: seems fine
(02:39:03 PM) jjohansen: alright thanks everyone, meeting adjourned
``` 
