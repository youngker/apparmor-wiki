```
(11:00:23 AM) intrigeri: meeting time
(11:00:44 AM) intrigeri: http://wiki.apparmor.net/index.php/MeetingAgenda
(11:01:02 AM) intrigeri: jjohansen, cboltz, sarnold, jdstrand ← or did I get the time wrong?
(11:01:42 AM) cboltz: your clock is right ;-)
(11:01:43 AM) goldwyn: intrigeri: in that case, I got it wrong too ;)
(11:02:17 AM) jjohansen: ha no, I had forgotten that we moved it a couple hours forward too
(11:02:53 AM) peetaur [~peter@p200300E10BC01300428D5CFFFEE650DA.dip0.t-ipconnect.de] entered the room.
(11:02:58 AM) jjohansen: sarnold, sbeattie, jdstrand, tyhicks meeting time
(11:03:04 AM) ***tyhicks is around
(11:03:18 AM) sarnold: ohai
(11:03:53 AM) jdstrand: hi
(11:04:01 AM) intrigeri: anything to add to the agenda?
(11:04:50 AM) ***sbeattie is here
(11:04:52 AM) jjohansen: right feel free to update the agenda or ping me and I will add items to the end
(11:05:01 AM) jjohansen: alright lets get started
(11:05:21 AM) intrigeri: I have nothing to add. The usual daily stuff can be discussed at other times.
(11:05:32 AM) jjohansen: first up, the 4.0 release was targeted for this fall
(11:05:34 AM) cboltz: jjohansen: "wiki update (and move)" is missing
(11:05:40 AM) goldwyn: jjohansen: I tried signing up for editing the wiki, but I never got a response/confirmation mail.
(11:06:13 AM) jjohansen: goldwyn: sorry, I should add in the instruction you need to ping someone because the email notification seem to be broken
(11:06:29 AM) jjohansen: if your request is still there I will approve it
(11:06:46 AM) jjohansen: cboltz: actually deliberately so :)
(11:07:05 AM) jjohansen: I know we need to get moving on it but the status hasn't changed
(11:07:17 AM) jjohansen: anyways, back to 4.0
(11:08:42 AM) jjohansen: the timing for it is not great, it doesn't suite ubuntu's next release and I wanted to see about suse, debian etc and try to plan out the timing of things a little better
(11:08:54 AM) ***cboltz wonders if jjohansen really hoped I'll forget this topic
(11:09:10 AM) sbeattie: jjohansen: is there a description of what you want to land in 4.0?
(11:09:41 AM) jjohansen: cboltz: no, not forgotten, just trying to make sure we complete the meeting before I have to run, for a lunch time apt
(11:09:58 AM) cboltz: ;-)
(11:10:19 AM) jjohansen: sbeattie: written down no, but there are a few things I would like to land in userspace that haven't happened yet
(11:10:52 AM) jjohansen: in particular I want to finish up and land the first iteration of policy versioning
(11:11:05 AM) intrigeri: (yes, please :)
(11:11:53 AM) jjohansen: the current policy situation is causing more and more problems (at least for me doing support) and if we are going to get enable in debian I think its going to almost be a requirement
(11:12:04 AM) intrigeri: re. timing for Debian: [now..March] is a good time for innovative change that might need stabilization time. Expect a freeze in ~1 year.
(11:12:32 AM) jjohansen: pinning is all well and good, but debian is trying to push more policy into applications, the more the applications drift from the base, the more problematic it becomes
(11:13:16 AM) jjohansen: thanks
(11:13:22 AM) jjohansen: cboltz: what about suse
(11:13:25 AM) sbeattie: yeah, I get that. I don't want to get in a rabbit hole of motivating each of the features, just trying to picture what they might be.
(11:13:58 AM) cboltz: I expect openSUSE Leap 15 (and also SLE 15) to be released in ~1 year, maybe a bit earlier
(11:14:46 AM) cboltz: I don't know an exact schedule yet, but since AppArmor should ideally enter Leap via SLE, a new major release shouldn't arrive too late
(11:15:02 AM) cboltz: goldwyn: do you know more about schedule and freeze plans for SLE?
(11:15:45 AM) goldwyn: SLE15 release date is Apr 19, 2018. We are in beta, so feature freeze.
(11:16:36 AM) jjohansen: hrmm so we already missed SLE15 :(
(11:16:40 AM) cboltz: earlier than I thought / remembered - so it's clearly earlier ;-)
(11:16:58 AM) sarnold: :(
(11:17:17 AM) cboltz: well, SLE already earned a "told you so" at the last major release from me, so I wouldn't be surprised if we get an exception this time ;-)
(11:17:39 AM) intrigeri: there will be a SLE16.
(11:17:47 AM) jjohansen: depends on timing
(11:17:51 AM) cboltz: I'm currently carring the aa-logprof json patches in 2.11.x for Tumbleweed, which makes maintaining the package funny[tm] (I had to rebase the patch for the new release)
(11:17:57 AM) JanC [~janc@ip-213-49-191-209.dsl.scarlet.be] entered the room.
(11:18:09 AM) jjohansen: intrigeri: sure, it just would have been nice to get a new release in suse as well
(11:18:17 AM) intrigeri: absolutely.
(11:18:41 AM) cboltz: so I'd really like to have those patches in an official upstream release - and while merging them into 2.11 would be the easiest solution, I'm not sure if this is what people expect in a maintenance branch
(11:18:58 AM) jjohansen: cboltz: I am willing to allow that into 2.11.2, suse is the only consumer so its not going to break anyone
(11:19:33 AM) cboltz: would you also include unix rule support for aa-logprof in 2.11.2?
(11:20:00 AM) cboltz: (I don't have much code for this yet)
(11:20:56 AM) jjohansen: ugh, that is more problematic, suse won't be the only one that will get the change
(11:21:11 AM) cboltz: I know ;-)
(11:21:45 AM) cboltz: the good thing is that the risk isn't too big - it can only have bugs in parts that are not covered by code today ;-)
(11:21:47 AM) jjohansen: I think we need to consider it, if we can't get 4.0 into SLE
(11:21:54 AM) intrigeri: ftr/fwiw 2.11.1 is too big for me to dare proposing for a Debian stable (Stretch) update.
(11:21:57 AM) cboltz: and test coverage for my rule classes is typically 100%
(11:22:27 AM) intrigeri: so .2 can as well add features. I'm not sure I'm 100% happy with the maintenance branch policy on the long term, but for 2.11.x it's how it is :)
(11:22:51 AM) jjohansen: intrigeri: yes, the 2.11.1 release window drag out far too long, it should have been released 6 months ago
(11:23:01 AM) jjohansen: we need to get better about doing regular releases
(11:23:02 AM) JanC_ left the room (quit: Ping timeout: 480 seconds).
(11:23:34 AM) cboltz: sounds like we should try to release 4.0 soon (even if not all features make it) and hope that we can get it into SLE15 and the next Debian release
(11:24:12 AM) cboltz: (2.12 is also fine if you want to reserve 4.0 for $new_major_feature_not_ready_yet ;-)
(11:24:19 AM) intrigeri: for me/Debian there's no big hurry, March (max.) would be fine.
(11:24:28 AM) jjohansen: intrigeri: ideally we would do a .1 release a few months after release with .2, .3 etc hopefully having gradually increasing windows
(11:24:52 AM) intrigeri: jjohansen: ack. sorry, I didn't mean to start a discussion about the long term maintenance branch policy.
(11:25:21 AM) tyhicks: intrigeri: I personally think it is a good topic to discuss so maybe we should try to discuss it next meeting
(11:25:31 AM) intrigeri: tyhicks: ack :)
(11:25:32 AM) jjohansen: heh, right. its fine I just wanted to say the last round of releases was not ideal for anyone
(11:25:43 AM) jjohansen: we need to make sure we do better there
(11:25:46 AM) jjohansen: that is all
(11:26:27 AM) jjohansen: right we can discuss the actual detail at the next meeting
(11:26:43 AM) jjohansen: okay so february is what would work for ubuntu
(11:27:24 AM) jjohansen: so I think I would like to target early february for the next major release
(11:27:56 AM) jjohansen: because that seems to work for debian too, and sadly we missed the SLE window already
(11:28:05 AM) cboltz: goldwyn: do you think we could get an exception for SLE? if so, what's the latest date?
(11:28:06 AM) jjohansen: so it won't affect them
(11:29:08 AM) ***cboltz wants to avoid having newer packages in Leap - while it's not a problem for me, it means duplicated work
(11:29:11 AM) jjohansen: cboltz, goldwyn: if you can get an exception, I am willing to cut a 2.12 release with what ever we can get in
(11:29:18 AM) goldwyn: cboltz: we could do a "fast track" if we can prove it's really important.
(11:29:55 AM) cboltz: I'd argue that not having to rebase the json patch on every 2.11 release is important enough ;-)
(11:29:59 AM) intrigeri: jjohansen: I agree with your sumary.
(11:29:59 AM) goldwyn: so, what would be the difference between existing 2.11 and 2.12?
(11:30:18 AM) jjohansen: cboltz, goldwyn: okay look into it, if you can do it we will cut a 2.12 release with the unix rules so they don't have to go into 11.2 which I would prefer anyways
(11:30:40 AM) jjohansen: goldwyn: there are patches on the dev branch that aren't in 11.1
(11:31:22 AM) jjohansen: in particular some of the stuff that cboltz was asking if he could drop into 11.2 would be ideal for a 2.12 release
(11:31:57 AM) jjohansen: so maybe a 2.12 release in november and a 4.0 in February
(11:32:18 AM) jjohansen: I can live with that
(11:32:19 AM) sbeattie: that sounds good to me.
(11:32:48 AM) sarnold: wow november is like next week
(11:32:49 AM) intrigeri: makes sense too.
(11:32:56 AM) jjohansen: yeah
(11:32:59 AM) intrigeri: sarnold: no, November ends in 5 weeks :)
(11:33:02 AM) cboltz: nobody said November 1st ;-)
(11:33:08 AM) sbeattie: sarnold: hush now, let me pretend it is still July
(11:33:09 AM) jjohansen: sarnold: it doesn't have to be the 1st week of november
(11:33:41 AM) jjohansen: alright lets move on, the next topic is the conversion to git
(11:33:50 AM) sarnold: I just went frmo "november yay that's like four months away" to "oh crap november is next week". hehe.
(11:33:55 AM) sarnold: I like intrigeri's interpretation best :)
(11:34:06 AM) jjohansen: sbeattie: where do we stand with this
(11:34:08 AM) jjohansen: :)
(11:34:32 AM) intrigeri: Git \o/
(11:34:52 AM) intrigeri: I assume we have an agreement wrt. moving to Git?
(11:35:17 AM) intrigeri: so I guess the topics is 1. where; 2. when/how/who? 3. how to deal with the MR backlog. correct?
(11:35:28 AM) jjohansen: intrigeri: yes, we are moving to git, its just a matter of when
(11:35:35 AM) sbeattie: jjohansen: I've been keeping the git repo up to date, as of yesterday.
(11:36:16 AM) jjohansen: sbeattie: nice, did you make any progress on some of the issues like translations?
(11:36:18 AM) sbeattie: (the regen process is not super clean if you're trying to follow along)
(11:36:34 AM) intrigeri: I think we need a flag day wrt. the MRs: spend some time together here to merge as much as we can, then migrate to Git and what is not good enough gets a "sorry, fix xyz and please submit a Git branch".
(11:36:37 AM) sbeattie: jjohansen: not really, but I guess I'll just eat the cost of managing that.
(11:36:46 AM) jjohansen: ugh
(11:37:41 AM) sbeattie: meh, I'd rather eat that to give people, particularly outside contributors, a tool they're more familier with 
(11:38:18 AM) jjohansen: okay, so you are just waiting on us to tell you "do it", to make this happen
(11:38:39 AM) sbeattie: intrigeri: sounds sensible, after this meeting, I'll look at the outstanding merge proposals.
(11:38:46 AM) sbeattie: jjohansen: pretty much.
(11:39:10 AM) intrigeri: I think I've already reviewed all the MRs I can handle.
(11:39:13 AM) jjohansen: does anyone have objections to sbeattie switching the apparmor project to git as soon as he can
(11:39:19 AM) intrigeri: sbeattie: do it
(11:39:19 AM) tyhicks: I've previously reviewed sbeattie's git tree and he's already got my "do it" vote
(11:39:41 AM) intrigeri: the sooner I can stop using bzr for exactly *one* project, the better :)
(11:39:45 AM) jjohansen: yep /me too
(11:39:46 AM) sbeattie: intrigeri: no worries, I appreciate what you've already examined.
(11:39:57 AM) intrigeri: (but no pressure, I can survive some more weeks / months if needed)
(11:40:14 AM) tyhicks: please not months :)
(11:40:37 AM) goldwyn: next month? ;)
(11:40:40 AM) sarnold: switching days after john did four releases sounds like a good time to me
(11:41:05 AM) jjohansen: agreed
(11:41:07 AM) sarnold: dunno why but it makes more sense than switching at some random point in the future
(11:41:22 AM) intrigeri: ack
(11:41:47 AM) tyhicks: sbeattie: I'd be willing to devote a several hours tomorrow or friday to help push it over the line - let me know if there's anything for me
(11:41:53 AM) jjohansen: sarnold: because ideally it will give us more time to work on the tooling for the next release
(11:42:26 AM) sbeattie: yes. I still have some work to do to make the release generation bits work with git.
(11:42:39 AM) jjohansen: sbeattie: I haven't heard any objection, so at your convenience please switch us over
(11:42:50 AM) jjohansen: sbeattie: I think its fine if those come after
(11:42:51 AM) sbeattie: ack, thanks!
(11:43:14 AM) jjohansen: Alright moving on to the next topic
(11:43:20 AM) sbeattie: come after> yeah, I'd rather convert and get people going then delay any more than necessary.
(11:43:27 AM) goldwyn: how about cut 2.12, and switch?
(11:43:36 AM) goldwyn: for 4.0 to git..
(11:43:50 AM) jjohansen: goldwyn: when do you want 2.12 to be cut?
(11:43:57 AM) cboltz: small note/wish: when reviewing/merging a MR, please also consider if it needs to be backported, and do that instantly
(11:44:10 AM) jjohansen: I thought cboltz still had a few things he wanted to work on for it
(11:44:12 AM) goldwyn: jjohansen: last month ;)
(11:44:28 AM) goldwyn: jjohansen: as soon as possible.
(11:44:35 AM) cboltz: yes, ideally I want to write support for unix rules in aa-logprof ;-)
(11:45:15 AM) cboltz: goldwyn: would it help to do a 2.12 beta yesterday ;-) with the clear note that 2.12 final will get that aa-logprof addition?
(11:45:33 AM) cboltz: that would allow to test all the other changes now instead of next month
(11:45:41 AM) jjohansen: goldwyn, cboltz: can you figure out what timing you want. If you want 2.12 now, I will cut it tonight and sbeattie can do the git conversion this week
(11:46:18 AM) goldwyn: cboltz: that sounds like a plan.
(11:46:23 AM) jjohansen: if cboltz is going to do some more dev then, it will be at least a couple weeks, and sbeattie can move forward and 2.12 can be the first release from the git
(11:47:32 AM) jjohansen: goldwyn, cboltz: alright I will cut a 2.12 beta tonight, and sbeattie will still do the git conversion later in the week
(11:48:01 AM) intrigeri: sounds good to me: we get Git, suse gets a beta, and cboltz gets time for the stuff he wants in 2.12.
(11:48:32 AM) cboltz: now I only need someone to review the two small patches I sent on sunday ;-)
(11:48:48 AM) jjohansen: back to the next topic which is migrating from launchpad to github or gitlab
(11:48:50 AM) cboltz: (and I need to review goldwyn's pending patches ASAP)
(11:49:08 AM) intrigeri: cboltz:  (maybe some day I'll dive into the Python codebase but for now I'll stick to policy)
(11:49:48 AM) jjohansen: cboltz: you have a few hours, I am talking about my night so say in 12h or so is when I will start to cut the release
(11:49:56 AM) jjohansen: err beta
(11:50:09 AM) cboltz: ok
(11:50:17 AM) intrigeri: so we had this discussion on the ML, and tl;dr is: some prefer Gitlab for ethical/practical reasons (you can move your data out, you didn't offer it to Github forever); some think that Github will get us more pass-by contributions.
(11:51:05 AM) intrigeri: I'm biased (in the 1st category). I wonder how much such pass-by contributions would be in practice. But I can live with both, no veto whatsoever.
(11:51:05 AM) tyhicks: I feel like we have more experience with Github than Gitlab between the AppArmor developers, as well
(11:51:15 AM) intrigeri: that's an important point
(11:51:19 AM) jjohansen: right, so basically if you want to discuss more we can, the conversion isn't happening right away, we are going to baby step to git first
(11:51:37 AM) tyhicks: jdstrand and I have been using GitHub quite a bit lately
(11:51:48 AM) intrigeri: I personally have more experience with gitlab but I'm far from being a core dev (I only do policy stuff so far, and I don't have commit/merge credentials anyway)
(11:52:04 AM) tyhicks: (I've only come across one or two GitLab projects that I needed to collaborate with)
(11:52:08 AM) intrigeri: if you folks prefer github I won't block.
(11:52:18 AM) jjohansen: intrigeri: oh really? do you want them? you more than meet the bar we set
(11:52:26 AM) cboltz: I have experience with Github and (a non-public instance of) Gitlab, so both are fine for me
(11:52:45 AM) intrigeri: jjohansen: hush, that was my secret bonus agenda point.
(11:53:01 AM) intrigeri: jjohansen: (seriously, yes, but let's stay focused on the current topic :)
(11:53:04 AM) jjohansen: the requirement was just basically some contribution, let people get to know you aren't there to cause issues, and show you can do a few patches okay
(11:53:15 AM) tyhicks: cboltz: since you have experience with both, do you have a preference?
(11:53:33 AM) jjohansen: intrigeri: ack, granted , make a request on lp, and I'll approve it
(11:53:54 AM) jjohansen: so the question is do we want to bike shed some more or take a vote today
(11:54:17 AM) intrigeri: I'm curious if anyone has experience with the CI features of either candidate. I have only seen/used Gitlab CI (seems nice) but didn't set it up, and never looked it what github has in this area.
(11:54:21 AM) cboltz: not really - both work for me, and I don't do enough with them to know serious advantages or disadvantages in one of them ;-)
(11:54:27 AM) sbeattie: I don't want to take too long to decide, as that was one of the motivating drivers for converting to git.
(11:54:41 AM) goldwyn: jjohansen: i'd prefer today.
(11:54:45 AM) intrigeri: same.
(11:55:21 AM) intrigeri: does anyone cannot live with one of the candidate? (i.e. the equivalent of veto in consensus-based decision making process)
(11:55:31 AM) intrigeri: s/candidate/candidates/
(11:55:44 AM) tyhicks: no veto from me
(11:55:57 AM) intrigeri: in other words, would someone leave, or significantly slow down contributions, if we picked X vs. Y?
(11:56:13 AM) sbeattie: no veto from me.
(11:56:16 AM) intrigeri: (paid workers don't count, they don't get to choose :P )
(11:56:26 AM) intrigeri: (</joke>)
(11:56:54 AM) tyhicks: :)
(11:57:21 AM) jjohansen: okay, so simple vote: lp, github, gitlab, abstain
(11:57:23 AM) intrigeri: to me it looks like more people here have experience with github. this could be the decisive argument vs. the "own your data" one.
(11:57:43 AM) intrigeri: gitlab (because "keep control on your data")
(11:57:44 AM) sarnold: no veto from me
(11:58:06 AM) sarnold: I certainly have more experience with github but wouldn't say I know it well enough to feel at home
(11:58:07 AM) goldwyn: if need be, you can setup mirroring between two services, using a service or a simple cron job.
(11:58:08 AM) cboltz: abstain - both will work ;-)
(11:58:14 AM) jdstrand: !lp
(11:58:24 AM) jdstrand: that wasn't really an option :P
(11:58:40 AM) intrigeri: pleeease, not lp.
(11:58:54 AM) jdstrand: like tyhicks, I have more experience with github and find it comfortable, but won't veto gitlab
(11:58:54 AM) jjohansen: sure it is, we could choose to stay on lp, I don't think anyone will but we could
(11:58:59 AM) goldwyn: github (more experience on github)
(11:59:14 AM) intrigeri: jdstrand: I count this as a vote for github
(11:59:30 AM) sbeattie: well, okay, let's take that as a first vote: anyone have a vote in favor of staying with lp?
(11:59:40 AM) jdstrand: jjohansen: I meant with your voting choices it wasn't listed. I'm voting against one option but abstaining from the others :)
(12:00:00 PM) jdstrand: like tyhicks said*
(12:00:16 PM) jdstrand: -1 for staying on lp
(12:00:17 PM) intrigeri: ouch, 1h already passed, I got to go. sorry, I'm used to meetings with a fixed 1h max duration, I scheduled other stuff after :/
(12:00:39 PM) jdstrand: intrigeri clearly voted -1 for staying on lp
(12:00:49 PM) sarnold: thanks intrigeri
(12:00:50 PM) jjohansen: intrigeri: you got your vote in, so you can scram
(12:01:07 PM) tyhicks: -1 for staying on lp
(12:01:08 PM) jdstrand: intrigeri: bye and thanks again for all your work! :)
(12:01:18 PM) sbeattie: thanks intrigeri, appreciate you for making the time you did.
(12:01:44 PM) goldwyn: -1 for lp. 
(12:02:11 PM) tyhicks: safe to say that lp is out
(12:02:38 PM) sbeattie: concur.
(12:02:56 PM) intrigeri: before I leave I'll just give my thoughts on some next topics: 1. regular release schedule: we do this at Tails and I love it (main advantage: missing a release is no big deal, you know your stuff will be released soonish anyway; 2. "resuscitate the monthly irc meeting": I won't do a monthly meeting but I would try hard to attend a {bi,tri}-monthly one. 3. " next meeting 20:00 UTC, Tues Nov. 14, 2017?" I can't make it that week, trav
(12:02:56 PM) intrigeri: eling/conference, but I don't have to be here.
(12:03:16 PM) intrigeri: jdstrand, sbeattie: thanks for the warm words :)
(12:04:53 PM) tyhicks: knowing that GitLab actively maintains a GitHub project importer (https://docs.gitlab.com/ee/user/project/import/github.html), I rest easy in voting for GitHub
(12:05:25 PM) jjohansen: gitlab
(12:05:53 PM) jdstrand: tyhicks: oh, so the idea is we actively use github, then setup gitlab to auto-import?
(12:06:45 PM) tyhicks: jdstrand: not quite - I was just saying that we could possibly migrate to GitLab in the future if necessary
(12:07:02 PM) jdstrand: sure
(12:07:11 PM) jdstrand: jjohansen: are you calling for a vote on gitlab?
(12:07:26 PM) jjohansen: jdstrand: no, that is my vote
(12:07:30 PM) jdstrand: ah
(12:08:26 PM) jdstrand: so, I  wonder if we should vote +1/abstain/-1 on gitlab and github and see where we land?
(12:08:45 PM) jdstrand: cause I won't veto gitlab, but would vote for github
(12:08:53 PM) jdstrand: (as an example)
(12:09:04 PM) jdstrand: it sounds like intrigeri was the opposite
(12:09:16 PM) jjohansen: correct
(12:09:20 PM) jdstrand: (won't veto github, but would vote for gitlab)
(12:09:36 PM) goldwyn: github +1 gitlab abstain
(12:09:41 PM) sbeattie: current vote totals that I have: gitlab(+2): intrigeri, jjohansen github (+3): tyhicks, jdstrand, goldwyn abstain: cboltz
(12:09:45 PM) jdstrand: github +1 gitlab abstain
(12:09:54 PM) tyhicks: github +1 gitlab abstain
(12:10:08 PM) sarnold: github abstain gitlab +1
(12:10:26 PM) jjohansen: github abstrain gitlab +1
(12:10:27 PM) jdstrand: oh I think it is down to sbeattie :P
(12:11:23 PM) sbeattie: greeeeaat, the person who hates git with a passion gets to decide.
(12:11:37 PM) jdstrand: you were too slow to vote and too busy tallying
(12:11:40 PM) Talkless: Gitlab is also not-self-hosted?
(12:11:40 PM) jdstrand: :)
(12:11:51 PM) sarnold: Talkless?
(12:12:09 PM) jdstrand: with great tallying comes great responsibility?
(12:12:19 PM) Talkless: as github I mean, free service.
(12:12:21 PM) sbeattie: Talkless: gitlab can either host for you, or lets you self-host.
(12:12:40 PM) tyhicks: Talkless: we're specifically talking about gitlab hosted-for-us in this vote
(12:12:48 PM) sarnold: I'd much rather let someone else host it, but the fact that gitlab software -is- available is encouraging to me
(12:13:04 PM) Talkless: That would be nice to reduce github monopoly :)
(12:13:35 PM) Talkless: I would wote for gitlab if my word would count.
(12:13:52 PM) jdstrand: if sbeattie chooses to abstain, then I would propose going to gitlab since the more senior-to-the-project voters prefer it and there are no vetos against it
(12:14:27 PM) sbeattie: alright, my vote is gitlab +0.5 github abstain, but here's my plan: after I convert to git, I'll start setting up in gitlab and if I hit any blockers, I'll revisit. 
(12:14:29 PM) jdstrand: (assuming that the tally is a tie that is)
(12:14:51 PM) sbeattie: does that work?
(12:15:01 PM) sarnold: sounds great
(12:15:04 PM) jjohansen: sbeattie: certainly if we hit issue we are definitely open to revisiting
(12:15:14 PM) jjohansen: gitlab it is
(12:15:19 PM) jjohansen: thankyou everyone
(12:15:25 PM) sbeattie: yay, a decision!
(12:15:42 PM) tyhicks: sbeattie: so will you even put the git tree on launchpad or will you go straight to a gitlab project that doesn't have bug tracking or anything else set up?
(12:15:53 PM) jdstrand: oh
(12:15:55 PM) jdstrand: hmm
(12:15:55 PM) tyhicks: (I know the git tree is already on launchpad but I'm talking about "announcing")
(12:16:05 PM) goldwyn: I have to leave as well. See you later.
(12:16:10 PM) tyhicks: bye goldwyn 
(12:16:12 PM) jdstrand: iirc, lp does have an import from github capability
(12:16:19 PM) jdstrand: bye goldwyn :)
(12:16:21 PM) sbeattie: goldwyn: thanks!
(12:17:08 PM) jdstrand: I'm not sure if it does with gitlab and I don't know if that makes a difference. I suspect it wouldn't make a difference, even if we chose to continue to use LP as a bug tracker
(12:17:51 PM) Talkless: Bye everyone.
(12:18:00 PM) jdstrand: so, I'm fine with sbeattie's proposal
Talkless tintou tyhicks 
(12:18:05 PM) jjohansen: bye Talkless
(12:18:18 PM) sbeattie: jdstrand: reading https://help.launchpad.net/Code/Git it looks like it can mirror from any publicly visible git repo.
(12:18:39 PM) jdstrand: ah, thanks for that
(12:19:50 PM) tyhicks: what's the reason for wanting git tree mirroring on launchpad?
(12:20:26 PM) jdstrand: tyhicks: I'm not sure there is strictly a reason. I was wondering out loud if that might help with bugs (if we stuck with LP for bugs)
(12:20:41 PM) sbeattie: tyhicks: I will probably push to launchpad initially, just to have an official place in the interim. 
(12:21:04 PM) sbeattie: I don't want to block work while we're in the process of moving.
(12:21:27 PM) tyhicks: sbeattie: ack
(12:21:43 PM) cboltz: I'd tend not to have LP as mirror - if we have it, I'm quite sure we'll also get merge requests on LP, and that means having them spread over two places
(12:22:38 PM) cboltz: (as interim solution, LP is of course ok - even if I wonder if the gitlab setup really takes so much time that we need that)
(12:22:41 PM) jdstrand: so
(12:23:02 PM) jdstrand: I'll put forth one thought that is a main reason why I voted for github
(12:23:16 PM) jdstrand: I said this last meeting but didn't again this time
(12:24:19 PM) jdstrand: while apparmor upstream has contributions from many people, most of the most active contributors currently work for Canonical. Several Canonical projects are now using github and know how to do several things
(12:24:50 PM) jdstrand: so there is mindshare around that and we can easily ask a coworker how they do something or how to fix something
(12:25:08 PM) jdstrand: I'm thinking mostly about "hey, this thing doesn't work for us, but lxd seems to be able to do it" or "hey, snappy does this with CI, we could too"
(12:25:50 PM) tyhicks: I think that's true for asking google, as well, but I think the vote is already final
(12:26:20 PM) jjohansen: yes, if we run into issue we can revisit
(12:26:31 PM) jjohansen: alright can we move on, I'd like to postpone the discussion of release schedules until the next meeting
(12:26:35 PM) jdstrand: if we go gitlab, we'd be mostly on our own (based on the abstains and tepid votes for gitlab, I don't sense a lot of experience within all the apparmor contributors with gitlab. again, I'm not changing my vote or even asking for a change. that was just something that was in the back of my mind
(12:27:17 PM) jjohansen: and I propose we skip the November meeting and have one in December to kibitz about release schedules and meeting schedules
(12:28:26 PM) sbeattie: jjohansen: I think we have a short term plan for releases; if you're suggesting a cadence, I think we can do that, but can postpone the discussion if you want.
(12:28:27 PM) cboltz: given how long the last two meetings were, I'd vote not to skip one ;-)
(12:28:40 PM) jdstrand: +1 to skip november. that puts us in US holiday week any way
(12:29:13 PM) jdstrand: oh Nov 14
(12:29:21 PM) jdstrand: well, that is too soon anyway :P
(12:30:21 PM) jjohansen: alright I have a hard stop, and have to run
(12:30:21 PM) jjohansen: I'll tally the votes when I get back
(12:30:21 PM) jjohansen: thanks everyone
(12:30:31 PM) sarnold: thanks jjohansen!
(12:30:36 PM) jdstrand: thanks jjohansen :)
(12:30:38 PM) sbeattie: jjohansen: doh, thanks!
(12:30:45 PM) tyhicks: thanks jjohansen 
(12:30:55 PM) cboltz: thanks everyone!
``` 
