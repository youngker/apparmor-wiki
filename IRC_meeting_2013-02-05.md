```
(12:00:47 PM) jjohansen: sarnold, sbeattie, jdstrand, mdeslaur, kees, cboltz: apparmor monthly meeting time if you are interested
(12:01:02 PM) jjohansen: agenda http://wiki.apparmor.net/index.php/MeetingAgenda
(12:01:03 PM) mdeslaur: hello
(12:01:07 PM) sarnold: woot
(12:01:15 PM) cboltz: I'm here
(12:01:21 PM) jdstrand: hi!
(12:01:23 PM) ***sbeattie o/
(12:01:43 PM) jjohansen: nice
(12:01:51 PM) jjohansen: alright lets get rolling
(12:02:03 PM) jjohansen: first up 2.8.1 has been released, thanks to sbeattie
(12:02:26 PM) jdstrand: \o/
(12:02:28 PM) jjohansen: cboltz: I assume that worked out okay for suse, as I didn't hear any complaints
(12:02:54 PM) cboltz: yes, 2.8.1 is in Factory (which will become 12.3 RC1 soon)
(12:03:00 PM) sbeattie: woot
(12:03:05 PM) cboltz: and is also released as update to 12.2
(12:03:46 PM) jjohansen: good alright, I think we can move on to 3.0
(12:03:51 PM) cboltz: (both with the mysql/mariadb path changes as additional patch)
(12:04:26 PM) jjohansen: oh, I guess those are candidates for 2.8.2 then :)
(12:04:51 PM) ***sbeattie may have a couple more candidates for an as yet hypothetical 2.8.2
(12:05:00 PM) cboltz: jjohansen: it's a tradition that the openSUSE packages _always_ have a post-release patch ;-)
(12:05:06 PM) sbeattie: but let's move on to the 3.0 discussion
(12:05:17 PM) cboltz: (at least since I'm maintaining them)
(12:06:45 PM) jjohansen: hehe, yes isn't that the tradition with every distro
(12:06:45 PM) jjohansen: anyways, so 3.0 is running behind (big surprise). And we have hit the point where we originally had wanted to move to beta, but gave ourselves up to a month to slip
(12:06:45 PM) jjohansen: we will use all of it, so I am proposing a beta target of march 4
(12:07:22 PM) jjohansen: with probably 1 more alpha in between, Maybe march 15, or perhaps the 18th
(12:07:34 PM) sarnold: february?
(12:07:36 PM) sbeattie: err, feb 15 or 18
(12:07:37 PM) sbeattie: ?
(12:07:40 PM) jjohansen: hehe yeah
(12:07:55 PM) jjohansen: sorry was looking at the March 1, in the agenda
(12:08:14 PM) jjohansen: 2 alphas would need to be feb 15, and March 1 or so
(12:08:27 PM) jjohansen: which doesn't really make sense with beta at march 4
(12:08:35 PM) sbeattie: agreed
(12:08:55 PM) jjohansen: and I really don't care to do an alpha once a week
(12:09:30 PM) jjohansen: currently I am debugging some issues on the alpha2 kernel and plan/hope to release it today
(12:09:44 PM) jjohansen: but at latest tomorrow
(12:10:22 PM) jjohansen: so with being behind, I currently don't see a way to fit any updates to genprof/logprof in
(12:10:51 PM) jjohansen: at least I don't have any time
(12:11:03 PM) sbeattie: let's get the kernel out and then we can see who can pick that up.
(12:11:11 PM) jjohansen: if some one wants to step up great
(12:11:34 PM) jjohansen: sbeattie: I expect to be working on kernel parser right up to beta
(12:12:17 PM) jjohansen: I am still planning on getting env vars, along with the base network labeling patch in
(12:12:37 PM) jjohansen: along with any dbus introspection, changes
(12:12:58 PM) jjohansen: but yes lets get alpha2 out
(12:13:35 PM) jjohansen: cboltz: unfortunately as things are going right now I don't know that I will find time to even push the alpha2 kernel into the build service
(12:14:08 PM) cboltz: do (or don't do) whatever you can ;-)
(12:14:21 PM) jjohansen: cboltz: is this something that is really important, or is waiting until beta okay
(12:14:51 PM) jjohansen: I do want to get this testing on suse
(12:15:17 PM) cboltz: at the moment most people are testing the upcoming 12.3, so some delay won't hurt
(12:16:30 PM) jjohansen: cboltz: okay, I am think beta at which point we will freeze features and turn to real testing and bug fixing
(12:16:30 PM) cboltz: if making a alpha2 kernel takes too much time for you, waiting until beta is ok IMHO
(12:17:35 PM) jjohansen: okay
(12:17:37 PM) cboltz: (my guess is that >90% of the apparmor users don't use more than the 2.7 features anyway ;-)
(12:18:55 PM) jjohansen: cboltz: sure, and most of the 3.0 features won't get used until 3.1/3.2, 3.0 is just laying the ground work
(12:19:03 PM) jjohansen: okay I don't have thing else
(12:19:24 PM) jjohansen: so /me opens it, anyone else have something they would like to discuss
(12:21:52 PM) jjohansen: alright I'll take that as nobody has anything else
(12:21:54 PM) cboltz: I have something that we postponed in one of the last meetings - but if someone else has something, we should do that first (mine lead to a long discussion last time ;-)
(12:22:07 PM) jjohansen: cboltz: shoot what is it
(12:22:18 PM) cboltz: the location for autogenerated sniplets ;-)
(12:22:30 PM) cboltz: we had some ideas, but no final result
(12:22:49 PM) jjohansen: cboltz: oh right, I believe I was supposed to take an action item to raise it on the mailing list
(12:23:03 PM) sbeattie: cboltz: if I were to do automated builds in osc based on bzr commits, would factory's specfile be the place to start from?
(12:23:16 PM) sbeattie: (for apparmor userspace, not the kernel)
(12:23:28 PM) cboltz: yes and no ;-)
(12:23:48 PM) cboltz: I packaged bzr trunk some weeks ago to a subproject of home:cboltz
(12:23:58 PM) cboltz: that would be the better start because it's more up to date
(12:24:14 PM) sbeattie: okay, cool, I'll look there. Thanks.
(12:24:40 PM) cboltz: I could even give you permissions on that project, or create a subproject of security:apparmor if we want to have it more official ;-)
(12:25:17 PM) cboltz: https://build.opensuse.org/package/show?package=apparmor&project=home%3Acboltz%3Abranches%3Ahome%3Acboltz
(12:25:17 PM) sbeattie: (I currently have a private jenkins instance building locally and uploading to a launchpad ppa, and want to extend it to the buildservice)
(12:26:06 PM) sbeattie: I don't think it should be terribly official; people should be clear that it's a build project for testing purposes only.
(12:26:08 PM) cboltz: (it's linked to the factory 2.8.1 package, and it looks like I have to fix a merge conflict in the spec ;-)
(12:26:31 PM) cboltz: that can be solved by calling it security:apparmor:testing ;-)
(12:27:22 PM) sbeattie: yeah :)
(12:28:38 PM) jjohansen: cboltz: are you okay with /me sending out an email to the mailing list to discuss the location of autogenerated snippets, I promise to get it out today, after the meeting even before I get back to debugging
(12:29:06 PM) cboltz: yes, sounds good
(12:29:26 PM) cboltz: we'll see if we find a perfect location or if we have to vote ;-)
(12:29:56 PM) ***sbeattie +1's deferring that discussion to the mail list
(12:30:21 PM) jjohansen: alright with that I propose that the next meeting be in 1 month March 5, 20:00 UTC
(12:30:36 PM) jdstrand: works for me
(12:30:55 PM) sarnold: jjohansen: feature freeze date is march 7 -- is that cause for concern with that date?
(12:31:21 PM) jjohansen: sarnold: hrmm, no I don't think so
(12:31:26 PM) sarnold: okay :)
(12:32:04 PM) sbeattie: Does it make sense to have the next meeting after the proposed beta date?
(12:33:36 PM) jjohansen: sbeattie: are we going to change anything about the beta because of it? We need to have something going in before the 7th, we could move the meeting forward a day and push the beta back, but ...
(12:33:37 PM) cboltz: depends if we want to discuss late features or the first bugs ;-)
(12:33:47 PM) jjohansen: hehe, exactly
(12:34:08 PM) sarnold: :)
(12:35:28 PM) sbeattie: well, it was more of, do we want to re-asses if it looks like things are going to slip for the beta.
(12:35:30 PM) jjohansen: sbeattie: looking at the calendar, what date would you propose
(12:35:35 PM) sbeattie: I dunno
(12:36:22 PM) sbeattie: just trying to figure out where/when we would discuss contingencies, if any.
(12:36:25 PM) jjohansen: Friday, Feb 31, Monday 4 or are people willing to commit to a weekend meeting?
(12:36:58 PM) sarnold: yes, feb 31 :)
(12:37:09 PM) sbeattie: lol
(12:37:30 PM) cboltz: yes, good date - you can be sure nothing else gets in your way ;-)
(12:37:30 PM) sbeattie: that's the date of the special meeting just for me, right? :)
(12:37:49 PM) jjohansen: sigh, I meant the 1st :)
(12:38:17 PM) cboltz: sbeattie: we'll do a special meeting for you at april 1st ;-)
(12:39:29 PM) sarnold: march 1 seems like a good idea to me...
(12:40:15 PM) jjohansen: Alright,
(12:40:15 PM) jjohansen: who votes for friday march 1, 20:00 UCT
(12:40:51 PM) cboltz: sounds good
(12:41:19 PM) sbeattie: I'm okay with that. That said, if everyone else thinks the next week makes more sense, that's cool, too.
(12:41:49 PM) ***sbeattie notes that tyhicks will escape that meeting, too.
(12:42:30 PM) ***jjohansen ponders whether thats a positive or not :)
(12:42:44 PM) sbeattie: for tyhicks, for sure. :)
(12:44:21 PM) jjohansen: alright, next meeting Friday March 1, 20:00 UCT
(12:44:28 PM) jjohansen: thanks everyone
(12:44:33 PM) jjohansen: meeting adjourned
(12:44:34 PM) sbeattie: jjohansen: thanks
(12:45:09 PM) jdstrand: thanks jjohansen 
(01:01:17 PM) cboltz: jjohansen: is there a special reason why you don't use headlines (== ... ==, one for each meeting) on the meeting page?
(01:02:16 PM) jjohansen: cboltz: nope, if you want to add them so I will remember to use them that would be great
(01:02:40 PM) cboltz: ok, will do that
(01:02:54 PM) cboltz: that should give us a clickable list with meeting dates
(01:09:33 PM) cboltz: done
(01:09:53 PM) jjohansen: thanks cboltz
```
