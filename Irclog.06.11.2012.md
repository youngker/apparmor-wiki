```
(12:00:47 PM) jjohansen: sbeattie, sarnold, jdstrand, tyhicks, mdeslaur: meeting time
(12:00:53 PM) ***sbeattie waves
(12:00:58 PM) tyhicks: hello
(12:01:19 PM) jdstrand_: hey
(12:01:22 PM) sarnold: woo
(12:01:43 PM) mdeslaur: oh, wow, meeting!
(12:02:45 PM) jjohansen: mdeslaur: sorry I promise it won't be as exciting as last weeks meetings ;)
(12:03:15 PM) jdstrand left the room (quit: Ping timeout: 480 seconds).
(12:03:16 PM) cboltz: don't promise something you can't deliver ;-)
(12:03:23 PM) mdeslaur: jjohansen: Adventure? Excitement? A Jedi needs not these things.
(12:03:28 PM) jjohansen: hehe
(12:03:59 PM) jjohansen: Alright so if we stick to the schedule the next meeting will be on Tue Dec 4 @ 20:00 UTC
(12:04:17 PM) jjohansen: that seem alright?
(12:04:31 PM) sbeattie: seems fine by me.
(12:04:49 PM) jdstrand_: sure
(12:04:54 PM) jdstrand_ is now known as jdstrand
(12:05:22 PM) jjohansen: alright if some one has problems with it shout
(12:05:22 PM) jjohansen: moving on
(12:05:25 PM) cboltz: looks good (a reminder on the ML would be nice ;-)
(12:06:11 PM) jjohansen: cboltz: yeah, I should have sent it out yesterday but I was only reminded this morning when my calendar popped up with a notice of the kernel team meeting
(12:06:35 PM) jjohansen: I'll make sure to send out a notification to myself a day in advance for the next one
(12:06:47 PM) cboltz: ;-)
(12:06:48 PM) jjohansen: So schedule, we are slipping already
(12:07:35 PM) jjohansen: We can either do
(12:07:35 PM) jjohansen: 1. roll alpha 1 this week with 2.8 + a couple of patches + dbus prototype
(12:07:35 PM) jjohansen: 2. roll alpha 1 in a couple of weeks with, basic parser C++ conversion, kernel lock rework, aafs profile directories, implicit labeling and stacking
(12:08:00 PM) ***jjohansen prefers 2
(12:08:21 PM) jdstrand: '2' seems fine
(12:08:23 PM) cboltz: that sounds like you have plans for alpha 1 and alpha 2 ;-)
(12:08:29 PM) sbeattie: I'd rather see us spend time on getting to 2. Is there any value in '1'?
(12:08:49 PM) sbeattie: or rather, is the value of doing '1' worth the time spent on it?
(12:09:09 PM) jjohansen: I can't see any other than it might get more people playing with dbus
(12:09:34 PM) cboltz: depends on the time needed to do the release ;-)
(12:09:48 PM) jjohansen: also if we do 1, I wouldn't expect to roll the next alpha until december some time
(12:10:19 PM) tyhicks: The dbus patches aren't really new at this point, so anyone interested could have probably already been playing with it
(12:10:28 PM) tyhicks: I vote for 2
(12:10:43 PM) jjohansen: cboltz: true its not much, but still there isn't much different from 2.8
(12:11:14 PM) jjohansen: alright that is enough votes to go with option 2
(12:11:25 PM) sarnold: I'm a bit concerned about the scope of 2. I like the enthusiasm but between kernel lock reworking and parser C++ fiddling and so forth, it feels like a lot that could through wrenches
(12:11:53 PM) sarnold: s/through/throw/
(12:12:41 PM) sbeattie: sarnold: yes, there's enough there, that I expect it will slip to december anyway.
(12:12:48 PM) jjohansen: sarnold: so, the lock rework and aafs stuff has already been floated, its had some changes and is getting another slight revision as we speak, the C++ stuff is actually mostly done as well I just need to take the time to tweak it
(12:13:17 PM) sbeattie: jjohansen: as I said at UDS, I'm happy to poke at bugs in the C++ stuff
(12:13:26 PM) jjohansen: sarnold: all of that could go this week, it just hasn't been my focus as I am trying to get the stacking work done
(12:13:56 PM) sarnold: jjohansen: okay. I'm just worried about mostly-done work potentially stalling behind other mostly-done work..
(12:14:02 PM) jjohansen: sbeattie: right I need to put it up somewhere, I fixed the policy_list crashes, on the plane
(12:14:14 PM) jjohansen: sarnold: heh understood
(12:15:30 PM) jjohansen: sarnold: basically I just don't think we are at a point to bother with alpha1 atm, but there is a whole bunch of work that should land soon (was trying for before UDS) and that makes a nice point for alpha1
(12:15:48 PM) sarnold: jjohansen: that's a good point, there ought to be -some- changes for an alpha. :)
(12:15:52 PM) ***sbeattie concurs.
(12:15:52 PM) jjohansen: sarnold: that by no means includes all of the outstanding work
(12:16:35 PM) sarnold: okay :) my concerns are assuaged. thanks.
(12:17:06 PM) jjohansen: I also think this means we should target the beta a little more firmly to the start of february
(12:17:40 PM) jjohansen: but we can discuss that more after alpha1/alpha2
(12:18:03 PM) jjohansen: So if there is nothing further I am going to move on to the UDS recap
(12:18:06 PM) cboltz: what would that (roughly) mean for the final release date?
(12:18:33 PM) cboltz: (openSUSE 12.3 is planned for march ;-)
(12:18:35 PM) jjohansen: cboltz: I think we would like the final release in march time frame
(12:18:43 PM) jjohansen: cboltz: oh hrmm
(12:19:20 PM) jjohansen: cboltz: what kind of time frame for a freeze are you looking for to get something into 12.3?
(12:19:54 PM) cboltz: that's a good question ;-)
(12:20:32 PM) cboltz: but it's not a real problem to ship 12.3 with a late beta
(12:20:38 PM) cboltz: and then deliver the final release as online update
(12:20:43 PM) cboltz: (been there, done that ;-)
(12:21:29 PM) cboltz: so if "beta in early february" works, it should also work on the openSUSE side
(12:21:42 PM) cboltz: if not, well, then I'll just stick with 2.8
(12:22:16 PM) jjohansen: cboltz: okay, can you look into timing more closely. We definitely want to freeze by february if we can
(12:22:45 PM) cboltz: ok
(12:22:48 PM) jjohansen: That gives us just under 3 months to get the majority of this done
(12:23:11 PM) jjohansen: cboltz: also the kernel will not have all the patches go up in this time frame
(12:23:45 PM) cboltz: we'll see where we are when the openSUSE feature freeze comes closer ;-)
(12:24:19 PM) jjohansen: cboltz: ubuntu will have to carry some out of tree patches, suse is welcome to as well, but with the kernel cadence there is no way we can freeze in late jan/ start of feb and have our patches in the upstream kernel
(12:24:32 PM) jjohansen: cboltz: ack
(12:24:37 PM) jjohansen: okay moving on
(12:25:06 PM) jjohansen: Basically UDS covered
(12:25:06 PM) jjohansen: Continue with core rework (stacking, labeling, extended permissions)
(12:25:06 PM) jjohansen: Continue work on env var filtering
(12:25:06 PM) jjohansen: Improve the userspace library and API to add the ability to do permission queries and cache them
(12:25:06 PM) jjohansen: Work on extending apparmor to better support the desktop and sandboxed applications using trusted userspace apps
(12:25:06 PM) jjohansen: DBus
(12:25:06 PM) jjohansen: gsettings
(12:25:06 PM) jjohansen: file picker
(12:25:06 PM) jjohansen: Display server
(12:25:53 PM) jjohansen: that is basically what we will be working on for this and even the next cycle, any questions?
(12:26:20 PM) jdstrand: sounds exciting :)
(12:26:51 PM) jjohansen: hehe
(12:27:11 PM) ***jjohansen is afraid it will be all too exciting :)
(12:27:17 PM) jdstrand: heh
(12:27:49 PM) jjohansen: cboltz: did you manage to catch/listen after the fact to most of the sessions
(12:28:18 PM) cboltz: I didn't do the recordings just to fill my harddisk ;-)
(12:28:18 PM) jjohansen: any input on these from the suse side of things?
(12:28:40 PM) jjohansen: cboltz: of course not, you did them to show me up :)
(12:29:10 PM) cboltz: damm, you weren't supposed to find out _that_...
(12:30:08 PM) cboltz: about the plans: it sounds exciting, and I'm sure I will come up with questions 
(12:30:24 PM) cboltz: but for now it looks good to me
(12:30:45 PM) cboltz: maybe one question
(12:30:54 PM) cboltz: you mostly talked about gtk/Gnome
(12:31:09 PM) cboltz: will there be a filepicker for KDE also?
(12:31:39 PM) mdeslaur: cboltz: plan is for it to be a separate library, toolkit independant
(12:32:15 PM) mdeslaur: cboltz: I believe it will be a gtk filepicker for now, but nothing says a kde one can't be an option
(12:32:17 PM) jjohansen: cboltz: so for this cycle the target is to just get some bases.  Build the apparmor api etc. I don't know what actual file pickers and stuff will be had long term
(12:33:05 PM) cboltz: the library is a good start
(12:33:21 PM) cboltz: we'll see what happens on the KDE side when it is ready ;-)
(12:33:58 PM) jjohansen: :)
(12:34:21 PM) jjohansen: alright, so we had a few discussion last meeting that didn't finish
(12:34:40 PM) jjohansen: should we continue them, or postpone them for a later meeting
(12:35:24 PM) cboltz: I'd say continue them, but do the "location for auto-generated sniplets" at the end of the meeting
(12:35:34 PM) cboltz: we already know it might become a longer discussion ;-)
(12:36:31 PM) jjohansen: well I only have 2
(12:36:31 PM) jjohansen: - snipets and
(12:36:31 PM) jjohansen: - how magical should aa-enable be? (should it look into the extras/ dir?)
(12:37:38 PM) sbeattie: for the latter, if we make changes to aa-enable, do we have someone who's volunteering to take that on?
(12:37:46 PM) jjohansen: so my take on the extras/ is it depends. Is extras reference profiles or profiles that likely need further editing
(12:37:46 PM) jjohansen: or do we split extras into disabled by default, and examples
(12:38:14 PM) jjohansen: sbeattie: sure be practical
(12:38:41 PM) cboltz: I'm afraid all profiles in extras/ need further editing ;-)
(12:38:51 PM) jjohansen: whether or not we have some one we can decide what we are going to do
(12:39:04 PM) cboltz: IIRC they didn't receive any update in the last years...
(12:39:20 PM) jjohansen: right so, in that case I would say no, the place to grab those is the tool like genprof and logprof
(12:39:45 PM) jjohansen: of course those tools aren't getting updated either :/
(12:39:52 PM) sarnold: or a new tool altogether
(12:39:56 PM) cboltz: my proposal for aa-enable is:
(12:40:07 PM) sarnold: aa-shop-for-profile (to steal from our juju friends :)
(12:40:15 PM) cboltz: if a profile in extras/ exists, print an info message
(12:40:39 PM) cboltz: ("profile not found, but you might want to use the profile in extras/")
(12:40:58 PM) jjohansen: seems reasonable
(12:40:59 PM) cboltz: but still fail because the profile doesn't exist in /etc/apparmor.d
(12:41:35 PM) cboltz: sarnold: you mean something like apparmor.opensuse.org + a fresh design? ;-)
(12:41:56 PM) sarnold: cboltz: nice end goal...
(12:42:31 PM) cboltz: ;-)
(12:43:57 PM) jjohansen: any one else with input on cboltz's suggestion, if not /me will add it to the roadmap
(12:44:20 PM) jdstrand: seems fine
(12:44:43 PM) sbeattie: +1 from me
(12:45:02 PM) jjohansen: good enough, moving on
(12:45:17 PM) cboltz: BTW: I'm just reading aa-disable
(12:45:17 PM) jjohansen: do we want a special location for auto-generated sniplets? (for example the samba on openSUSE, which currently lives in local/)
(12:45:17 PM) jjohansen: what to call it
(12:45:17 PM) jjohansen: policy-extensions.d
(12:45:17 PM) jjohansen: snippets/autogenerated
(12:45:17 PM) jjohansen: snippets/dhcpd.d
(12:45:17 PM) jjohansen: snippets/foo.d
(12:45:17 PM) jjohansen: snippets/autogenerated/smbd.d
(12:45:31 PM) cboltz: does it "just" create the symlink
(12:45:36 PM) cboltz: or does it also unload the profile?
(12:46:14 PM) jjohansen: cboltz: hrmmm, I would hope it unloads the profile but I haven't looked at that in a long time
(12:46:14 PM) cboltz: (I didn't see an "unload" part, but I only had a quick look at it)
(12:46:15 PM) jdstrand: cboltz: it should also unload it
(12:47:06 PM) cboltz: ok - I'll check and open a bugreport if it doesn't ;-)
(12:47:15 PM) jdstrand: my $cmd_info = qx(cat $filename | $parser -I$profiledir -R 2>&1 1>/dev/null);
(12:48:02 PM) cboltz: looks good :-)
(12:48:49 PM) sarnold: .. except for the needless abuse of cat.
(12:49:18 PM) cboltz: ;-)
(12:49:27 PM) jjohansen: hrmm, yes why are we catting it into the parser
(12:49:32 PM) mdeslaur: sarnold: oh, you're one of those? :)
(12:49:49 PM) sarnold: mdeslaur: I guess I'm out of the closet now..
(12:49:56 PM) mdeslaur: hehe
(12:50:05 PM) sbeattie: the cat's out of the bag, as it were. 
(12:51:12 PM) cboltz: I have a question that came up at the openSUSE conference: handling of profiles that come with some rpm package
(12:51:23 PM) ***jjohansen adds item to cleanup use of cat etc in the init scripts, and just pass the files directly to the parser
(12:51:24 PM) jdstrand: I'm guessing that is old code cargo-culted in via copy and waste
(12:51:25 PM) sarnold: (and cripes I hope that $filename doesn't do something stupid on silly filenames... cboltz, you've still got that tarball of bad filenames, right? :)
(12:51:37 PM) cboltz: does Ubuntu have a %post script that loads profiles when they are installed in /etc/apparmor.d/ 
(12:51:51 PM) cboltz: (if yes, I'd like to steal this script ;-)
(12:52:08 PM) jjohansen: cboltz: sort of, it has a deb helper dh_apparmor
(12:52:21 PM) jdstrand: cboltz: we have a debhelper hook, yes. it is in the dh-apparmor package
(12:52:40 PM) cboltz: do you have a link to it?
(12:52:55 PM) cboltz: or would it make sense to move the logic to a aa-install-profile tool?
(12:53:06 PM) mdeslaur: cboltz: http://paste.ubuntu.com/1338325/
(12:53:55 PM) jjohansen: cboltz: hrmmm, I think we are always going to want some logic in a packaging specific hook, but generalizing it as much as possible isn't a bad idea
(12:54:30 PM) jjohansen: of course looking at dh-apparmor ...
 (12:56:48 PM) sarnold: .. shouldn't there be some quotes around $LOCAL_APP_PROFILE here? ... mkdir dirname $LOCAL_APP_PROFILE ... 
(12:57:38 PM) cboltz: using quotes results in never becoming famous ;-)
(12:58:25 PM) sarnold: that's on your slide deck somewhere, isn't it? :)
(12:58:35 PM) cboltz: yes ;-)
(12:58:35 PM) mdeslaur: yeah...then again, if an attacker can control that, you have more things to worry about :P
(12:58:38 PM) jjohansen: where is the notority in that?
(12:58:43 PM) sarnold: mdeslaur: very true.
(12:58:52 PM) cboltz: you remember the (in)famous deletion of /usr by bumblebee?
(12:59:11 PM) jjohansen: mdeslaur: not so worried about an attacker as a poor dev having to debug that
(01:00:00 PM) mdeslaur: true
(01:00:11 PM) sarnold: is install not available to dh scripts? (and does e.g. install -m 644 avoid race or does it set the modes after the copy?)
(01:00:36 PM) cboltz: BTW: I'd also put quotes around the LOCAL_APP_PROFILE= line - you never know what #PROFILE# will be
(01:01:01 PM) mdeslaur: wow, this turned into an audit session :)
(01:01:04 PM) mdeslaur: darn security guys :)
(01:01:27 PM) jdstrand: well, it should be quoted, but this is called via a debian/rules entry. the packager is in control of that, not an attacker. if a packager is naming the profile 'foo; rm -rf /' then, well, sheesh
(01:01:51 PM) sarnold: jdstrand: but "music helper.exe" is a real possibility :/
(01:02:02 PM) jdstrand: there are a lot easier ways for a packager to hose your system via a maintainer script
(01:02:31 PM) cboltz: BTW: I also found a race condition ;-)
(01:02:53 PM) cboltz: test -e "$LOCAL_APP_PROFILE"  ....... mv -f "$tmp" "$LOCAL_APP_PROFILE"
(01:03:49 PM) jdstrand: sarnold: it is very uncommon for a file in debian to have a space in the name. the dirname would complain anyway
(01:04:14 PM) sarnold: jdstrand: oh?
(01:04:34 PM) cboltz: oh, that's why it comes with a     2>/dev/null   || true     ? ;-)
 (01:04:46 PM) jdstrand: dirname /etc/apparmor.d/local/music helper.exe 
 (01:04:56 PM) jdstrand: dirname: extra operand helper.exe' 
(01:05:24 PM) jdstrand: cboltz: it is customary to always use || true on things that can fail in a maintainer script. otherwise the upgrade halts
(01:05:34 PM) mdeslaur: there are all deliberately left in so we can jailbreak our devices once secure boot is widespread
(01:05:43 PM) mdeslaur: s/there/these/
(01:05:52 PM) sarnold: mdeslaur: target rich environments don't make themselves...
(01:06:00 PM) cboltz: I get a different result from dirname: two lines:
(01:06:02 PM) jdstrand: as for the race condition-- it is safe. if someone jams something in between the test -e and the mv -f, the mv -f overwrites it
(01:06:08 PM) cboltz: .
(01:06:11 PM) cboltz:  /etc/apparmor.d/local
(01:06:21 PM) jdstrand: anyway
(01:06:48 PM) jjohansen: right, lets get back on track, so does have anyone have anything add to snippets
(01:06:54 PM) jdstrand: this is fairly pointless. yes, it should be quoted, but it isn't a security issue and it is not something a dev would normally run into
(01:08:46 PM) jdstrand: (#PROFILE# is not under attacker control, unless you are installing a deb from an untrusted source, and then you just gave away your system anyway)
(01:09:46 PM) jdstrand: jjohansen: please proceed :)
(01:10:01 PM) jdstrand: assuming there isn't more to discuss on the dh script :P
(01:10:24 PM) sarnold: where are we with those? I think we're all agreed we need some place to put them and we just bikeshedded ourself to tears last time..
(01:10:35 PM) ***jdstrand apologizes if he seems snippy-- I have an errand I have to get to soonish
(01:10:57 PM) jjohansen: sarnold: yep
(01:11:49 PM) jjohansen: in fact we have been at this 70 minutes already so I am just going to table them to next meeting as I don't have time to spend another 20 minutes on the meeting today
(01:11:51 PM) sarnold: I'm a fan of autogenerated/ if they are truly autogenerated -- nothing like a clear warning...
(01:12:19 PM) mdeslaur: this is under /var somewhere, right?
(01:12:30 PM) jdstrand: I also like profile-extensions.d, even if it is a bit long when combined with autogenerated/
(01:13:22 PM) sbeattie: how about we take proposals to the list?
(01:13:37 PM) jjohansen: sounds good!
(01:13:37 PM) cboltz: mdeslaur: /var would be a bad idea IMHO - it might technically be correct, but OTOH I remember that Ubuntu doesn't even want the profile cache in /var because it might be mounted late
(01:13:56 PM) mdeslaur: cboltz: ah, right...ok
(01:14:50 PM) jjohansen: I will send a mail with the current suggestions to the list, and we can discuss at leisure there
(01:15:08 PM) jjohansen: anything else that is quick and not snippet related?
(01:15:09 PM) sbeattie: jjohansen: +1
(01:15:30 PM) cboltz: two short questions:
(01:15:38 PM) jjohansen: cboltz: go
(01:15:49 PM) cboltz: is running aa-exec without any parameters expected to do nothing?
(01:16:07 PM) cboltz: I'd expect some usage output, "missing parameters" or something like that
(01:17:35 PM) jjohansen: hrmmm, I am indifferent on that one. I modeled it on another tool, but I can't remember which now
(01:18:42 PM) jjohansen: cboltz: I know you asked it before to the list, I would say patches welcome :)
(01:19:01 PM) jjohansen: cboltz: what is your other question?
(01:19:10 PM) cboltz: if you tell me what is the expected behaviour, I'll look at it
(01:19:31 PM) cboltz: maybe "<prog>" is NOT optional?
(01:20:00 PM) jjohansen: cboltz: sure I am fine with that
(01:20:10 PM) cboltz: ok
(01:20:16 PM) cboltz: so, second question
(01:20:37 PM) cboltz: some months ago, I proposed a profile for samba winbindd, but got no real feedback
(01:20:41 PM) cboltz: https://lists.ubuntu.com/archives/apparmor/2012-May/002752.html
(01:21:05 PM) cboltz: this profile is active by default in openSUSE since some months, and I did not get any complaints ;-)
(01:21:54 PM) jjohansen: hrmm, samba is not really my thing so I can't really say.
(01:22:40 PM) jjohansen: But if we are talking about the commit rules, I think its well past the couple of weeks with out a response so the ack isn't needed ;-)
(01:22:56 PM) cboltz: *g*
(01:23:02 PM) sbeattie: heh
(01:23:05 PM) sarnold: my vague recollection of winbindd is that it does not do much. including it into raring now would make a certain amount of sense, get it fleshed out if we've got local changes...
(01:23:25 PM) sarnold: hehe
(01:23:27 PM) cboltz: the profile is easy to test - if starting and stopping winbindd works, it should be fine
(01:23:27 PM) jdstrand: oh, I missed that
(01:23:36 PM) jdstrand: sarnold: yeah, that is what I remember as well
(01:24:13 PM) sarnold: cboltz: it's the odder configs that worry me a bit -- would it need some extra access if used as a PDC or BDC or .. watch me walk right off the end of my samba knowledge :)
(01:25:06 PM) cboltz: As I said - I didn't receive any bugreports ;-)
(01:25:20 PM) jdstrand: in abstractions/winbind we have:
(01:25:26 PM) jdstrand: /var/{lib,run}/samba/winbindd_privileged/pipe rw,
(01:25:27 PM) cboltz: and it was also checked by Lars M?ller, so it should hopefully be fine
(01:25:29 PM) jjohansen: sarnold: well for odder configs wouldn't the user have to be tweaking the configs anyway
(01:25:36 PM) jdstrand: but the windbindd profile only has:
(01:25:41 PM) jdstrand: /var/lib/samba/winbindd_privileged/pipe w,
(01:25:58 PM) jdstrand: that might be an Ubuntu/Debianism
(01:26:23 PM) jdstrand: I'm fine with checking it in as is. we can adjust for and Ubuntu/Debianisms later
(01:26:49 PM) jdstrand: we also have:
(01:26:53 PM) jdstrand: /{,var/}run/samba/*.tdb rw,
(01:26:57 PM) jdstrand: in the samba abstraction
(01:27:40 PM) jdstrand: not sure how many (if any) of the tdb files winbind uses would end up in /var/run or /run...
(01:28:10 PM) jdstrand: anyhoo, point still stands. whenever Ubuntu wants to ship it, we can test and make those adjustments and send them to the list
(01:28:51 PM) cboltz: ok, so I'll commit it
(01:28:52 PM) sbeattie: yeah
(01:28:55 PM) jdstrand: cboltz: ACK from me if you do s/proc/@{PROC}/
(01:29:09 PM) cboltz: ok
(01:29:39 PM) jdstrand: cboltz: thanks for submitting that! (and sorry for the much delayed review)
(01:32:03 PM) jdstrand: fyi, I am also testing a fix for the quoting in the dh script
(01:32:37 PM) sarnold: thanks :)
(01:32:39 PM) mdeslaur: jdstrand: hehe...OCD wins again :)
(01:32:51 PM) jjohansen: alright meeting adjourned, thanks everyone
(01:32:51 PM) jjohansen: next meeting dec 4
(01:32:56 PM) mdeslaur: thanks jjohansen!
(01:32:59 PM) jdstrand: mdeslaur: heh
(01:32:59 PM) mdeslaur: thanks everyone
(01:32:59 PM) sarnold: woo. thanks jjohansen :)
(01:33:18 PM) jdstrand: thanks jjohansen!
(01:33:23 PM) cboltz: mdeslaur: what does OCD mean?
(01:33:41 PM) jjohansen: cboltz: obsessive compulsive disorder
(01:34:05 PM) cboltz: thx ;-)
(01:34:22 PM) sbeattie: thanks jjohansen 
(01:34:22 PM) sarnold: cboltz: see also http://imgur.com/a/9fJLP  :)
(01:34:41 PM) cboltz: jdstrand: can you post the fixed script to the mailinglist, please?
(01:34:57 PM) mdeslaur: sarnold: ARGH! :P
(01:35:01 PM) jjohansen: sarnold: arggh no don't
(01:35:41 PM) jdstrand: cboltz: well, it isn't in apparmor-- it is a Debian/Ubuntu specific debhelper snippet. maybe I am misunderstanding...
(01:35:43 PM) ***sarnold cackles evily
(01:35:54 PM) sarnold: jdstrand: cboltz wants to steal it for his own use in rpm pacakging
(01:36:01 PM) jdstrand: my favorite is the pizza
(01:36:12 PM) jdstrand: I see
(01:36:46 PM) jdstrand: actually, my real favorite is the bookcase. that is awesome :)
(01:37:49 PM) sarnold: heh, yeah, I thought it'd be fnu to have one like that. but the problem is, density... it's not nearly dense enough.
(01:38:04 PM) jdstrand: sarnold: you just do it on all your walls :)
(01:38:12 PM) sarnold: jdstrand: ENOTENOUGHWALLS
(01:38:34 PM) cboltz: or ETOOMANYBOOKS ;-)
(01:38:42 PM) jdstrand: indeed :)
(01:38:50 PM) sarnold: cboltz: heh, never thought of that possibility...
(02:06:01 PM) cboltz: hmm, interesting bzr problem...
(02:06:10 PM) cboltz: I commited the winbind profile
(02:06:39 PM) cboltz: but it's still listed in the "Added" section of bzr st
(02:06:55 PM) cboltz: and when trying to commit it again, I get
(02:07:04 PM) cboltz: Unable to obtain lock  held by cboltz@bazaar.launchpad.net on taotie (process #11265), acquired 25 minutes, 39 seconds ago.
(02:07:05 PM) cboltz: See "bzr help break-lock" for more.
(02:07:07 PM) cboltz: bzr: ERROR: Could not acquire lock "(remote lock)": bzr+ssh://bazaar.launchpad.net/%2Bbranch/apparmor/
(02:07:18 PM) cboltz: any recommendations?
(02:17:26 PM) jdstrand: cboltz: bzr break-lock bzr+ssh://bazaar.launchpad.net/%2Bbranch/apparmor/ should do it
(02:17:50 PM) cboltz: ok
(02:17:58 PM) cboltz: I just wanted to be sure not to break anything
(02:18:09 PM) cboltz: (well, except the lock obviously ;-)
(02:23:31 PM) cboltz: commit worked now :-)
(02:30:21 PM) jdstrand: cool
```
