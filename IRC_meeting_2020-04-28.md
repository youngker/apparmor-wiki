 ```
(11:01:44 AM) jjohansen1: cboltz: meeting time?
(11:02:03 AM) cboltz: yes
(11:04:18 AM) jjohansen1: sarnold, jdstrand, sbeattie, jmbl: meeting time
(11:04:26 AM) sarnold: thanks
(11:04:54 AM) jmbl: hi jjohansen1, here :-)
(11:05:09 AM) jmbl: hi cboltz sarnold 
(11:05:20 AM) sarnold: hey jmbl, cboltz :)
(11:05:33 AM) jjohansen1: hey jmbl
(11:05:34 AM) cboltz: hi everybody
(11:05:44 AM) jjohansen1: hi cboltz :)
(11:06:56 AM) jjohansen1: this is probably all we are going to get today, so lets get started
(11:07:19 AM) jjohansen1: first item on the agenda is release cadence
(11:09:05 AM) jjohansen1: I would like to make releases more regular, feature releases are hard to do that with (but I would like them to be more frequent) but we very much could do something with the stable releases
(11:09:39 AM) chrisccoulson [~chrisccou@95.172.237.253.ip.static.uno.uk.net] entered the room.
(11:09:40 AM) jjohansen1: so for feature release: I would like to see 3.0 and 3.1 drop this year
(11:10:12 AM) jjohansen1: will 3.x being smaller feature releases like the 2.x series were
(11:10:48 AM) jjohansen1: 3.1 would have all the things that were targeted to 3.0 but just aren't going to make it so we can get 3.0 out the door
(11:11:15 AM) jjohansen1: stable release wise: I would like to see a tailing off model
(11:12:02 AM) jjohansen1: where we try to get a point release out within 3 months of the feature release, and then move to about 6 months, and keep it at that
(11:12:49 AM) jjohansen1: unless, there is a need for an emergency fix, or there has been no updates in which case we can skip the release
(11:13:14 AM) sarnold: that sounds really good to me
(11:13:35 AM) sarnold: smaller more frequent updates would be less taxing individually, anyway
(11:14:03 AM) cboltz: given our "usual" post-release patch (there's always something one day later), I wonder if waiting 3 months is a good idea ;-))
(11:14:05 AM) jjohansen1: sure, though it does increase the number of releases we are maintaining
(11:14:44 AM) jjohansen1: cboltz: notice I didn't say wait 3 months, I said within 3 months :)
(11:15:06 AM) jjohansen1: I think a release the following day falls within that range
(11:15:12 AM) cboltz: ah, I overlooked this detail ;-)
(11:15:52 AM) cboltz: so basically you are saying that I may kick you if there isn't a point release within 3 / 6 months after fixes went in ;-)
(11:16:32 AM) jjohansen1: cboltz: yeah, when we release we should set an expected schedule for that release
(11:17:35 AM) cboltz: good idea
(11:19:29 AM) jjohansen1: okay, lets try running with that
(11:19:35 AM) jjohansen1: next topic
(11:19:46 AM) jjohansen1: bugreport links in manpages etc. - launchpad, gitlab, both?
(11:20:24 AM) jjohansen1: my stance has been, gitlab is preferred, but I don't care as long as someone points us to the bug
(11:20:45 AM) cboltz: right
(11:20:48 AM) jjohansen1: so if its convenient for it to be in bugzilla, or lp, or an email great
(11:20:56 AM) sarnold: both, I've seen "create an account" hinder bugreports before, and including both is more likely someone has an account at one of them
(11:21:01 AM) cboltz: currently / for historical reasons, we have only launchpad mentioned in the manpages - and that's why I brought this up
(11:21:05 AM) jjohansen1: I don't want there to be friction in reporting bugs
(11:21:41 AM) jjohansen1: ah, yes we can update that to prefer gitlab
(11:22:33 AM) jjohansen1: cboltz: that is something we could get done today
(11:23:02 AM) cboltz: indeed, it's a "boring" change, I just wanted to know how to change it
(11:23:33 AM) cboltz: so should the manpages say   please report bugs at $gitlab_link or $launchpad_link
(11:23:43 AM) jjohansen1: hrmm, I would honestly skip mentioning lp and bugzilla
(11:23:44 AM) cboltz: or should we make the gitlab preference more visible?
(11:24:00 AM) jjohansen1: file issue in gitlab or send an email to the mailing list
(11:24:26 AM) jjohansen1: the mailing list remains so that people don't have to create an account if they don't want to
(11:24:53 AM) cboltz: so you wouldn't even mention launchpad anymore? (no objections from me)
(11:24:53 AM) jjohansen1: cboltz: I am fine with making the gitlab preference more visible
(11:25:17 AM) jjohansen1: but yeah, we are working toward moving away from lp as an upstream
(11:25:46 AM) jjohansen1: ubuntu will still use it for distro bugs, but I don't think we as an upstream project need to
(11:26:22 AM) jjohansen1: its still going to be there for awhile, but I would rather not point people at it
(11:26:45 AM) cboltz: ok, then I'll change the manpages to gitlab and mailing list
(11:27:16 AM) jjohansen1: thanks
(11:27:34 AM) ihet [~oftc-webi@c-6b0d70d5.47-1-64736c15.bbcust.telenor.se] entered the room.
(11:28:03 AM) cboltz: next topic? ;-)
(11:28:38 AM) jjohansen1: yes
(11:28:40 AM) jjohansen1: switch local/ files to include if exists and no longer create them by default?
(11:28:49 AM) jjohansen1: I like it
(11:28:55 AM) jjohansen1: +1
(11:29:04 AM) cboltz: the funny thing is that I came up with that while reading a debian bug ;-)
(11:29:44 AM) jjohansen1: which just makes me like it all the more, lets wipe out an entire class of bug
(11:30:01 AM) sarnold: I was less sure about the "no longer create them by default"
(11:30:07 AM) sarnold: when they exist, they are discoverable
(11:30:17 AM) jjohansen1: true
(11:31:00 AM) jjohansen1: so this is certainly something that can be done in two phases
(11:31:20 AM) jjohansen1: first switch everything like this to include if exists
(11:31:21 AM) cboltz: see https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=956175 for the background why I propose "no longer create them by default"
(11:31:55 AM) jjohansen1: and then take a look at creating by default
(11:32:33 AM) jjohansen1: I do think it would be nice to have a tool to report, and maybe even create the defaults based off of what is in policy
(11:34:16 AM) jjohansen1: yeah, there are certainly cases where we shouldn't be creating them by default
(11:35:02 AM) jjohansen1: I don't know if that should be extended to all of the cases though
(11:35:40 AM) jjohansen1: so again, lets move in two phases
(11:35:53 AM) jjohansen1: maybe even 3
(11:36:10 AM) jjohansen1: switch to include if exists for all local includes
(11:36:46 AM) jjohansen1: 2. tooling to make these easily discoverable/auto create them based on policy
(11:37:22 AM) cboltz: 2a (and on my TODO list since a looong time) - let logprof (create and) write to the local file
(11:37:27 AM) jjohansen1: 3. look more at if some of these should be created by default, or whether we just stop creating all of them by default
(11:38:50 AM) cboltz: 3 is basically the question if we want to have a "clean" directory (with only files that are not empty), or if we want to push our users' noses into local files by creating them by default
(11:38:55 AM) jjohansen1: cboltz: right, I knew logprof can do that, I just want to make sure we have a dedicated tool
(11:39:06 AM) sdeziel: since "include if exists" is pretty explicit, I don't think the file actually need to be created empty for discoverability
(11:39:21 AM) cboltz: well, logprof _could_ do that - but it can't yet
(11:40:07 AM) jjohansen1: cboltz: oh, hmmm, I thought you had added that already, I am mistaken
(11:40:36 AM) cboltz: it's on my TODO list since a very long time
(11:41:09 AM) cboltz: but it's not too easy to implement, and there are other things that are probably more important
(11:41:16 AM) cboltz: (like nested childs)
(11:41:16 AM) jjohansen1: sdeziel: well, looking at file listing is easer than looking at or grepping profiles
(11:42:07 AM) jjohansen1: how much more so? I don't know
(11:42:09 AM) sdeziel: jjohansen1: I believe a sysadmin is likely to look at "dpkg -L $package" and anything that automatically create that file will miss it
(11:42:51 AM) cboltz: rpm supports %ghost files, not sure if dpkg has something similar
(11:43:08 AM) jjohansen1: sdeziel: meh, depends on the admin and from where they have come on approaching policy
(11:50:00 AM) jjohansen1: cboltz: not that I know of
(11:50:15 AM) jjohansen1: but I am no debian packaging guru either
(11:50:37 AM) jjohansen1: okay, lets work on getting parts one and two of this done
(11:51:06 AM) jjohansen1: and then we can revisit whether we want to stop creating these by default
(11:51:29 AM) jjohansen1: does anyone have anything else they would like to discuss
(11:52:37 AM) cboltz: darix recently complained that the 2.12 abstractions are too old - probably because it has been a while since the last 2.12.x release
(11:52:40 AM) cboltz: so maybe do one? ;-)
(11:52:57 AM) cboltz: (and maybe also another 2.13.x because we backported quite some things in the last month)
(11:54:39 AM) jjohansen1: yes, another 2.13.x is due to address issues in 2.13.4, its been 10 months for 2.12 so I suppose it is due for a new release as well
(11:55:16 AM) jjohansen1: I am not so sure about 2.11, and 2.10
(11:56:10 AM) jjohansen1: actually circling back to release cadence, there is a point with older releases that I would like to even go longer than 6 months between updates
(11:57:53 AM) cboltz: I think we'll automatically get that by "not many commits" and/or "no important changes"
(11:59:05 AM) jjohansen1: yeah
(11:59:25 AM) jjohansen1: I just wanted to call it out more explicitly
(12:00:32 PM) jjohansen1: looking at it, 2.11 has some fixes, and 2.10 has some typo fixes and one real bug fix
(12:01:16 PM) jjohansen1: I will work on getting some releases out for all of them with 2.11 and 2.10 being lower priority
(12:01:44 PM) jjohansen1: I won't get any of the stable releases done this week
(12:02:58 PM) jjohansen1: and will probably prioritize the 5.8 pull request, so we are looking at 2-3 weeks before I get to stable updates
(12:03:10 PM) jjohansen1: with 2.13 being the possible exception
(12:03:31 PM) jjohansen1: next up, next IRC meeting
(12:04:08 PM) jjohansen1: should we skip the May meeting which is scheduled to be just in 2 weeks
(12:04:25 PM) jjohansen1: and make the next meeting be the June meeting
(12:04:25 PM) sarnold: heh, sounds like a good candidate to skip to me
(12:04:29 PM) jjohansen1: may vote is yes
(12:05:29 PM) cboltz: no objections
(12:06:14 PM) jjohansen1: alright, I will send out a mail
(12:06:24 PM) jjohansen1: thanks everyone, meeting adjourned
 ```