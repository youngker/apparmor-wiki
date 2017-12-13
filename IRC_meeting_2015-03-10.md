```
(01:00:29 PM) jjohansen: sarnold, sbeattie, jdstrand, mdeslaur, tyhicks, cboltz, intrigeri: meeting time
(01:00:32 PM) jjohansen: intrigeri: right
(01:00:35 PM) mdeslaur: \o
(01:00:38 PM) tyhicks: hello
(01:00:38 PM) jdstrand: hey
(01:00:48 PM) cboltz: hi
(01:00:53 PM) ***sbeattie o/
(01:01:02 PM) jjohansen: Alright lets get started
(01:01:16 PM) jjohansen: The agenda is at http://wiki.apparmor.net/index.php/MeetingAgenda#Next_Meeting:_IRC_Meeting.2C_20:00_UTC.2C_March_10_2015
(01:01:55 PM) jjohansen: so first up (so cboltz doesn't have to ask) the wiki update: I have sent a support request to osu to update the wiki version
(01:02:03 PM) jjohansen: now it is waiting on them
(01:02:08 PM) cboltz: :-)
(01:03:08 PM) cboltz: sounds like I should have asked earlier for free drinks at debconf depending on the wiki version ;-)
(01:03:40 PM) jjohansen: that might have been motivating (I'll never tell! :P)
(01:04:09 PM) jjohansen: alright next up, intrigeri is there anything specific you would like to add to the agenda around debian/tails?
(01:04:19 PM) intrigeri: nope.
(01:04:24 PM) intrigeri: MoC: meeting starts
(01:05:01 PM) jjohansen: ah yes I should learn to do that :)
(01:05:25 PM) jjohansen: s/learn/beat it into head and fingers to do that
(01:05:44 PM) jjohansen: okay moving on, next item 2.9.2
sbeattie sbeattie_ 
(01:06:00 PM) jjohansen: sbeattie: where are we at on cutting a 2.9.2?
(01:06:26 PM) MoC: heya
(01:06:28 PM) sarnold: heyooo :)
(01:06:30 PM) MoC: sorry.
(01:07:43 PM) sbeattie: jjohansen: I'm hoping to release it soon, but I wanted to get more of cboltz's patches reviewed and in before doing so.
sbeattie sbeattie_ 
(01:08:13 PM) jjohansen: sbeattie: so we are hoping to roll most of those into 2.9.2 instead of just 2.10?
(01:08:30 PM) sbeattie: That said, if it made a downstream's life easier to have it released, we could relesae it pretty much right away, I think.
(01:09:08 PM) cboltz: right, and we should probably also include the patch that finally fixes the "profile foo /bar" handling (I'll probably send it this week)
(01:09:19 PM) cboltz: oh, and I'd like to get https://bugzilla.opensuse.org/show_bug.cgi?id=921098 in (profile updates for Samba 4.2)
(01:09:20 PM) jjohansen: cboltz: any thoughts on them landing in 2.9.2 vs. say 2.10 next month
(01:09:57 PM) cboltz: the final patch of the series will be a bugfix (with lots of preparation patches ;-) so it should go into 2.9.x
(01:10:26 PM) cboltz: since this was broken since quite some time (probably already in the perl tools), I don't care too much about the value of x
(01:10:39 PM) jjohansen: well I get wanting a bug fix in 2.9.x, its just the lots of preparation patches that bothers me
(01:11:00 PM) cboltz: OTOH, if we decide to include the updated syslog profiles in 2.9, we should also include the patch that fixes the handling in aa-logprof ;-)
(01:11:27 PM) jjohansen: well it is a bug fix :)
(01:11:32 PM) cboltz: the preparation patches are low risk IMHO ;-)
(01:11:44 PM) jjohansen: okay
(01:11:50 PM) cboltz: the only thing that can really break is splitting off the functions
(01:12:00 PM) cboltz: everything inside the functions got test code added
(01:12:22 PM) jjohansen: already if you think 2.9.x is the place I am fine with that
(01:13:01 PM) jjohansen: alright lets move onto 2.10
(01:13:31 PM) jjohansen: tyhicks: want to provide a little update on where dbus is at
(01:15:16 PM) tyhicks: jjohansen: it is merged :)
(01:15:25 PM) intrigeri: \o/
(01:15:40 PM) sarnold: \o/]
(01:15:58 PM) ***tyhicks looks for the version that it was released in
(01:16:17 PM) jjohansen: thank you tyhicks for doing all the work to get the dbus mediation merged
(01:16:26 PM) tyhicks: It made it into 1.9.12: http://permalink.gmane.org/gmane.comp.freedesktop.dbus/16167
(01:16:35 PM) tyhicks: no problem!
(01:16:46 PM) sbeattie: tyhicks: woot, thanks for persevering!
(01:16:55 PM) tyhicks: jjohansen: thanks for providing a great start to the patches! :)
(01:17:01 PM) ***jjohansen should note that the dbus mediation still relies on a patched kernel because some of the features it relies on have not landed upstream
(01:17:19 PM) jdstrand: I know upstreaming is on the agenda
(01:17:39 PM) jjohansen: yeah which takes us nicely into the next point on the agenda
(01:17:52 PM) jdstrand: I wonder if it makes sense to comment on when the dbus kernel upstreaming is planned now that dbus has it in 1.9.12
(01:17:58 PM) jdstrand: ah, heh
(01:18:23 PM) jjohansen: so the kernel upstreaming is going to move in pieces
(01:18:25 PM) jdstrand: I admit, I'm fairly hazy on the upstreaming plan of attack (not that it all needs to be laid out now)
(01:18:48 PM) jjohansen: there are some base patches and bug fixes that will be sent up this week
(01:19:11 PM) tyhicks: jjohansen: will that include the dbus support?
(01:19:30 PM) jjohansen: the next set will be the base patchset to introduce the labeling structure and code, that will not add new functionality
(01:20:02 PM) jjohansen: the next patchset after that should add the labeling work around sockets
(01:20:17 PM) jjohansen: this is the bit needed for dbus support
(01:20:32 PM) jjohansen: there is no reason these two patchsets can't land in the same kernel
(01:20:51 PM) tyhicks: so the "base patches and bug fixes" will land in 4.1
(01:20:55 PM) jjohansen: I just want to separate out the logical progression of the changes, to make review either
(01:21:09 PM) jjohansen: tyhicks: that is the plan
(01:21:31 PM) tyhicks: the next two mentioned will land in 4.2 to 4.3? (maybe one each)
(01:22:25 PM) jjohansen: I expect more feedback/review on an feature set that adds new features so I am not going to predict when a given feature will land, but yeah I expect probably in 4.2 or 4.3
(01:22:30 PM) tyhicks: I shouldn't have said "will land"... rather, those are the targets
(01:22:42 PM) ***tyhicks nods
(01:22:50 PM) jjohansen: though if things go well it is possible we could land some with the base set in 4.1
(01:22:50 PM) tyhicks: nice! :)
(01:23:26 PM) tyhicks: I'd imagine that sarnold and I should start penciling in some kernel patch review time each week
(01:24:38 PM) jjohansen: yeah, I am sure I can bombard you with more than enough patches to regret that
(01:25:00 PM) tyhicks: hey, somebody has to review them
(01:25:07 PM) ***tyhicks shrugs
(01:26:08 PM) jjohansen: yes, and the canonical kernel team has offered a sacrificial lamb to help us there :)
(01:26:30 PM) tyhicks: yes! we really need to start leveraging him
(01:26:48 PM) tyhicks: these first couple rounds of patch review is probably a great start
(01:26:59 PM) jjohansen: but we should throw as much review at them as possible
(01:27:03 PM) tyhicks: that way he can ease into the code
(01:27:06 PM) jjohansen: yep
(01:27:10 PM) tyhicks: great idea, jjohansen 
(01:27:28 PM) jjohansen: alright lets move onto systemd
(01:27:31 PM) tyhicks: jjohansen: please add him to cc on the patches that you send out
(01:27:38 PM) jjohansen: tyhicks: ack, I will
(01:28:03 PM) ***jjohansen will adds a note to add him to the apparmor ml white list as well
(01:29:03 PM) jjohansen: so systemd integration is running behind, currently the libapparmor patches to handle caching and loading are under review
(01:29:35 PM) jjohansen: once this is done, the work to link systemd against libapparmor and do direct loading of policy can be done
(01:29:48 PM) tyhicks: yes
(01:29:57 PM) tyhicks: all patches needed to support systemd cache loading should be out on the list: https://lists.ubuntu.com/archives/apparmor/2015-March/007320.html
(01:30:09 PM) jjohansen: I believe the plan is currently to include the necessary library work in 2.10
(01:30:29 PM) jjohansen: tyhicks: yep, I expect to finish up with my review of them today
(01:30:31 PM) tyhicks: systemd will need to add some code sort of like what I explained here: https://lists.ubuntu.com/archives/apparmor/2014-December/006973.html
(01:30:40 PM) tyhicks: perfect!
(01:30:41 PM) jjohansen: right
(01:31:37 PM) jjohansen: once we have the systemd patch, testing will begin on that and we will work on landing it in upstream systemd
(01:31:47 PM) jjohansen: this will be after the 2.10 release
(01:32:03 PM) jjohansen: since we want to rely on the upstream library have support
(01:33:06 PM) jjohansen: since I have been asked a couple of times since last meeting, I don't believe the initial systemd support will have the ability to specify apparmor policy for the unit being loaded
(01:33:09 PM) jdstrand: jjohansen (and tyhicks): ah yes, great idea getting him involved via patch reviews
(01:33:29 PM) jjohansen: it is something we want to do, just not likely in the first pass
(01:33:39 PM) Guest543 left the room (quit: Ping timeout: 480 seconds).
(01:33:54 PM) tyhicks: jjohansen: hmm... I wasn't aware of that requirement
(01:34:03 PM) tyhicks: jjohansen: I'll see if that is something that I can add on easily enough
(01:34:12 PM) tyhicks: (in time for 2.10)
(01:34:20 PM) jjohansen: tyhicks: its a feature we have in upstart and it has been requested
(01:34:21 PM) intrigeri: so, that's independent from the AppArmorProfile= directive that systemd already supports. got it.
(01:35:06 PM) sarnold: tyhicks: darix had some idea of parametric policy directives for systemd, might be worth poking him to explain, he's dealt with systemd a lot longer than we have..
(01:35:13 PM) jdstrand: I thought what we had in upstart was equivalent to the AppArmorProfile= directive?
(01:35:37 PM) jjohansen: intrigeri: well not necessarily, its a matter of making sure everything is full supported and good
(01:35:37 PM) jdstrand: ie, change profile, not load policy
(01:35:44 PM) jjohansen: jdstrand: ack
(01:36:15 PM) intrigeri: (note that there are some issues currently with AppArmorProfile=, that may affect the library code too. these problems are in the ordering of setting up namespaces and loading the AA profile, IIRC. Michael Scherer is the person to talk to about such matters.)
(01:36:39 PM) jjohansen: right
(01:37:02 PM) tyhicks: intrigeri: is there actually any policy loading involved with the AppArmorProfile= directive or is it just a profile switch?
(01:37:04 PM) intrigeri: (he's always happy to give us a hand, don't hesitate asking him.)
(01:37:20 PM) ***tyhicks is reading the man page and not seeing how a policy load would be involved
(01:37:21 PM) jjohansen: intrigeri: the base is there, it just needs work to make sure everything is working in the right order/together as it should
(01:37:26 PM) intrigeri: tyhicks: just a switch.
(01:37:31 PM) tyhicks: oh, ok
(01:37:35 PM) jdstrand: afaik, it is only changing profile, but I've not dug into it at all
(01:37:39 PM) intrigeri: tyhicks: details are in systemd.exec(5)
(01:37:43 PM) tyhicks: thanks
(01:37:50 PM) mdeslaur: I confirm, it's just a switch, not a load
(01:38:54 PM) jdstrand: jjohansen: do you have more information on "I don't believe the initial systemd support will have the ability to specify apparmor policy for the unit being"
(01:39:32 PM) jjohansen: jdstrand: sure, I should have said I don't believe we will do anymore work around ...  so no change of state there
(01:39:59 PM) jdstrand: ok
(01:40:18 PM) jdstrand: the bug that intrigeri mentioned aside, we should be in pretty decent shape with what we have planned
(01:40:32 PM) jjohansen: I hope so
(01:41:00 PM) jjohansen: okay lets move on to unioning issues
(01:41:02 PM) cboltz: are there plans to replace the initscript with a systemd unit to load/reload/... all profiles?
(01:41:07 PM) ***jdstrand has to step away-- I'll read backscroll
(01:42:03 PM) jjohansen: cboltz: yes, the point of adding direct support into systemd is so that there is no need of the initscript
(01:42:36 PM) jjohansen: base initscripts will of course be kept around for systems that need them
(01:42:49 PM) intrigeri: jjohansen: I don't understand how this will work for non-daemons, but I'm sure you've thought about it so no need to explain me now.
(01:43:06 PM) intrigeri: s/non-daemons/stuff that's not started by a systemd unit/
(01:43:28 PM) jjohansen: intrigeri: actually I haven't looked into it yet
(01:43:52 PM) tyhicks: the built-in systemd support will load all of the cached policy files in early boot
(01:43:59 PM) jjohansen: right
(01:44:14 PM) jjohansen: if the policy is already loaded the transition shouldn't be a problem
(01:44:28 PM) tyhicks: there will still need to be an init script (or unit or whatever you want to call it) that checks to see if anything has changed and needs to be recompiled
(01:44:35 PM) tyhicks: that's "stage 2"
(01:44:37 PM) ***cboltz wonders if a unit with ExecStart=apparmor_parser -r /etc/apparmor.d/ (+ ExecRestart/ExecReload etc.) is enough or if it misses some things the initscript does
(01:44:37 PM) jjohansen: if you have something that wants to dynamically create policy (lxc) that would be hareder
(01:44:50 PM) intrigeri: tyhicks: thanks for clarifying :)
(01:45:14 PM) darix: if you want i can copy paste my idea
(01:45:21 PM) darix: it is still in my backlog
(01:45:37 PM) cboltz: oh, BTW, having ExecStatus= would be nice - otherwise switching to a unit would be a regression for status output
(01:45:38 PM) tyhicks: might as well capture it in the meeting log
(01:45:42 PM) jjohansen: darix: go ahead
(01:46:12 PM) darix: jjohansen: http://paste.lighttpd.net/zv#PLAdAgkkMkupIOpCe3PLczd9
(01:46:58 PM) jjohansen: thanks darix
(01:47:07 PM) sarnold: thanks darix :)
(01:47:20 PM) darix: and that you cant claim i only come here with work: https://twitter.com/neiltyson/status/575396118651084800
(01:47:45 PM) ***jjohansen will make sure to past the contents into the archive of the log so we won't loose its contents

18:06:48 < darix> for systemd integration was there something discussed to assign apparmor profiles to the unit being started?
18:07:08 < darix> e.g. imagine the openvpn@.service file
18:07:14 < darix> which you start with something like
18:07:28 < darix> systemctl start openvpn@home
18:07:35 < jjohansen> darix: yeah there are some plans to do that, but the first step is getting systemd linked against a libapparmor that provides the cache management
18:07:38 < darix> and it will load /etc/openvpn/home.conf 
18:07:46 < darix> now it would be nice that you can also say
18:07:53 < jjohansen> so it can load profiles early, and not call out to the parser
18:07:59 < darix> ApparmorContext=/usr/sbin/openvpn//%1
18:08:00 < darix> or
18:08:03 < darix> ApparmorContext=/usr/sbin/openvpn.%1
18:08:38 < darix> the later would load a specific profile while the first loads a hat inside the main profile
18:09:09 < darix> so many things 
18:09:43 < jjohansen> darix: so yeah something like that, I don't think anyone has really worried about the specifics of that part just yet
18:09:54 < darix> that way you can make sure that an openvpn client can only read the stuff for 1 connection, and if it gets exploited it couldnt just read all files
18:09:59 < darix> oki
18:10:15 < darix> openvpn is just one example where i often run into the issue
18:10:18 < jjohansen> the focus has been on fixing up the library, and getting that level of integration into systemd
18:10:22 < jjohansen> yeah
18:10:34 < darix> another would be language specific webservers like puma, gunicorn. 
18:10:50 < darix> where you might launch 3-4 apps with the same mini webserver 
18:11:04 < darix> thank you for listening^^
18:12:16 < jjohansen> darix: I'll try to remember to poke you so that you can kick and scream about it when some one is actually assigned to implement that piece of 
                      systemd integration
18:12:38 < darix> woohoo

(01:47:49 PM) darix: https://twitter.com/CCP_Guard/status/575367506531074050
(01:48:46 PM) jjohansen: hehe
(01:48:48 PM) darix: btw: I actually start to like systemd. my new server doesnt need runit anymore e.g.
(01:49:34 PM) jjohansen: okay, are we done with systemd?
(01:49:36 PM) darix: cboltz: something to consider might be an ExecStartPre which just runs the parser
(01:49:45 PM) darix: like in dry run mode
(01:50:05 PM) darix: we use it to check the config of a daemon before trying a restart/reload
(01:50:40 PM) darix: jjohansen: i am done now
(01:50:49 PM) cboltz: AFAIK the parser won't break the in-kernel profiles if it hits invalid syntax etc., so this shouldn't be needed
(01:51:07 PM) jjohansen: correct
(01:51:13 PM) intrigeri: so, next topic is: unioning issues.
(01:51:27 PM) jjohansen: so it is
(01:51:56 PM) intrigeri: I'm working on making live-boot and Tails work with overlayfs (since Linux in sid won't have aufs anymore as soon as Jessie is out), so I'm all ears and interested :)
(01:51:58 PM) jjohansen: overlayfs is currently just broken, and we are going to be working with upstream to try to get it fixed
(01:52:58 PM) darix: are the current issues documented somewhere?
(01:53:00 PM) jjohansen: upstream has already begun working on fixing many of the issues involved we just need to make sure we are on top of it and providing them feedback, and maybe a patch or two if needed
(01:53:25 PM) jjohansen: darix: only sort of in the lkml/fsdevel threads around the issues
(01:53:35 PM) jjohansen: it affect more than just apparmor
(01:53:39 PM) darix: oki
(01:54:08 PM) tyhicks: there is a bug that darix can follow
(01:54:11 PM) ***tyhicks looks for it
(01:54:19 PM) jjohansen: to summarize, basically overlayfs took some short cuts and some places the hooks see the upper (overlayfs) dentry/vfsmnt
(01:54:38 PM) jjohansen: and some places only see the lower dentry/vfsmnt (which is also a private clone mnt)
(01:55:18 PM) tyhicks: darix: this is a decent placeholder bug to follow for the general overlayfs issue: https://bugs.launchpad.net/apparmor/+bug/1408106
(01:56:05 PM) jjohansen: once the overlayfs issues are fixed we should be good with doing unioning via overlayfs
(01:56:57 PM) intrigeri: be sure that I'll (have to) test it, including with multiple lower-layers
(01:56:57 PM) jjohansen: the over end is better alias support which would be needed for aufs, but at the moment finishing up that work is not on the schedule
(01:57:26 PM) jjohansen: intrigeri: yeah we will have to test it too, there are several projects that want to use it
(01:58:00 PM) intrigeri: jjohansen: fyi, as far as Tails is concerned, we won't need improved support for aufs as soon as overlayfs+AA work fine for us.
(01:58:00 PM) darix: coreos just announced it yesterday if i recall correctly
(01:58:29 PM) jjohansen: intrigeri: ack, thanks
(01:58:34 PM) intrigeri: darix: "it" == ?
(01:58:42 PM) darix: overlayfs
(01:59:02 PM) darix: and they drop btrfs in favor of ext4
(01:59:07 PM) darix: well ext4+overlayfs
(02:00:33 PM) darix: oh wow ... that was in january. and it just made it into the dead tree copy of the german linux magazine this month
(02:01:08 PM) jjohansen: all right lets move on, to darix's favorite Q. inet mediation
(02:02:03 PM) jjohansen: there has been work on the protype code, but it is not scheduled to be completed nor land until the current feature set lands upstream
(02:02:16 PM) jjohansen: so at least post 4.2
(02:02:21 PM) jjohansen: (sorry darix)
(02:03:06 PM) jjohansen: since I really don't have anything more around that, lets move on to 2.10
(02:03:18 PM) jjohansen: currently we have 2.10 scheduled to come out next month
(02:04:15 PM) jjohansen: however we need the systemd requirements to land in libapparmor so that is what will dictate whether we are late or not
(02:05:13 PM) jjohansen: currently the only other work I am aware of that needs to go into 2.10 is cboltz's cleanups to the the utils but it looks like most of that will be going into 2.9.x
(02:05:16 PM) jjohansen: as well
(02:06:09 PM) jjohansen: does anyone know of anything else that should be on the 2.10 schedule? Keeping in mind its release is just around the corner
(02:07:20 PM) tyhicks: I know of one thing
(02:07:27 PM) jjohansen: tyhicks: go
(02:07:39 PM) tyhicks: it'd be nice if we exposed a libapparmor function to split up an apparmor context into a label and a mode
(02:08:16 PM) jjohansen: yes, okay. That should be easy to do
(02:08:36 PM) tyhicks: applications that want to use the dbus-daemon's GetConnectionCredentials method to retrieve a connection's context will need to use that libapparmor function
(02:08:53 PM) jjohansen: right
(02:09:05 PM) cboltz: is there a chance to get https://bugs.launchpad.net/apparmor/+bug/888077 fixed?
(02:10:10 PM) jjohansen: cboltz: no, that turns out to be the rather large chunk of work needed to support aufs as well
(02:10:44 PM) jjohansen: I have done a fair bit of work around this and have experimental patches, but I just don't have time to finish it
(02:10:49 PM) jjohansen: at this time
(02:10:55 PM) cboltz: ah, ok - I just wondered about how long that "mostly done" would take ;-)
(02:12:13 PM) jjohansen: cboltz: well its gone through a few iterations, one of my big hangups right now is getting some more infrastructure in place to test the generated dfas, since the code stamps all over dfa generation and could break all kinds of things in subtle ways
(02:12:15 PM) intrigeri: I think we're applying one of John's experimental patches in Tails.
(02:12:50 PM) jjohansen: well some of the earlier stuff
(02:12:52 PM) tyhicks: I've filed https://launchpad.net/bugs/1430532 for the libapparmor function needed to split up a context
(02:12:54 PM) cboltz: no problem - given the age of the bug, another release doesn't really hurt
(02:13:22 PM) jjohansen: Alright lets move on
(02:13:27 PM) jjohansen: thanks tyhicks
(02:13:40 PM) jjohansen: Does anyone have anything else they would like to discuss
(02:15:07 PM) intrigeri: OPW: this round is done. It was great. MoC did awesome work. Her final report is http://apparmor.451f.org/2015/03/09/final-report/
(02:15:09 PM) darix: jjohansen: that is kernel version 4.2 right? not apparmor 4.2?
(02:15:46 PM) jjohansen: darix: correct
(02:16:11 PM) jjohansen: thank you MoC for all your work
(02:16:29 PM) sarnold: intrigeri: done already? it feels to me likke it just started :)
(02:16:29 PM) intrigeri: face-to-face AppArmor sprint about cross-distro collaboration: I'll come back to this idea, probably in April.
(02:16:40 PM) sarnold: MoC: thanks :)
(02:17:08 PM) jjohansen: hehe, intrigeri that would be nice
(02:17:08 PM) intrigeri: sarnold: yay, 3 months isn't that long. but MoC will stay around I'm sure.
(02:17:08 PM) darix: well i have one more spot that i run into regularly ... puppet module registry and tooling around it
(02:17:08 PM) MoC: thank you!
(02:17:08 PM) sbeattie: intrigeri: I concur, very happy with the work MoC has done.
(02:17:13 PM) MoC: i got some karma on launchpad now, hahah :))
(02:17:18 PM) darix: intrigeri: i am shy!
(02:17:42 PM) MoC: and I plan to stay around indeed
(02:17:43 PM) MoC: :)
(02:17:47 PM) tyhicks: good! :)
(02:17:50 PM) sbeattie: MoC: excellent!
(02:17:50 PM) sarnold: \o/
(02:18:01 PM) darix: jjohansen: e.g. i was looking for some google-chrome apparmor profile. but there was no real definitive source.
(02:18:01 PM) jjohansen: darix: you just need the proper bribes to come out of your shell
(02:18:23 PM) intrigeri: I've no other topic to add.
(02:18:27 PM) cboltz: intrigeri: for cross-distro - I'd propose debconf or the openSUSE conference (May 1-4)
(02:18:29 PM) sarnold: a kebap or two should prove sufficient.. :)
(02:19:11 PM) darix: jjohansen: so if ubuntu gets into gsoc. that would be a good topic to cover imho
(02:19:34 PM) intrigeri: cboltz: Yep, that's the minimalistic solution, but I'm afraid we won't have any Ubuntu*AppArmor person if we just do that.
(02:19:47 PM) jjohansen: darix: indeed
(02:20:07 PM) darix: something like the old app opensuse was hosting for so long^^
(02:20:11 PM) darix: just well better ;)
(02:20:54 PM) sarnold: I don't think I saw ubuntu or suse in the gsoc organizations this year
(02:21:14 PM) cboltz: intrigeri: I hope that some of the Ubuntu people will visit debconf, so it should work there IMHO
(02:21:43 PM) jjohansen: there will certainly be some ubuntu people at debconf, its just a matter of who
(02:21:48 PM) intrigeri: cboltz: You mean Ubuntu people who're working on AppArmor? Nobody said so on the ML in that thread.
(02:22:19 PM) cboltz: I said "I hope" ;-)
(02:22:24 PM) jjohansen: intrigeri: currently none of those working on apparmor are scheduled to attend
(02:22:41 PM) cboltz: jjohansen: you should change that ;-)
(02:22:45 PM) jjohansen: I'm not saying it won't happen, just its unlikely
(02:23:02 PM) intrigeri: anyway, I still have to process the replies to the thread I started late January about it. and to think a bit about fundraising, in case travel sponsorship would be enough to bring some Ubuntu people there.
(02:23:20 PM) jjohansen: cboltz: heh, well I'm not so sure my wife would agree :P
(02:23:27 PM) intrigeri: (/me would love to bribe Canonical employees to come work on cross-distro AppArmor stuff during their vacation)
(02:23:30 PM) darix: jjohansen: here is an idea
(02:23:36 PM) darix: jjohansen: bring her with you!
(02:24:35 PM) jjohansen: I can try but, I haven't successfully convinced her to travel with me yet
(02:24:50 PM) MoC: intrigeri: :))
(02:25:05 PM) intrigeri: got to go. talk to you later folks. /me going back to his afk vacation, briefly interrupted for this meeting.
(02:25:12 PM) MoC: heidelberg is supposed to be beautiful, jjohansen :)
(02:25:13 PM) MoC: cu intrigeri
(02:25:21 PM) intrigeri: cheers! take care.
(02:25:23 PM) MoC: thanks for coming !
(02:25:35 PM) jjohansen: MoC: I'll let her know that :)
(02:25:40 PM) sarnold: bye intrigeri 
(02:25:43 PM) jjohansen: thanks for coming everyone
(02:25:52 PM) sarnold: oh man it's in heidelberg??
(02:25:52 PM) MoC: bye *
(02:25:53 PM) darix: i wonder if i ever met jjohansen in person
(02:25:58 PM) darix: cya moc
(02:26:03 PM) MoC: cu *
(02:26:08 PM) sarnold: bye MoC :)
(02:26:13 PM) jjohansen: next meeting is tentatively scheduled for April 14 @20:00 utc
(02:26:31 PM) jjohansen: meeting adjourned
``` 
