```
(01:02:15 PM) jjohansen: sarnold, sbeattie, tyhicks, mdeslaur, jdstrand: meeting time
(01:02:23 PM) tyhicks: hello
(01:02:24 PM) sarnold: o/
(01:02:30 PM) ***sbeattie waves
(01:02:49 PM) jjohansen: alright lets get started
(01:02:53 PM) mdeslaur: hello
(01:03:04 PM) jjohansen: so there are only a couple things on the agenda
(01:03:25 PM) jjohansen: 2.8.4 and the 2.9 release
(01:03:45 PM) jjohansen: Lets start with 2.9
(01:04:21 PM) jjohansen: so we have been fixing a lot of bugs, and also rolled some new features into dev
(01:04:31 PM) jjohansen: since the beta1 release
(01:05:05 PM) jjohansen: are we planning on pulling in everything (taking a new snapshot), or are we going to try and only pull over the bug fixes?
(01:05:31 PM) sbeattie: for 2.9? I was expecting we'd take everything.
(01:05:54 PM) jjohansen: okay, I wasn't sure as we actually dropped quite a few features in
(01:06:04 PM) jjohansen: I'm fine with taking everything
(01:06:11 PM) sbeattie: Well, I guess I hadn't really considered the alternative.
(01:06:18 PM) sarnold: I think for 2.9 we should take everything; as I understand it, ubuntu is the primary consumer of a 2.9 release for utopic, and that'll feature freeze in a few weeks; it'd be nice to get more of the features more exposure and testing
(01:06:53 PM) ***jdstrand agrees
(01:07:10 PM) ***tyhicks agrees
(01:07:22 PM) jdstrand: though, like sbeattie, I hadn't really considered otherwise. seems like busywork to not take everything
(01:07:27 PM) jjohansen: alright good enough for me, we will roll a new beta snapshot soon. I'm thinking in two weeks
(01:07:44 PM) sbeattie: Sure. I have a thing or two to land for it.
(01:08:07 PM) jjohansen: right, there are a few things I want to land as well
(01:08:32 PM) jjohansen: and that way it can line up with the rtm from Ubuntu
(01:09:27 PM) jjohansen: alright, moving on to 2.8.4
(01:10:27 PM) jjohansen: I think we aren't ready to release it yet, there are still some fixes needed. Particularly some bug reports around apache/mod_apparmor
(01:11:01 PM) sbeattie: Yeah, I have a fix for that (Bug 1322778) that I need to submit and land.
(01:11:26 PM) jjohansen: I'd like us to focus on 2.9 first (I'd like the mod_apparmor fixes in there as well), and then we can cherry-pick and roll a 2.8.4 release a week or two later
(01:11:59 PM) jjohansen: any objections?
(01:12:14 PM) sarnold: sounds fair to me
(01:12:16 PM) ***jdstrand has no objections to focusing on 2.9 first :)
(01:12:19 PM) tyhicks: no objections here
(01:13:13 PM) jjohansen: okay thats good enough
(01:13:22 PM) sbeattie: no, sounds reasonable. My only concern is 2.8.4 getting lost, falling off the back burner of the stove, since we've been meaning to do it for a while.
(01:13:44 PM) sbeattie: (no objection, that is)
(01:13:47 PM) jdstrand: I don't think that will happen
(01:14:05 PM) jdstrand: those fixes we will want to push into 14.04 as SRUs presumably
(01:14:12 PM) jjohansen: nor do I, and I think it needs some of the fixes we haven't landed yet
(01:14:25 PM) sbeattie: Okay.
(01:14:29 PM) jdstrand: so we will be reminded of them. we can push those (and 2.8.4) to some point later in this cycle
(01:14:44 PM) jjohansen: ugh the 14.04 SRU is different than either
(01:14:51 PM) jdstrand: yes
(01:14:57 PM) jjohansen: so back to 2.9
(01:15:01 PM) jdstrand: I wasn't trying to conflate things
(01:15:09 PM) jdstrand: the first sru is for the python tools stuff
(01:15:17 PM) jdstrand: I was more talking about a 2nd
(01:15:22 PM) sarnold: since we've been Good about our point releases it may be worth asking for a MRE to let us just push new tarballs into packages.
(01:15:49 PM) jjohansen: 14.04 is based on the 2.9 snapshot. I believe the plan is to only backport fixes for the SRU, which is what we decided not to do for the next 2.9 beta
(01:15:55 PM) jdstrand: except Ubuntu never really has upstream tarballs
(01:16:04 PM) jdstrand: of apparmor. we add a bunch of patches
(01:16:09 PM) jjohansen: sure
(01:16:34 PM) jdstrand: maybe some day...
(01:16:34 PM) jjohansen: I'm just noting that the extra work of backporting fixes has to be done anyways
(01:16:36 PM) jdstrand: :)
(01:16:49 PM) ***jdstrand nods
(01:17:04 PM) mdeslaur: who is going to use 2.8.4?
(01:17:09 PM) jjohansen: I just wanted to make sure with that fact clear, that no one changes their mind about 2.9
(01:17:21 PM) sarnold: oh, hrm, I see we didn't ship 2.8.x anywhere ourselves. scratch the MRE idea, it wouldn't save us much.
(01:17:22 PM) jjohansen: mdeslaur: anyone who is still using 2.8
(01:17:36 PM) mdeslaur: jjohansen: and who is that?
(01:17:46 PM) sbeattie: mdeslaur: debian, I think.
(01:17:48 PM) jjohansen: sarnold: yes we did, we just called it 2.7...
(01:17:49 PM) mdeslaur: if a release falls in the forest, no nobody is around...
(01:17:50 PM) sarnold: likely suse is still on 2.8
(01:18:26 PM) jjohansen: debian, suse, anyone on upstream who hasn't gone to 2.9 yet because we haven't release an official 2.9 yet
(01:18:45 PM) sarnold: slackers
(01:18:59 PM) jdstrand: 2.9 is where all the fun is
(01:19:07 PM) jjohansen: so 2.8.4 will be the basis for updates to existing support releases
(01:19:27 PM) jjohansen: I expect debian, suse etc will pick up 2.9 once we release it
(01:19:32 PM) mdeslaur: debian won't push 2.8.4 to jessie, and they can get 2.9 into sid...
(01:19:48 PM) mdeslaur: I mean, if someone wants it, that's one thing
(01:20:17 PM) jjohansen: yeah we need to update 2.8.3 as the current stable, 2.9 won't land for a bit yet
(01:20:39 PM) jjohansen: apache/mod_apparmor seems to be pretty broken in 2.8.3
(01:21:46 PM) jjohansen: my preference would actually be to prioritize 2.8.4 over 2.9, but the reality is we need to get the next 2.9 snapshot out asap
(01:22:05 PM) jjohansen: okay I think we are in agreement and we can move on
(01:22:20 PM) jjohansen: I am going to add another little section
(01:22:28 PM) jjohansen: upstreaming effort
(01:23:33 PM) jjohansen: so I have reworked the query interface patch a bit, to drop the bits that cause dbus to freak out, and will be pushing it upstream this next kernel cycle, along with any other bits, that are directly tied to the labeling rework
(01:23:33 PM) tyhicks: quick! send everything upstream while hallyn is temporarily maintaing the security tree!
(01:23:42 PM) jjohansen: hehe :)
(01:24:03 PM) sarnold: nice
(01:24:19 PM) jjohansen: I don't think the labeling rework is quite ready, I need to cleanup and fold back the 100+ patch queue
(01:24:40 PM) sarnold: yes please :)
(01:25:00 PM) jjohansen: this won't be sufficient for the dbus apparmor patchset, as it requires the labeling work for the socket label
(01:25:19 PM) jjohansen: sarnold: oh come on you know you want to review my fixes to fixes ;)
(01:25:43 PM) jjohansen: speaking of which, tyhicks how is the dbus apparmor upstreaming
(01:26:12 PM) tyhicks: jjohansen: it is waiting on upstream review
(01:26:22 PM) jjohansen: alright thanks
(01:26:23 PM) tyhicks: jjohansen: I've received 2 or 3 reviews and revised the patch set accordingly
(01:26:39 PM) tyhicks: jjohansen: but simon is not focused on dbus right now, so he hasn't had time to look at the latest patch set
(01:27:00 PM) tyhicks: jjohansen: it has been about 1.5 months since I posted it, so I was planning on sending him a friendly poke
(01:27:31 PM) tyhicks: (I completely understand simon's situation)
(01:28:15 PM) jjohansen: yeah, its all too familiar :/
(01:28:15 PM) jjohansen: alright, does anyone have anything else?
(01:29:00 PM) jjohansen: sounds like a no to me
(01:29:29 PM) ***jdstrand doesn't have anything
(01:30:10 PM) tyhicks: nothing here
(01:30:19 PM) jjohansen: any objections to doing the July meeting, usual time 2nd Tuesday so that would be the 8th,
(01:30:59 PM) sarnold: that's a month away, let future seth worry about that
(01:32:16 PM) mdeslaur: heh
(01:32:16 PM) jjohansen: well we can change if needed, but I'll set it to that date tentatively
(01:32:18 PM) sbeattie: yeah, that's fine
(01:32:20 PM) jjohansen: thanks everyone
(01:32:23 PM) mdeslaur: thanks!
(01:32:24 PM) jjohansen: meeting adjourned
(01:32:26 PM) sbeattie: thanks, jjohansen 
(01:33:43 PM) tyhicks: thanks
``` 
