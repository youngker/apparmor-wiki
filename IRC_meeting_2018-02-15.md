```
[19:00:15] <cboltz> meeting time ;-)
[19:00:35] <goldwyn> hi
[19:01:27] <tyhicks> hello
[19:02:10] <tyhicks> quite the agenda today: https://gitlab.com/apparmor/apparmor/wikis/MeetingAgenda#next-meeting-irc-meeting-1800-utc-thursday-feb-15-2018
[19:02:23] <tyhicks> should we start with "..." or "anything else"? ;)
[19:02:39] <cboltz> lol
[19:03:28] <jjohansen> hey cboltz, goldwyn
[19:03:51] <jjohansen> sbeattie: , sarnold, jdstrand meeting time
[19:04:07] <jdstrand> hello
[19:04:08] <jjohansen> Talkless: meeting time if you want
[19:04:44] <sbeattie> ohai
[19:05:05] <jjohansen> okay lets get started
[19:06:10] <jjohansen> we are working off of the last bit of last months meeting schedule, sorry I meant to move it forward but just haven't yet
[19:06:42] <sbeattie> jjohansen: thanks for gitting the wiki dns straightened out
[19:07:14] <jjohansen> np, though I still need to get better rewrite rules working so that wiki refs like
[19:07:50] <jjohansen> wiki.apparmor.net/AppArmorMonitoring
[19:08:01] <jjohansen> resolve to https://gitlab.com/apparmor/apparmor/wikis/AppArmorMonitoring
[19:08:05] <jjohansen> instead of https://gitlab.com/apparmor/apparmor/wikis/home/index.php/AppArmorMonitoring
[19:08:17] <jjohansen> but at least its a start
[19:08:55] <jjohansen> so the 4.0 release, I don't see us getting a beta out this month unfortunately
[19:09:09] <jjohansen> there is just too much too do, especially around policy
[19:10:19] <jjohansen> we could go the route of releasing a "beta" with feature release exceptions iff its going to help a distro include the next version in a release
[19:10:25] <tyhicks> you're talking about policy versioning?
[19:10:28] <jjohansen> yes
[19:11:23] <cboltz> what about a "small" release with just the patch for multiple caches we discussed at FOSDEM?
[19:11:55] <cboltz> (personally, I won't complain about a beta with exceptions, but I'm not sure if the SLE people like that ;-)
[19:12:00] <jjohansen> cboltz: yes, that is still on the table, whether suse wants a 2.12.1 or to call it 2.15 I'll let you make that call
[19:12:48] <cboltz> the additional feature sounds like a reason against the 2.12.1 number IMHO
[19:12:52] <goldwyn> cboltz: for which SLE release? SLE15 is frozen with 2.12
[19:13:03] <jjohansen> well I don't particularly like beta with release exceptions either, its only something I would consider iff it would help with getting 4.0 into a release it wouldn't make other wise
[19:13:39] <cboltz> goldwyn: we (jjohansen, Richard brown and I) had a discussion at FOSDEM, and support for multiple caches would solve some problems with Kubic
[19:14:20] <cboltz> and Richard assumed that we could still get that into SLE 15
[19:14:53] <jjohansen> goldwyn: basically cherry-picking a feature to address an open issue
[19:15:22] <jjohansen> like I said, whether you guys want to call it 2.12.1 or something else like 2.15 is up to you
[19:15:30] <jjohansen> 2.12 is already a suse special
[19:15:48] <cboltz> I'd tend to call it 2.15
[19:15:54] <goldwyn> now unless if that feature has a FATE (feature request), it will not make it.
[19:16:11] <goldwyn> However, if you say it is a open issue: What is the open issue?
[19:16:32] <goldwyn> Sorry if I sound like the ignorant fool. Missed the last meeting.
[19:16:38] <cboltz> creating the cache fails because Kubic has a read-only filesystem at boot
[19:17:00] <cboltz> support for multiple caches would mean we can pre-build the cache, and maybe even ship the cache in a package
[19:17:00] <goldwyn> Hmm, if it can be regarded as a bug, we could push it in.
[19:17:22] <jjohansen> goldwyn: there is an existing bug, give me a sec I'm looking for it
[19:17:42] <goldwyn> I see. Now it is clearer.
[19:18:11] <sbeattie> goldwyn: don't worry, I'm just as in the dark, so your questions are helpful to me.
[19:18:44] <cboltz> there is also https://build.opensuse.org/request/show/567451 with the idea to use a systemd timer to create the cache some minutes after boot
[19:19:01] <jjohansen> I think this is it
[19:19:03] <jjohansen> https://bugzilla.suse.com/show_bug.cgi?id=1069906
[19:19:08] <cboltz> but as you can read in my comment (and can't read in some follow-up mails), I don't like that idea
[19:20:26] <jjohansen> basically having multiple caches will let you ship pre built caches and not worry about the kernel invalidating an existing cache so you need to write it
[19:20:51] <jjohansen> Thorsten had asked Richard to talk to me about it at fosdem
[19:22:01] <jjohansen> cboltz: unfortunately I am running behind on the promised patches, but I should have something this weekend
[19:22:03] <goldwyn> Okay, so what is in OBS is the systemd timer stuff. right?
[19:22:31] <cboltz> right, SR 567451 is about the timer
[19:23:49] <goldwyn> when is the multiple caches patches expected?
[19:24:23] <tyhicks> the initial implementation is already available: https://gitlab.com/apparmor/apparmor/merge_requests/4
[19:24:57] <jjohansen> right, I have some revision patches, I need to finish up, I was hoping last weekend, but its looking like this weekend now
[19:25:02] <tyhicks> however, there are some things still needing to be fixed in that PR and I don't know what all additional features are needed for the Kubic bug
[19:25:31] <tyhicks> jjohansen: you're working on patches that go on top of PR #4?
[19:25:36] <jjohansen> basically all they need is multiple caches, so they can ship more than one precompiled
[19:25:40] <jjohansen> tyhicks: yeah
[19:26:06] <jjohansen> nothing huge
[19:26:23] <jjohansen> PR#4 is basically what we are going to run with
[19:28:37] <jjohansen> okay, are we done with multiple caches for suse
[19:29:00] <cboltz> is there a chance to have /usr/.../cache/ as additional cache dir for packaged cache?
[19:29:21] <jjohansen> cboltz: what do you mean by additional?
[19:29:51] <cboltz> similar to /lib/systemd/system/*.service and /etc/systemd/system/*.service
[19:30:00] <cboltz> so packaged cache could go to /usr/...
[19:30:22] <cboltz> and if someone modifies a profile, the updated cache would go to /etc/apparmor.d/cache (or whatever cachedir is configured)
[19:30:39] <cboltz> this could avoid some fun with *.rpmnew files
[19:30:58] <jjohansen> cboltz: yeah, that is doable
[19:31:28] <cboltz> :-)
[19:31:38] <sarnold> oof, I'm not used to these early morning meetings yet .. sorry
[19:32:07] <cboltz> sarnold: EWRONGTIMEZONE ;-)
[19:33:24] <sarnold> cboltz: story of my life :)
[19:33:39] <cboltz> as a sidenote for those who wonder why the next release will likely be named 2.15 (and why we skip 2.13 and 2.14) - you probably know that 13 is seen as an unlucky number, and as I learned a while ago, 14 is seen as unlucky number in asia
[19:34:52] <tyhicks> does that outweigh the potential confusion for users?
[19:35:03] <jjohansen> the other half of the story is suse finds it amusing :P
[19:35:33] <cboltz> right, it will match the next SLE and openSUSE Leap release - 15
[19:35:47] <cboltz> SLE skipped 13 and 14 for exactly that reason
[19:36:00] <cboltz> and Leap is even more funny - after 42, we'll have 15 ;-)
[19:36:02] <jjohansen> At this point I think its probably best for suse to name it 12.1 as that is a point release to what they already have in
[19:36:22] <goldwyn> oh,, please no more numerology crap. it sure is amusing and confusing.
[19:36:36] <jjohansen> but if they want to call this suse special release 2.15, I'm not opposed to letting them do it
[19:37:03] <jjohansen> our next major upstream release with policy versioning will be 4.0
[19:37:21] <cboltz> goldwyn: you don't even want to know the other numbering schemes we discussed in the board for Leap ;-)
[19:38:15] <cboltz> jjohansen: I'm not sure if 2.12.1 is a good idea - it's about adding a feature, which is nothing I'd expect in a minor release
[19:38:15] <jjohansen> alright I think that covers multiple caches for suse
[19:38:22] <tyhicks> I hate to be a stick in the mud but I'd like to cast a vote for predictable versioning that can be clearly documented
[19:38:42] <jjohansen> cboltz: yes I know but it is also cherry-picking a specific feature to address an open issue
[19:38:59] <jjohansen> but I'll let suse make the call
[19:39:29] <cboltz> well, you know my opinion ;-)
[19:39:51] <jjohansen> cboltz: yes but you also have to get it past Thorsten
[19:40:08] <jjohansen> tyhicks: yeah I like predictable numbers too
[19:40:34] <cboltz> I just have to route it via Richard ;-)
[19:41:01] <jjohansen> I'm not fond of skipping 3.0, but that could cause confusion, hence us jumping to 4.0
[19:41:31] <tyhicks> what's the cause of the confusion around 3.0?
[19:41:52] <jjohansen> the dev kernel patches in ubuntu were called 3.x
[19:42:13] <jjohansen> with the upstreaming and policy versioning I'd like to make a clean break
[19:43:00] <tyhicks> I can see how that could cause confusion
[19:44:35] <sbeattie> jjohansen: why 4.0 and not 3.0?
[19:44:46] <sbeattie> oh do
[19:44:48] <sbeattie> doh
[19:44:55] <sbeattie> got it
[19:45:08] <cboltz> if someone asks why we jump, we can always say that we are just following traditions - examples being PHP, Typo3 and SLE ;-)
[19:45:28] <jjohansen> going forward, I'd like to keep thing more consistent, and even avoid the silliness of skipping .13, .14, and .4 (if we you want to include japan)
[19:46:54] <cboltz> sounds like we should simply find a reason to increase the major release after doing a .12 release ;-)
[19:47:06] <cboltz> (and BTW, AFAIK 14 is about china, not japan)
[19:47:56] <tyhicks> I'm not sure that skipping 3.0 will result in less confusion than if we used 3.0 and a small number of users got mixed up with the Ubuntu kernel dev patches labeled 3.0
[19:48:19] <tyhicks> only a very small number of AppArmor users will ever come across that versioning scheme used for the Ubuntu kernel patches
[19:48:39] <tyhicks> every AppArmor user will experience the 2.x to 4.x jump
[19:49:16] <jjohansen> tyhicks: okay, we can revisit the 3.0/4.0 decision
[19:49:42] <jjohansen> its true that the 3.x stuff was mostly visible to me
[19:50:46] <sarnold> (indeed I couldn't have told you what the 3.x was used for :)
[19:51:14] <jjohansen> cboltz right .14 is china, .4 is japan because it sounds like death or dead or some such
[19:51:53] <cboltz> http://www.prokerala.com/numerology/chinese-unlucky-numbers.htm says 4 is also china
[19:51:58] <goldwyn> btw, why are there two LSS conferences this year?
[19:52:13] <cboltz> but I have to admit I didn't search for japanese unlucky numbers ;-)
[19:52:19] <sarnold> goldwyn: I understand a lot of folks didn't want to come to the united states
[19:52:24] <sarnold> goldwyn: and rather than just move it, ..
[19:52:41] <goldwyn> well it is not in the US. which means I will need a visa.
[19:53:13] <jjohansen> goldwyn: because LSS wanted to service the non-north american community as well, and having one just in europe didn't work for a lot of people
[19:53:18] <jjohansen> its an experiment
[19:53:38] <tyhicks> we're getting close to the top of the hour - is there anything else urgent that we should talk about for this month?
[19:53:39] <goldwyn> so the talks would be the same?
[19:53:46] <jjohansen> goldwyn: there north american one is in canada
[19:54:00] <jjohansen> the european one is in Scotland
[19:54:47] <tyhicks> I added this to the meeting agenda but I'll repeat it here - John updated the AppArmor kernel backports branch: http://kernel.ubuntu.com/git/jj/linux-apparmor-backports/
[19:54:49] <jjohansen> tyhicks: 3.0/4.0 beta are we going to want that in 18.04?
[19:55:08] <tyhicks> s/branch/tree/
[19:55:41] <jjohansen> so two points to add to that, there are a few more backport kernels to add some time
[19:55:58] <jjohansen> but I am not in a hurry to do them and
[19:56:15] <Talkless> Hi everyone. Is there a list of upcomming 4.0 features anywhere?
[19:56:15] <jjohansen> I wanted to know whether people wanted that tree pushed to gitlab
[19:56:57] <jjohansen> Talkless: 3.0/4.0 what ever we call it is going to cover the first steps of policy versioning
[19:57:00] <jjohansen> so
[19:57:05] <jjohansen> - multiple caches
[19:57:13] <jjohansen> - specifying a version in policy
[19:57:58] <jjohansen> - and maybe hashing of the compiled binary to deal with certain cache consistency issues
[19:58:08] <tyhicks> jjohansen: gitlab is probably a better place for it since kernel.ubuntu.com doesn't seem to support https for the git transport
[19:58:48] <jjohansen> tyhicks: I agree, I just wanted to make sure others agreed with me
[19:59:17] <cboltz> speaking about gitlab - am I the only one who didn't receive mail notifications since about a month?
[19:59:27] <tyhicks> I've been getting notifications
[19:59:55] <jjohansen> Talkless: 3.0/4.0 is going to be required to work with the upstream network rules when they land as we are doing an abi break to avoid the issues that got us the 4.14 revert
[20:00:30] <sbeattie> cboltz: I'm also getting notifications
[20:00:34] <tyhicks> jjohansen: regarding 3.0/4.0 beta in 18.04, I don't think multiple caches is a hard requirement for us at this time so I don't think we should break our neck to get it in
[20:00:43] <sarnold> cboltz: it's been several days since my last notification, bu feb 8 was the last code push I saw, I think..
[20:00:56] <jjohansen> tyhicks: I was more thinking of the policy versioning
[20:01:08] <tyhicks> jjohansen: policy versioning would certainly be nice but I didn't think that was going to be included in a beta
[20:01:14] <cboltz> strange[tm], my latest mail from gitlab was from Jan 11 :-/
[20:01:45] <jjohansen> not if we roll a beta next week, but maybe if we roll one on the 26
[20:01:46] <sbeattie> cboltz: for commits only, or for merge requests as well
[20:02:01] <jjohansen> it tight for the March 1st FF
[20:02:38] <jjohansen> but I'd really like to get the next release out before too long so the changes can start propogating
[20:02:39] <Talkless> jjohansen: is policy verioning, which is upocomming in 4.0, is same-or-similar to these.. "conditionals" ?
[20:03:29] <jjohansen> Talkless: well better conditional support is planned for but it is not going to make the first cut
[20:03:50] <jjohansen> the next release will have the bare minimum to support policy versioning
[20:04:09] <jjohansen> and then we can stage more features into subsequent releases
[20:05:46] <jjohansen> can we back up a minute, and take a vote on whether the next major release, the first to support policy versioning should be 3.0 or 4.0
[20:06:41] <sarnold> I prefer 3.0 but could live with 4.0.
[20:06:53] <tyhicks> I like 3.0
[20:07:16] <cboltz> both are fine for me ;-)
[20:08:20] <jdstrand> we are talking about just the version string here? if so, no vote
[20:09:48] <jjohansen> jdstrand: yes, its just the version we are going to call it, not a big deal
[20:10:03] <jdstrand> right, that is what I thought. /me defers
[20:11:13] * sbeattie abstains as well
[20:12:08] <sbeattie> cboltz: FYI, your email alias is still in the set of recipients for 'emails on push' in gitlab
[20:12:39] <cboltz> good to know, thanks
[20:15:55] <jjohansen> well I vote for 4.0, but its moot
[20:16:02] <jjohansen> 2 - 3.0
[20:16:05] <jjohansen> 1 - 4.0
[20:16:15] <jjohansen> 2 abstain
[20:16:22] <jjohansen> so 3.0 it is
[20:16:58] <jjohansen> alright, lets finish up, we can talk about trying to have more regular releases, and kibitz more about consistent versioning next time
[20:17:02] <jjohansen> thanks everyone
[20:17:06] <jjohansen> meeting adjourned
[20:17:20] <sarnold> thanks jjohansen :)
[20:18:07] <cboltz> thanks!
[20:18:11] <goldwyn> thanks!
[20:18:27] <Talkless> thanks for info jjohansen
[20:20:11] <sbeattie> thanks jjohansen
[20:20:49] <tyhicks> thanks jjohansen!
[20:27:20] <jdstrand> thanks jjohansen :)
```
