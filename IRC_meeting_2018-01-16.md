```
[19:00:06] <cboltz> meeting time ;-)
[19:00:22] <jjohansen> yep
[19:00:30] <Talkless> intrigeri absent?
[19:01:09] <jjohansen> meeting time: intrigeri, tyhicks, jdstrand, sbeattie, sarnold
[19:01:17] <jjohansen> Talkless: it would appear so
[19:01:36] <jjohansen> I don't think tyhicks, or jdstrand will be able to make it today either
[19:02:04] <jjohansen> and it appears sarnold is not around yet
[19:02:26] <jjohansen> nor Igoldwyn
[19:02:26] <sbeattie> oh, right, meetings are earlier in the day now.
[19:03:15] <jjohansen> yeah, so we are down to 4, should we post pone, to tomorrow or next week or run through a few things
[19:03:53] <cboltz> I'd say run through the things we can discuss without having everybody around
[19:03:56] <sbeattie> I don't have a problem running through a few thing quickly.
[19:05:33] <jjohansen> okay, so we didn't have much on the schedule beyond gitlab status, and release cadence
[19:06:01] <jjohansen> any complaints with gitlab beyound to its git
[19:06:09] <jjohansen> ;-)
[19:07:03] <cboltz> only the handling of Acked-by timeout (and that the perfect solution is not to need it), so nothing new
[19:07:11] <sbeattie> I'm not crazy about the way it handles merges, but it's not egregious
[19:07:20] <Talkless> does gitlab has (will have?) full wiki copy, as http://wiki.apparmor.net/ is down?
[19:07:24] <jjohansen> so yeah both of those are bad
[19:07:38] <sbeattie> Talkless: the wiki is mostly there.
[19:08:02] <Talkless> ok
[19:08:12] <sbeattie> I think the DNS snafu was the result of jjohansen trying to get wiki.apparmor.net to point there.
[19:08:27] <jjohansen> a couple of conversion issue, but content should be there, and we do have a dump of the old wiki if we find something is missing
[19:08:35] <jjohansen> yeah
[19:08:52] <cboltz> what's the exact problem with the DNS?
[19:09:05] <sbeattie> There's a few small bits missing, and it all could use a review, both for specific formatting issues (I tried to do so in the conversion) and an overall review.
[19:09:25] <jjohansen> so I have been trying to get apparmor.net to point to the gitlab.com/apparmor page and wiki.apparmor.net to point to the wiki in gitlab
[19:10:18] <cboltz> but...?
[19:10:24] <jjohansen> except well, doing the updates with the domain registrar hasn't worked, I have tried multiple times
[19:10:36] <jjohansen> I am in contact with their tech support
[19:11:00] <jjohansen> if something doesn't happen soon I will move the domain to a new registrar
[19:11:14] <cboltz> you'll probably need a little apache vhost that sends out a redirect
[19:11:30] <cboltz> I have enough servers running, so if you need such a redirect vhost, tell me ;-)
[19:11:37] <jjohansen> cboltz: likely
[19:12:35] <jjohansen> but the registrar is supposed to have the ability to set that basic vhost and redirect
[19:12:40] <jjohansen> its just isn't working
[19:12:51] <jjohansen> I will let you know if I need a vhost
[19:13:32] <cboltz> ok
[19:13:40] <cboltz> don't be too patient with your registrar ;-)
[19:13:42] <jjohansen> do the other part of the gitlab question was what do we want to do about bug tracking, and translations
[19:13:55] <jjohansen> so far we have left them on launchpad
[19:14:03] <sbeattie> jjohansen: right, I noticed you merged in some translations fixes.
[19:14:38] <jjohansen> yeah, it was a manual process
[19:14:57] <jjohansen> I certainly wouldn't mind it being better, but it wasn't too bad
[19:15:18] <cboltz> for translations, the question is how many translations we get via launchpad "for free" we wouldn't get when moving away from launchpad
[19:15:46] <jjohansen> right, ENOCLUE
[19:15:49] <cboltz> (in other words: do you think we could ask our current translators to work on $whereever instead of launchpad?)
[19:17:10] <cboltz> _if_ we want to move away from launchpad, using weblate (as hosted service) might be an option
[19:17:19] <jjohansen> I really don't know, I just don't have any insight on the translation side
[19:18:01] <cboltz> sbeattie: any insight or opinion from you?
[19:18:06] <jjohansen> the question is, is it worth moving away from launchpad, just to move to another service that is external to the main hosting on gitlab
[19:18:26] <sbeattie> yeah, I don't have much insight into the translations community. 
[19:18:57] <sarnold> good morning
[19:19:21] <cboltz> AFAIK weblate should be able to interact with git/gitlab - I don't know the exact workflow, but it will probably commit to a git branch or something like that
[19:19:44] <jjohansen> also I think bug tracking plays in, if we are keeping bug tracking on lp, then that is some impetus to stay with lp, so we aren't split across 3 services
[19:19:46] <cboltz> so it would be similar to how launchpad interacts with bzr
[19:20:35] <jjohansen> cboltz: lp is interacting with gitlab, its pulling in the project its just that the translations are still in a bzr tree
[19:21:12] <cboltz> so the only manual step is to push new translations to gitlab? Doesn't sound too bad ;-)
[19:21:19] <jjohansen> its not a huge deal, the only person who has to deal with it is the person merging the translations
[19:21:52] <jjohansen> right, update a bzr co, take a diff, apply to gitlab and push
[19:21:59] <jjohansen> its not too bad
[19:22:49] <cboltz> sounds like something you should run in cron.weekly - no need to do that manually ;-)
[19:23:11] <jjohansen> yeah that is not a bad idea
[19:23:25] <jjohansen> so atm, I think we just stick with lp
[19:23:46] <jjohansen> but I am open to suggestions, and revisiting
[19:24:23] <jjohansen> The other topic was release cadence and maintenance releases
[19:24:31] <cboltz> my main concern was syncing between gitlab and bzr, but since this doesn't look too hard - no objections to keeping translations on lp
[19:25:54] <jjohansen> I would like more regular releases, but I think we need more people in on the discussion
[19:26:06] <sbeattie> jjohansen: I think we should probably revisit whether issue tracking should be on lp or gitlab in a month or two
[19:26:21] <jjohansen> I will say that I would like to get the beta for the next release out in early February
[19:26:26] <cboltz> I more than once added "changes-since-$release" patches in the openSUSE packages - more regular releases would help to avoid that
[19:26:27] <sbeattie> jjohansen: I concur, and apologize for not driving them more frequently.
[19:26:30] <jjohansen> sbeattie: sounds good
[19:26:58] <sbeattie> (concur == more frequent releases)
[19:27:13] <jjohansen> :)
[19:27:42] <jjohansen> so for time frame, I want to land the next release before the 4.16 kernel is released
[19:28:05] <jjohansen> I am going to try and land the networking patch again for 4.16
[19:28:08] <sarnold> oh god. "early februrary" is two weeks away, not two months.
[19:28:34] <jjohansen> to use it, you will need an updated user space with policy versioning support
[19:29:25] <jjohansen> I am working at landing the first iteration of policy versioning for the parser, which is the minimum required, but it would be nice if the tools could support it
[19:30:08] <jjohansen> If need be tool support could follow in a point release, or we can follow up with another major release
[19:30:32] <jjohansen> the basic policy versioning support in the parser would be
[19:30:33] <cboltz> do you have a quick summary (especially of the added syntax etc.) the tools need to support somewhere?
[19:30:37] <jjohansen> - conditional include
[19:30:44] <jjohansen> - feature definition
[19:30:51] <jjohansen> - policy hashing
[19:31:01] <jjohansen> - maybe multiple cache files
[19:31:41] <jjohansen> cboltz: you have seen conditional include, I will get a first version of the feature definition patch up this week
[19:31:58] <jjohansen> and we can bicker about exact syntax then
[19:32:12] <cboltz> ok
[19:32:55] <jjohansen> there will need to be some work done on apparmor conditional support, but I expect most of that to come in a later release
[19:33:27] <jjohansen> we will need to decide which release to backport basic policy versioning to
[19:34:27] <jjohansen> conditional include, and the basic feature declaration aren't going to be bad to backport
[19:35:18] <jjohansen> but the rest of it, I am very leery of backporting
[19:35:43] <jjohansen> oh the other big one, is policy is going to need to be updated to use policy versioning
[19:36:37] <sarnold> only for the policies that use the new features though, right?
[19:37:25] <cboltz> abstractions/nameservice includes network rules, and we don't have too many profiles not using this abstraction ;-)
[19:37:27] <jjohansen> sarnold: well for any policy using network, unix or dbus rules which exist in the base abstractions
[19:37:50] <cboltz> oh, and abstractions/base even uses unix rules ;-)
[19:39:41] <jjohansen> I don't have anything else, the floor is open
[19:40:17] <Talkless> so, first tool has to support versioning, then update policies, then with new kernel and new features pinned, it (versioning) will start to work?
[19:43:47] <jjohansen> Talkless: yeah it requires all 3 to be updated
[19:44:31] <jjohansen> kernel could come before policy, which just means older policies use an older abi
[19:44:50] <jjohansen> kernel will also support older tools/policy
[19:45:11] <Talkless> if kernel is old, how tool will handle ABI versions, if kernel does not... support.. stating it's abi.. or whatvere that works?
[19:45:21] <jjohansen> the only hard ordering is the tools have to be updated before the policy is changed
[19:45:30] <cboltz> older abi will be funny[tm] for openSUSE because network rules won't be enforced anymore, right?
[19:46:02] <jjohansen> Talkless: so there will be the option to downgrade (so enforce what can be enforced), or fail
[19:46:36] <Talkless> jjohansen: that's by parser.conf ?
[19:47:00] <jjohansen> or on the command line
[19:47:04] <jjohansen> either would work
[19:47:45] <Talkless> k
[19:48:43] <cboltz> for the tools, the perfect solution would need some rewrites (which possibly includes handling nested child profiles, or nested $whatever in general)
[19:48:57] <cboltz> doing all this right will be interesting[tm] because with nested child profiles, the way how the tools store the profiles internally will need to change - it's currently limited to main profile + a single level of child profiles
[19:49:17] <cboltz> however, adding some basic support (just to ensure the new rules get parsed and are kept when writing the profile) shouldn't be too hard
[19:50:29] <cboltz> I already have an idea how the code should look like after the rewrite, but I don't yet know when I have time to actually write it
[19:50:52] <cboltz> (to make things more interesting, test coverage of this part of the tools is... limited)
[19:51:00] <jjohansen> sure
[19:51:22] <jjohansen> cboltz: remember this is just the first iteration, there is lot to do on the parser side
[19:51:43] <jjohansen> all I am trying for is the minimum to support using the feature upstream
[19:52:47] <cboltz> yes, I know
[19:52:54] <cboltz> I just wanted to show the bigger picture ;-)
[19:53:42] <jjohansen> sure, lots of work todo
[19:53:44] <cboltz> (as a quick solution, we might end up with another Raw_*_Rule class that just stores the new rule types, but doesn't really understand it)
[19:53:50] <jjohansen> anything else?
[19:54:11] <cboltz> not on this topic
[19:54:29] <jjohansen> okay, any other topic
[19:54:57] <cboltz> when was the last time someone did an upload to coverity?
[19:55:16] <sbeattie> cboltz: a while ago, that's on my list to automate.
[19:55:56] <cboltz> automating it sounds like a good idea
[19:56:21] <cboltz> but I'd recommend to do a manual upload *now* (and then maybe monthly) until this is done
[19:58:15] <sbeattie> yeah, not a bad idea, and doing so would push me to automate.
[19:58:40] <cboltz> ;-)
[19:59:44] <jjohansen> okay anything else?
[20:00:49] <cboltz> I found out that desktop notifications now need $DBUS_SESSION_BUS_ADDRESS to work
[20:01:08] <jjohansen> oh fun
[20:01:10] <cboltz> it's   DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus   on openSUSE
[20:01:27] <cboltz> does it look the same way on Debian and Ubuntu, or do we need to make it a commandline parameter?
[20:01:48] <cboltz> jjohansen: it's even more funny - if I don't set it, it starts to eat up CPU
[20:02:07] <cboltz> (no idea why exactly)
[20:02:44] <jjohansen> bad error handling, would be my guess
[20:03:46] <cboltz> sounds like a valid guess- if someone wants to help with debugging, ping me after the meeting
[20:04:24] <cboltz> jjohansen, Talkless: what does   echo $DBUS_SESSION_BUS_ADDRESS   print for you?
[20:05:04] <sbeattie> cboltz: on ubuntu 17.10, I get the same: unix:path=/run/user/1000/bus
[20:05:14] *** Quits: Shivaprasad (~Shivapras@183.82.23.231) (Quit: Leaving.)
[20:05:25] <jjohansen> unix:path=/run/user/1000/bus
[20:05:35] <Talkless> cboltz: unix:path=/run/user/1000/bus
[20:05:53] <cboltz> nice, so it's the same scheme everywhere
[20:05:54] <jjohansen> irc the 1000 is the uid
[20:05:58] <cboltz> right
[20:06:24] <cboltz> so I can hardcode it (except the uid) and don't need to add another parameter
[20:07:41] * cboltz adds this to his TODO list
[20:08:15] <cboltz> any other topic besides "next meeting date"?
[20:11:19] <sbeattie> not from me
[20:12:02] <cboltz> so - when should we do the next meeting?
[20:12:16] <cboltz> 2nd Tuesday would be Feb 13, but that won't work for me
[20:12:39] <cboltz> (last day of carnival)
[20:12:53] <jjohansen> wednesday, or thursday are alternatives
[20:14:17] <cboltz> I'd prefer thursday if it doesn't exclude someone else
[20:14:50] <jjohansen> I'm fine with thursday
[20:15:27] <jjohansen> we can send out an announcement and ask for people to reply if its a problem
[20:15:50] <jjohansen> that way intrigeri and goldwyn can have a say
[20:16:09] <jjohansen> well and anyone else who isn't here but might be interested
[20:17:32] <cboltz> good idea
[20:17:58] <cboltz> BTW: please define the "we" in "we can send out ..." ;-)
[20:18:09] <jjohansen> alright lets set it tentatively for thursday Feb 15, @18:00 UTC
[20:18:18] <jjohansen> I will send out an email
[20:18:43] <cboltz> :-)
[20:18:59] <sbeattie> thursday works better for me than wednesday
[20:19:06] <sbeattie> (of that week in feb)
[20:20:35] <jjohansen> alright email sent
[20:24:30] <jjohansen> alright meeting adjourned, thanks everyone
[20:24:30] <sbeattie> thanks!
[20:24:37] <cboltz> thanks!
[20:26:28] <Talkless> thanks, nice to read about new upcomming features  :)
[20:27:58] <sarnold> thanks jjohansen :)
```
