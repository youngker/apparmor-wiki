```
(01:02:40 PM) jjohansen: cboltz, sbeattie, jdstrand, tyhicks, sarnold, goldwyn: meeting time
(01:03:02 PM) ***sbeattie waves hello
(01:03:22 PM) goldwyn: hello
(01:03:33 PM) cboltz: hi
(01:05:16 PM) jjohansen: unfortunately it looks like intrigeri won't make it
(01:05:34 PM) jjohansen: and tyhicks, and jdstrand are busy as well
(01:06:05 PM) goldwyn: I assume the agenda is git migration. I would like to add, github vs gitlab. I don't have any preference per say, as long as CI comes with it. Both have CI.
(01:06:13 PM) jjohansen: we can either delay a week, or just forge ahead
(01:06:41 PM) jjohansen: goldwyn: yes that is the major item on the agenda
(01:06:52 PM) cboltz: goldwyn: the agenda is on http://wiki.apparmor.net/index.php/MeetingAgenda
(01:10:07 PM) sarnold: neat, a real agenda :)
(01:10:55 PM) jjohansen: well, at least a sort of agenda
(01:11:28 PM) ***cboltz wonders if he should add "status of the YaST AppArmor module"
(01:11:38 PM) jjohansen: cboltz: feel free
(01:12:06 PM) goldwyn: If you plan to delay (or schedule another one), is it possible to have it 2 hours early? ie 1800 UTC?
(01:13:20 PM) tyhicks: jdstrand and I are around at least for the next 15 minutes
(01:13:24 PM) jjohansen: goldwyn: yes that is possible
(01:13:31 PM) jdstrand: jjohansen: I don't know how long I will be present. I have no opinion on github vs gitlab. I have experience with github as a reviewer and committer, and it works quite well
(01:13:45 PM) jdstrand: it seems I will be here for 15 minutes
(01:13:49 PM) tyhicks: ;)
(01:13:56 PM) jjohansen: so quick vote, delay or move forward. atm we don't have a debian representative
(01:14:19 PM) jdstrand: we probably should have a Debian representative
(01:14:50 PM) cboltz: I'm familiar with github and with (a non-public instance of) gitlab, so both would work for me
(01:14:52 PM) sbeattie: maybe since he's planning a mini-debian-apparmor sprint we could reschedule for during that week?
(01:15:15 PM) jdstrand: ie, reps from people who regularly contribute. probably a mailing list vote is best
(01:15:17 PM) sbeattie: since some of us are working towards trying to allocate time to that effort?
(01:15:18 PM) tyhicks: I suspect that both would work for me, as well, but I have very little real world experience with gitlab
(01:15:32 PM) sbeattie: s/he/intrigeri/
(01:15:46 PM) tyhicks: I have read some (possibly old) blog posts complaining about poor performance at gitlab
(01:16:06 PM) jjohansen: my preference aligns with sbeattie's and integeri's already stated positions
(01:16:30 PM) tyhicks: because of gitlab being open?
(01:16:54 PM) tyhicks: I think that is definitely a positive
(01:16:55 PM) jjohansen: I prefer gitlab from an ethics pov, but don't have enough experience with either to express a practical functional comparison
(01:17:02 PM) jjohansen: tyhicks: yes
(01:17:14 PM) sbeattie: tyhicks: I would be surprised if the size of either of our git repos (apparmor and apparmor-profiles) would be an issue; CI performance issues on the other could be a drawback.
(01:17:33 PM) tyhicks: I will say that it wasn't long ago that apparmor was hosted in a non-open hosting solution...
(01:17:49 PM) jjohansen: right, CI, bug tracking and translations are all things to be considered
(01:18:07 PM) jjohansen: I have no practical experience with any of those on either of the platforms
(01:19:06 PM) jdstrand: I know of a few projects that host bugs in LP and code in github. there are disadvantages there, but we have a large backlog. Does the bug discussion need to be intertwined with the code hosting?
(01:19:17 PM) jdstrand: also, I know github allows for travis CI
(01:19:32 PM) tyhicks: gitlab allows for travis CI, too
(01:19:32 PM) ***jdstrand also doesn't have experience with gitlab
(01:19:44 PM) sbeattie: tyhicks: a couple of them, actually, but launchpad has been open source (mostly) since 2009.
(01:19:54 PM) tyhicks: err, I guess gitlab has gitlab CI
(01:20:44 PM) jjohansen: jdstrand: yes it needs to be intertwined a bit, as we need to know what our options are, and what costs they have
(01:20:50 PM) sbeattie: so, wrt CI, one of the issues is that we want to test against a variety of (distro) kernels.
(01:21:11 PM) jjohansen: that doesn't mean we have to switch bug systems at the same time as code hosting
(01:21:16 PM) jdstrand: I think someone may have mentioned this somewhere, but would one over the other encourage more participation? I mean github is certainly popular, but is our demographic going to prefer gitlab over github?
(01:21:31 PM) sbeattie: I know we can test against a bunch of distro userspaces, but are they run in docker containers or full vms?
(01:21:53 PM) jjohansen: jdstrand: I have the same question, and I don't think we have the information to know
(01:22:28 PM) jjohansen: but I do think our demographic does adjust the skew, so that its not so lopsided in githubs favor, where it lands, who knows
(01:23:16 PM) sarnold: what's the minimum effort involved in submitting a patch via gitlab vs via github?
(01:23:30 PM) jjohansen: sbeattie: what of launching on our own servers? One of use could set up VMs on one of our own hosted machines, is that something we could integrate with either github or gitlab CI
(01:23:41 PM) sarnold: loads of folks already have github accounts, whether or not they liked github, just because so many others are already there..
(01:23:44 PM) jjohansen: sarnold: they are roughly the same
(01:24:05 PM) jdstrand: apparently github allows using any number of CI systems
(01:24:22 PM) jdstrand: most seem to be container based or $$ for booting your own kernel
(01:24:24 PM) jjohansen: sarnold: sure, but again just because its tons of people doesn't mean its skewed so heavy with the demographic that we interact with
(01:25:09 PM) jdstrand: however, they do allow hooking into jenkins. the snapd team has similar requirements for CI
(01:25:21 PM) sarnold: jjohansen: good point.
(01:25:26 PM) jdstrand: they use github + jenkins + linode
(01:25:59 PM) jdstrand: in this manner, they can control the contents of the vms and load arbitrary policy/etc/etc
(01:26:05 PM) sarnold: i've never seen 'circus issues' on gitlab, so i'm slightly inclined to favour it :) but I've never actually -used- gitlab so it's not really a strong endorsement
(01:26:34 PM) tyhicks: I would strongly prefer that we don't have to maintain our own testing infrastructure if we don't have to
(01:26:40 PM) tyhicks: maybe it will end up being required, though
(01:27:00 PM) sbeattie: yes, having maintained a personal jenkins server, I'd like to get out of that task.
(01:27:58 PM) sarnold: is it jenkins that sucks or maintaining the vms that sucks or the .. whole thing that sucks? :)
(01:28:31 PM) jdstrand: sarnold: github is very pleasant to use in my experience. I am not a kernel developer, so not sure how the flow maps to how people like to do things. You can use the cli to do a lost of things. so I would fork, create a branch, push the branch, then go to the ui to request a PR/fill out the title and summary of the PR
(01:28:56 PM) jdstrand: the review process is quite nice
(01:29:05 PM) sbeattie: sarnold: heh, my jenkins machine was underpowered enough that I didn't have the room to set up vms, but trying to deal with build changes across different branchs of apparmor versus different releases of just ubuntu was a hassle enough.
(01:29:10 PM) jdstrand: you can block on CI/etc
(01:29:19 PM) tyhicks: it would be better if we could spend our time writing new tests than maintaining the environment to run them :)
(01:29:26 PM) sbeattie: indeed
(01:29:47 PM) sarnold: exactly, but I could imagine "load a custom kernel" to be a sticky point for most of the available services
(01:30:09 PM) tyhicks: do we need that?
(01:30:16 PM) jdstrand: don't we?
(01:30:18 PM) jdstrand: :)
(01:30:37 PM) tyhicks: it depends on what we're continually testing
(01:31:01 PM) jdstrand: I can imagine tests for kernels with differnet features
(01:31:09 PM) jdstrand: different*
(01:31:32 PM) sbeattie: not sure we need exactly that, more like, "things don't break when used on ubuntu 16.04 kernel or debian stable or opensuse"
(01:31:33 PM) tyhicks: there's a large part of our tests that are kernel agnostic
(01:31:48 PM) tyhicks: lets not maintain infrastructure for those tests
(01:31:57 PM) jdstrand: however, just cause I can imagine it doesn't mean it is what we should do. however, if we don't have control of the kernel, I suspect we'll end up stuck on something old
(01:32:16 PM) jdstrand: eg, I've heard of some CI systems only just upgrading to Ubuntu 14.04
(01:32:55 PM) sbeattie: oof
(01:33:30 PM) jdstrand: so, github is flexible, aiui (no personal experience). perhaps we could focus on parser autopkgtests there with one of the free CI systems. if we want to expand, we can explore jenkins/etc
(01:33:41 PM) sbeattie: well, OTOH, we could start with what the existing CI supports and then look at expending with our own needs.
(01:33:53 PM) sbeattie: jdstrand beat me to it.
(01:33:53 PM) jdstrand: err, github CI is flexible (no personal experience)
(01:33:55 PM) jjohansen: we need testing of custom kernels, or I am going to be rolling out testing of kernel dev outside of the infrastructure
(01:35:02 PM) goldwyn: I doubt if any public CI provides custom kernels.. or even distros.
(01:35:31 PM) jdstrand: so, I'm confident we could glom onto what the snapd team is doing and copy that to get something started. I don't think linode is free in the way they use it (though, aiui, it is reasonably priced)
(01:35:45 PM) tyhicks: off-the-shelf public CI  would fully support parser tests, python utils tests, libapparmor tests
(01:37:35 PM) tyhicks: as a point of reference, the libseccomp project doesn't run kernel tests as part of their CI process when a pull request is made
(01:37:59 PM) jjohansen: thats fine
(01:38:07 PM) jdstrand: so, I think we could go as little or as far as we want wrt CI with github
(01:38:15 PM) jdstrand: what does gitlab offer?
(01:38:31 PM) tyhicks: https://about.gitlab.com/comparison/gitlab-vs-travis-ci.html
(01:38:32 PM) jjohansen: its more like I want to kick of the CI when I have a new kernel to test, or weekly to ensure that upstream kernel changes haven't broken something
(01:39:22 PM) tyhicks: yeah, that makes sense
(01:39:30 PM) sarnold: wow that travis icon is uuuuugly
(01:40:16 PM) tyhicks: jjohansen: I'm not completely convinced that testing needs to be an integrated part of the userspace project hosting solution (but it sure would be nice if it was)
(01:40:34 PM) tyhicks: (by "that testing", I'm talking about the kernel testing you mentioned)
(01:40:43 PM) jdstrand: https://docs.gitlab.com/ee/integration/jenkins.html
(01:41:01 PM) jdstrand: so, if they can do jenkins, we can presumably do jenkins+linode
(01:41:53 PM) jjohansen: tyhicks: I agree, I just want to use as much of the same infrastructure as possible, if one makes it easier that would be a nudge in that direction
(01:42:03 PM) jdstrand: which means, it seems at a high level, neither is a clear winner (again, having no personal experience with gitlab ci)
(01:42:03 PM) jjohansen: if not, well I'll just cry in my corner
(01:42:48 PM) tyhicks: jjohansen: yeah, reusing as much as possible would be nice
(01:43:16 PM) jjohansen: so what I am hearing is we don't have enough info to make an informed decision
(01:43:22 PM) jdstrand: there is a good deal of minshare around github with the projects I work with
(01:43:26 PM) jdstrand: mindshare
(01:43:40 PM) jdstrand: (I don't know if that is a nudge or not)
(01:43:52 PM) jjohansen: is it possible, we could do some digging, and reconvene in 2 weeks during the debian apparmor sprint
(01:44:44 PM) jjohansen: jdstrand: well it gives you some experience around github so that counts for something
(01:44:44 PM) tyhicks: might be useful to read up on how other kernel projects are doing CI in the meaning (https://criu.org/Continuous_integration#Kernel_testing)
(01:45:11 PM) jjohansen: indeed
(01:46:51 PM) jjohansen: alright lets table the discussion for two weeks and tentatively set the meeting for 18:00, Tuesday Oct 24
(01:47:21 PM) jjohansen: cboltz: you wanted to bring up the Yast2 module, go
(01:48:24 PM) cboltz: well, I'm just abusing the fact that goldwyn is here and want to ask about the status of the rewrite ;-)
(01:49:04 PM) sarnold: :)
(01:49:21 PM) jjohansen: as long as its goldwyn and not me :)
(01:49:29 PM) cboltz: so - goldwyn, when can I expect the new version of the YaST2 AppArmor module in Tumbleweed?
(01:49:33 PM) goldwyn: Well, we managed to remove all of the perl dependency by using JSON. Lost "Edit profile" in the process.
(01:52:05 PM) goldwyn: But I need to think of a way to get "Edit profile" back. Just that we don't have all options for it. We may end up using just part of available edit options.
(01:52:54 PM) cboltz: do you think the edit mode has to be available in each interface (Qt, Gtk, text)?
(01:53:22 PM) cboltz: it's probably much easier to write a Qt-only editor in python because that could easily use the AppArmor code
(01:54:02 PM) cboltz: writing an editor from scratch (even if you support only some of the rules) is probably much more work
(01:54:19 PM) goldwyn: Yes, if it is available in one, it needs to be in all. Yast is just a library with different ways to render it.
(01:55:10 PM) cboltz: my idea would be that YaST only starts it as an external application
(01:55:51 PM) goldwyn: I am sure that will be rejected by the yast community, but I can ask. 
(01:55:57 PM) cboltz: and that external application would be written in python and only use a Qt interface (with pyQt or whatever)
(01:56:09 PM) cboltz: I slightly doubt someone starts YaST in text mode to edit a profile
(01:56:15 PM) cboltz: those people will use vi anyway ;-)
(01:56:52 PM) goldwyn: yes, you are alienating a whole lot of people who call just yast , select apparmor.. from other modes
(01:57:11 PM) tyhicks: (I'm stepping away. See you all later!)
(01:58:18 PM) cboltz: it shouldn't be hard to add a "(only graphical)" to the profile editor menu option - or just hide it in text mode ;-)
(01:58:44 PM) cboltz: I know and understand that YaST aims to support text and graphical mode equally
(01:58:55 PM) goldwyn: I am thinking of using /sys/kernel/security/apparmor/features, but it would not be complete and would need translations/keywords for apparmor.
(01:59:47 PM) cboltz: but I can promise you that writing a profile editor in ruby (which you'll probably need to do if you write it as a "real" YaST module) will be *lots* of work
(02:00:50 PM) cboltz: so if you want to do it - no objections, but you have been warned ;-)
(02:01:14 PM) goldwyn: yes, if only we had a way to get a list of "apparmor capabilities" and the "keystrings" associated with it.
(02:01:43 PM) sarnold: can you cheat and kick off sensible-terminal -e "$EDITOR /patht/to/profile" kind of thing?
(02:02:51 PM) goldwyn: sarnold: yes, but thats not helpful. UI should be able validate, give options etc. the clickety-click stuff.
(02:03:14 PM) cboltz: it should be possible to query the python classes somehow (using a python helper that answers in json)
(02:03:44 PM) cboltz: but given how different the various rule types are, this sounds easier than it probably is in practise
(02:04:08 PM) sarnold: goldwyn: cripes. that's a different kind of tool entirely. a useful thing perhaps but .. wow.
(02:05:23 PM) cboltz: I have a feeling that this might be another case of doing a json-based frontend
(02:05:39 PM) cboltz: so keep most code in python (so that it can use the AppArmor classes etc.)
(02:06:05 PM) goldwyn: cboltz: It would not be complete. But I think that is where we can start. I am trying to get around creating a list of doc/table which has supported options. But does userspace (backports included) know every available option?
(02:06:16 PM) cboltz: and when a user clicks   edit THIS rule   ask the python helper what can be edited, and in which ways
(02:06:33 PM) cboltz: then give the changed rule back to the python helper to validate it
(02:07:23 PM) cboltz: most rule types are in classes (utils/apparmor/rule/*.py)
(02:07:54 PM) cboltz: adding some information about what is available is possible
(02:07:58 PM) goldwyn: validation should be okay.. it is the list of possible options which I am worried about.
(02:08:03 PM) sarnold: goldwyn,cboltz, if you really want to go down this route, check out https://github.com/Microsoft/language-server-protocol/blob/master/protocol.md -- some non-trivial amount of editors already support this thing
(02:08:14 PM) cboltz: the more interesting question is how an interface that fits for all rule types would look like
(02:10:02 PM) cboltz: the answer is probably to hand over arrays, and I start to think that you should look at the PostfixAdmin *Handler classes because I do something like that in them ;-)
(02:10:23 PM) cboltz: (the classes set up a big array in initStruct(), and the list view and edit form configures itsself based on this array)
(02:12:21 PM) goldwyn: Yes, that should do it.
(02:12:23 PM) cboltz: see https://github.com/postfixadmin/postfixadmin/blob/master/model/DomainHandler.php (starting at line 47) for an example
(02:12:39 PM) cboltz: and let me warn you that it might look scary when you first look at it ;-)
(02:12:44 PM) goldwyn: BTW, do we have man pages or any documentation for options available?
(02:12:58 PM) goldwyn: maybe we can link the two together..?
(02:13:03 PM) cboltz: yes,   man apparmor.d
(02:13:20 PM) cboltz: PostfixAdmin is written in PHP, which makes this quite hard ;-)
(02:13:56 PM) goldwyn: so probably we should find a way to separate it, and generate man pages out of it.
(02:15:36 PM) cboltz: man apparmor.d   probably has all the relevant information already
(02:16:09 PM) cboltz: (some things, like the list of capabilities, is included as a pointer to other manpages, but that's fine IMHO)
(02:16:39 PM) cboltz: but yes, if we can provide the information to an editor, we can also use it for documentation
(02:16:55 PM) goldwyn: yes, what I mean is to keep all doc in parsable info which can be converted to JSON, and generate apparmor.d.man out of it.
(02:17:34 PM) sarnold: auto-generating docs from lex and yacc sounds like one of the least fun ways to spend your time
(02:18:06 PM) cboltz: that's why you want to do it from the utils - that's more fun ;-)
(02:18:13 PM) goldwyn: sarnold: I agree
(02:18:42 PM) cboltz: I'm still not sure if we can autogenerate the apparmor.d manpage, but at least I like the idea
(02:21:12 PM) cboltz: goldwyn: do you already have a hackweek project? ;-)
(02:22:20 PM) cboltz: BTW: I just managed to crash the YaST module - clicking the "New" button (to enter a custom path) results in   Undefinded method 'Inputfield' (did you mean InputField?)
(02:22:52 PM) goldwyn: cboltz: well not as yet, but it will be something in filesystem this time.
(02:23:10 PM) ***cboltz wonders - if YaST is clever enough to propose the correct name, why doesn't it simply use it?
(02:23:17 PM) goldwyn: cboltz: can you open a bz entry?
(02:23:24 PM) cboltz: yes, of course
(02:25:16 PM) goldwyn: I guess, we are done with Yast status discussion? If yes, I'll let jjohansen take over to mediate.
(02:26:02 PM) cboltz: yes
(02:26:06 PM) cboltz: next topic please ;-)
(02:26:09 PM) jjohansen: alright, since we are over time and we have a meeting in 2 weeks I will bump the remaining items to the next meeting
(02:26:18 PM) jjohansen: and call this meeting adjourned
(02:26:21 PM) jjohansen: thanks everyone
``` 
