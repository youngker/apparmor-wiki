```
[19:02:12] <jjohansen> meeting time
[19:02:39] * cboltz hides ;-)
[19:02:44] <jjohansen> cboltz, sarnold, sbeattie, jdstrand1
[19:02:48] <jjohansen> hey cboltz
[19:03:09] <jjohansen> you can always hope that no else shows up :)
[19:03:34] <cboltz> ;-)
[19:04:17] <sbeattie> hey jjohansen 
[19:04:39] <sbeattie> hrm, my calendar has it in an hour.
[19:04:49] <sbeattie> but eh, whatever, I can meet now
[19:05:04] <cboltz> jjohansen: I have an additional topic - the apparmor-profiles repo and your idea to convert it to branches
[19:05:43] <cboltz> sbeattie: daylight saving time change vs. UTC?
[19:06:20] <jjohansen> cboltz: uh thought that was part of the policy reorg on the agenda
[19:06:30] <sbeattie> cboltz: my life is ruled by google calendar...
[19:06:49] <sbeattie> cboltz: but I also stalk jjohansen's calendar, and it's the same time there.
[19:06:53] <cboltz> jjohansen: ok, that will also work ;-)
[19:07:19] <jjohansen> sbeattie: sorry, indeed our calendar has it at 19:00 for some reason, I am assuming DST vs PST
[19:07:50] <jjohansen> well except the email said 18:00 ...
[19:08:36] <cboltz> maybe make it 18:00 UTC in your calender and let google do the timezone dance? ;-)
[19:08:45] <jjohansen> yeah
[19:11:18] <jjohansen> okay, I don't know if we are going to get anyone else today, so lets get started
[19:11:55] <jjohansen> first up is policy
[19:12:22] <jjohansen> does anyone even remember what {/usr,}/etc conversion is about?
[19:12:57] <cboltz> moving default config (not meant to be modified) from /etc/ to /usr/etc/ - and the policy changes needed for this
[19:12:59] <jjohansen> my guess is shipping an empty etc
[19:13:12] <cboltz> right, that's the final goal
[19:15:17] <jjohansen> if /usr/etc/ if for things that shouldn't be changed, then any rule that allows w access should be split into
[19:15:38] <jjohansen>   w /etc ...
[19:15:48] <jjohansen>   r /usr/etc/ ...
[19:16:19] <cboltz> good point, we didn't think about that before
[19:16:19] <sbeattie> fortunately, that shouldn't be many, but yeah, we should have that as a guiding policy principle.
[19:16:22] <jjohansen> the rest of the /etc/ rules that are read only can get the {/usr,}/etc conversion
[19:17:19] <jjohansen> other than that I think unless there is good reason this isn't a change that I really want to see backported
[19:18:09] * sbeattie agrees with that.
[19:18:23] <jjohansen> of course I am waiting for cboltz to say they are using it in 2.12 ...
[19:18:24] <jjohansen> :)
[19:18:44] <cboltz> depends on the release date of 3.0 ;-)  but in general I agree. Right now, only Tumbleweed has some files in /usr/etc/
[19:19:10] <jjohansen> cboltz: okay, we are trying to get 3.0 out in february
[19:19:21] <cboltz> sounds good :-)
[19:19:25] <jjohansen> or I am anyways
[19:19:53] <cboltz> to make things slightly more interesting - the /usr/etc/ stuff was somewhat delayed by MR 212 (which would get lots of merge conflict if we do /usr/etc/ handling)
[19:20:26] <jjohansen> the other end of this, is that apparmor policy itself should probably be using an overlay in 3.0 and its default install location should move from /etc to /usr/etc
[19:20:28] <cboltz> therefore we should finally decide if we accept that MR as is
[19:21:49] <cboltz> I have no idea how aa-logprof will handle that, but I'm quite sure it will result in doing funny[tm] things :-/
[19:22:18] <cboltz> for example, AFAIK it can only handle one profile directory
[19:24:28] <jjohansen> cboltz: I have no objections to MR 212, but it was my understanding it will break logprof and so we wanted to delay doing it until that could be fixed
[19:25:29] <cboltz> no, IIRC I had some doubts especially about @{lib} which looks very broad compared to the current rules
[19:26:01] <cboltz> (if I'm the only one who thinks so, feel free to ignore me and merge it ;-)
[19:26:27] <jjohansen> oh right
[19:26:39] <jjohansen> I do think that could be tightened up so
[19:26:53] <jjohansen> the idea in general, I don't have objections to
[19:27:14] <jjohansen> I need to actually review the patch changes before acking
[19:27:47] <jjohansen> if its done right we should be able to keep things just as tight
[19:29:08] <jjohansen> I will take a look
[19:29:31] <cboltz> ok, thanks
[19:30:03] <jjohansen> cboltz: so your comment about how aa-logprof will handle it, was with regard to the overlay for policy
[19:30:14] <cboltz> right
[19:30:56] <jjohansen> it seems kind of silly for us to transition our policy rules to support /usr/etc/  but not the rest of apparmor
[19:31:13] <cboltz> yes, I know ;-)
[19:32:03] <sbeattie> should we open a tracking issue?
[19:32:18] <cboltz> supporting that is somewhere on my always-growing TODO list, but I'm afraid that it will need quite a while until it reaches the top of that list
[19:32:24] <jjohansen> perhaps, we can make it a switchable config target, if jmbl can get the new build working in time
[19:32:40] <cboltz> no objections against a tracking issue, but I doubt that will speed things up
[19:32:57] <jjohansen> sbeattie: yeah, a tracking issue makes sense
[19:33:25] <jjohansen> so if logprof won't be able to support the overlay, do we not change that for 3.1
[19:33:31] <jjohansen> err 3.0
[19:33:51] <jjohansen> I would still like to support the overlay in the parser in 3.0
[19:34:30] <jjohansen> we could advertise it as available but not ready for default because not everything supports it yet
[19:34:57] <cboltz> I'd guess that logprof will just ignore log events for profiles in /usr/etc/ (because it doesn't find them), therefore nothing really bad should happen
[19:35:04] <cboltz> therefore - no objections
[19:37:40] <jjohansen> tracking issue: https://gitlab.com/apparmor/apparmor/issues/69
[19:37:58] <jjohansen> cboltz: okay, we will circle back before release
[19:38:13] <sbeattie> thanks
[19:38:57] <jjohansen> okay, so lets move on to the profile reorg
[19:39:08] <jjohansen> which has two separate parts to it
[19:39:40] <jjohansen> 1. splitting out policy from the base release. So everything in profiles/
[19:40:35] <jjohansen> 2. moving that into a repo with the other policy (current repo) and reorganizing it so its sane for multiple distros
[19:41:21] <jjohansen> right now its hard for distros to use, there is no reference policy that they can compare against
[19:41:45] <jjohansen> and the distro specific changes are stored as a diff against a reference policy.
[19:42:03] <jjohansen> I have proposed https://gitlab.com/apparmor/apparmor-profiles/merge_requests/40
[19:42:17] <jjohansen> as a potential way of reworking the repo
[19:42:29] <jjohansen> so that there is a single reference policy
[19:42:46] <jjohansen> the idea is that releases get tagged
[19:43:32] <jjohansen> and if distros need to branch from the reference policy a branch is made from the tag they are deviating from
[19:44:02] <sbeattie> I don't want to bikeshed too much, but would the tags stil be ubuntu release based? I'm assuming we would accept and/or encourage other distro release tags/branches?
[19:44:03] <jjohansen> this lets us track where the distro deviates, and gives distros a reference policy they can follow
[19:44:31] <sbeattie> would we do independent releases?
[19:44:46] <jjohansen> sbeattie: so current reorg is entirely ubuntu based only because ubuntu is the only distro in the repo atm
[19:44:48] <sbeattie> or well releases of the tree at all as tarballs?
[19:45:07] <jjohansen> but yes, each distro should feel free to tag and branch as needed
[19:45:32] <jjohansen> its more about making it easy to track the difference
[19:45:49] <sbeattie> OTOH, since there's no (current) need for any preprocessing before release, the gitlab generated tarballs should be fine for people to use.
[19:46:01] <cboltz> your MR looks much better than the current repo layout, but I'm not sure if it will work if we also move the profiles and abstractions from the apparmor repo to it - especially when it comes to $distro-only profiles. I'm somewhat afraid that we'll end up in a branch hell ;-)
[19:46:21] <cboltz> unfortunately I don't have "the solution"[tm] for a perfect layout at hand
[19:46:41] <jjohansen> as for releases. Yes independent releases would be possible, but I think in general the major releases would move in lockstep the base apparmor release
[19:47:48] <cboltz> I'd tend to keep /ubuntu/ (or /$distro/) as real directories, and another /common/ directory with the profiles and abstractions currently living in the apparmor repo
[19:48:11] <jjohansen> cboltz: so I agree the current reorg needs work. There are some decisions to be made. The reorg calls out 3 big ones
[19:48:19] <cboltz> but if you have good arguments why that would be a bad idea, please tell me ;-)
[19:49:06] <jjohansen> the current approach of directories, does not work for change tracking
[19:49:37] <jjohansen> if we want to enable distros to compare and track their changes against a reference policy
[19:50:11] <jjohansen> so that it makes it easier for distros to update their changes a branch and tag based approach is the way to go
[19:50:32] <jjohansen> as for branching hell, that is possible
[19:50:48] <jjohansen> but its no worse than the directory hell we have now
[19:51:08] <jjohansen> and it has the benefit that branches are only made if the distro deviates
[19:51:58] <cboltz> can you give a practical example of the branch layout that includes a) the abstractions from the apparmor repo and b) an ubuntu-only profile (currently /ubuntu/$version)?
[19:51:58] <jjohansen> also distro maintainers only need to checkout the master branch and their own branches, they don't need the other distro branches
[19:53:26] <cboltz> ok, that sounds like having a) in master
[19:53:43] <cboltz> and having an "ubuntu" branch for b) (with sub-branches for $version)
[19:53:45] <cboltz> right?
[19:54:15] <jjohansen> well, I think just ubuntu-XXX branches
[19:55:16] <jjohansen> that way the distro can more easily carry their changes forward against the reference policy, instead of drifting further
[19:55:48] <jjohansen> but I am not against a distro doing what ever it thinks best for its releases
[19:56:43] <cboltz> I have a feeling that distros will have lots of fun with rebasing to get abstraction fixes (but not "big" changes) into stable releases...
[19:57:33] <cboltz> basically the backporting we do now would become the job of the distributions
[19:57:59] <jjohansen> so how I envision it (making up tags completely) is
[19:57:59] <jjohansen> ubuntu-18.04 branches from apparmor-2.13, and adds its sauce
[19:57:59] <jjohansen> ubuntu-20.04 branches from apparmor-3.0 and cherry-picks the changes from ubuntu-18.04 that are still needed, and adds new ones if so desired
[19:58:33] <jjohansen> cboltz: that is already work for distros
[19:58:38] <sbeattie> jjohansen: are you expecting then to have apparmor-X.YY tags/branches?
[19:58:42] <jjohansen> this should simplify that
[19:59:01] <sbeattie> so upstream backporting would happen there?
[19:59:05] <jjohansen> sbeattie: yes the proposal calls for tags and branches
[19:59:18] <cboltz> well, not really - currently, I get abstraction updates "for free" with the maintenance releases we do from the apparmor-2.xx branches ;-)
[19:59:31] <sbeattie> jjohansen: well, right, but I read it as distro tags and branches
[19:59:42] <jjohansen> so we tag a release, and if we do a dot release we create a branch from the tag
[19:59:53] <jjohansen> distros can rebase on the branch
[20:00:05] <jjohansen> ah, yeah that too
[20:00:47] <jjohansen> distros get their own tags and branches. they can rebase those off the reference policy
[20:01:13] <jjohansen> against the whole point is having a reference policy with all the policy together
[20:01:29] <jjohansen> we definitely need reference policy tags and branches
[20:01:48] <jjohansen> if I missed that out of the doc I will make sure to add it
[20:02:18] <cboltz> to verify I understand this right - "reference policy" can mean "master" for tumbleweed, and "apparmor-2.13" for Leap 15.2?
[20:02:25] <sbeattie> there will be some CI work to do, in that we want to make sure that polixy/tools changes can handle the existing reference policy *and* that policy changes are well formed against the existing tools.
[20:02:29] <jjohansen> cboltz: yes
[20:03:09] <cboltz> ok, then I start to agree ;-)
[20:03:11] <jjohansen> sbeattie: yep
[20:03:49] <cboltz> sbeattie: actually we'll also need to adjust "make check" at least for the utils because they assume to find something in ../profiles
[20:04:09] <cboltz> and make check for the profiles because they use the in-tree parser and logprof
[20:04:49] <jjohansen> cboltz: part of the goal here is so we can have more frequent policy releases, separating policy from other changes also makes it easier to see what is changing and will help debian with taking fixes
[20:05:18] <cboltz> agreed on that
[20:05:33] <cboltz> I just wanted to point out the dependencies between the profiles and "everything else"
[20:06:03] <cboltz> especially in the "make check" area
[20:06:34] <jjohansen> cboltz: well again, major releases should be in lockstep. It is the point releases we can run separately
[20:06:48] <sbeattie> cboltz: yes, that's part of what I meant by "tools"
[20:06:50] <jjohansen> oh right for testing
[20:06:54] <jjohansen> yep
[20:07:41] <jjohansen> okay, so that is not a problem that is going to be solved within the next month, so lets make the reorg a longer term issue
[20:07:51] <jjohansen> not something for 3.0
[20:08:07] <cboltz> on the profiles side, the solution is probably to always run with   USE_SYSTEM=1   by default (that is, use /sbin/apparmor_parser and /usr/sbin/aa-logprof)
[20:08:24] <cboltz> + an option to specify the paths to parser and logprof
[20:09:22] <cboltz> on the tools/parser side, carrying a set of test profiles and abstractions might work - maybe simply rename /profiles/ to /test/profiles/ instead of deleting them from the apparmor repo ;-)
[20:09:42] <jjohansen> yeah we could do that
[20:10:31] <sbeattie> hrm, maybe.
[20:10:31] <jjohansen> I think the cross repo testing could potentially move into e2e
[20:11:50] <sbeattie> possibly. pretty sure we can git clone a repo in the CI environment, too.
[20:17:19] <jjohansen> oh, yeah that would work nicely
[20:18:10] <sbeattie> so can we capture important elements of this discussion in the merge request?
[20:18:44] <jjohansen> yes, I will work on getting the MR updated
[20:18:57] <cboltz> sbeattie: I'd say that that means some ascii art showing the proposed branch layout ;-)
[20:20:11] <jjohansen> I second cboltz should implement his excellent idea of ascii art :)
[20:21:21] <cboltz> sounds like a way to verify that I really understood the planned layout ;-)
[20:21:57] <jjohansen> :-)
[20:22:55] <jjohansen> okay we have run 20 minutes over already, lets table this and I will work on updating the MR, and we can carry on discussion there, and if needed revisit in the next meeting
[20:23:38] <jjohansen> is there anything else that needs to be discussed before we end the meeting
[20:24:25] <cboltz> I'd say stable releases - but that's more a reminder than something to discuss ;-)
[20:24:43] <jjohansen> cboltz: on second thought sarnold has been awful quite, maybe we should nominate him, to make sure he understands it ;-)
[20:25:03] <sarnold> noooooooo
[20:25:22] <jjohansen> cboltz: well the topic there was more around setting out a more regular schedule around stable releases
[20:25:28] <jjohansen> I think that can wait
[20:25:42] <cboltz> sarnold: looks like jjohansen knows how to wake up people ;-)
[20:25:48] <jjohansen> sarnold: but, but ascii art!
[20:26:54] <sarnold> I'm sure there's an emacs mode to help with that and we could rope in someone who does emacs..
[20:27:42] <jjohansen> sarnold: oh, great idea! I nominate amurray to implement the emacs mode
[20:27:48] <jjohansen> :-)
[20:27:59] <jjohansen> alright thanks for coming everyone
[20:28:01] <cboltz> sarnold: it's ascii art - you just type it ;-) (in your preferred $EDITOR)
[20:28:04] <jjohansen> meeting adjurned
[20:28:20] <sarnold> thanks jjohansen
[20:28:20] <jmbl> hi sbeattie jjohansen cboltz sarnold. My sincere apologies but I had some higher priority work distract me. I hope to finish it up asap (by Friday) and get back to the automated build work. Again, Sorry about that. 
[20:28:35] <jjohansen> cboltz: stop making it easier than it needs to be :-)
[20:28:43] <sarnold> cboltz: http://ergoemacs.org/emacs/emacs_ascii_diagram.html
[20:29:32] <sbeattie> jmbl: no worries, understood.
[20:29:45] <cboltz> sarnold: what you need is   mkdir -p foo/bar baz ; tree   ;-)
[20:29:48] <jjohansen> jmbl: no rush. the only reason your work got brought up was in the context of we could add a new config option around some changes being discussed
[20:30:12] <jjohansen> it is not something that is needed right now, but your work will be nice to have :)
[20:30:29] <jmbl> ahhh cool. ok. I do very much want to get back to it and make progress.
[20:30:43] <jmbl> haha, i am setting a schedule for myself. :-)
[20:31:20] <jjohansen> jmbl: being swamped with work is completely understandable, we all have stuff we would like to get to :-)
[20:31:45] <jjohansen> personally, sleep and going to the beach are pretty high on my list :-)
[20:32:13] <jmbl> jjohansen, you copied my list!!! :-) :-)
[20:32:40] <tyhicks> what's driving jmbl's build system work?
[20:32:48] <jjohansen> :-)
[20:34:02] <jjohansen> tyhicks: besides its something that has been desired for a long time, and is a way for her to get familiar with the project?
[20:34:03] <tyhicks> I had wanted to do what she's working on to make automated coverity scans possible (as well as just an easier set of instructions to build apparmor userspace)
[20:34:22] <jjohansen> yep, pretty much all that
[20:34:25] <jmbl> tyhicks, oh wow!! 
[20:34:26] <tyhicks> ok, cool
[20:34:52] <jmbl> tyhicks, if you multitasking decreases yo can always work with me on it
[20:35:01] <jmbl> ok.. bad typing... sighhhh
[20:35:16] <jjohansen> tyhicks: jmbl was looking for something she could do on the side, it seemed like a good one that wouldn't block other work and would help her get familiar with the project
[20:35:28] <tyhicks> jjohansen: I agree
[20:36:03] <tyhicks> jmbl: you said that you're going to be done by friday so I doubt I'll free up before then (but I can be a reviewer next week)
[20:37:02] <jmbl> tyhicks, cool... but please always feel free to work on it... my concern is i might be a bit slow because of the multitasking.
[20:37:34] <jmbl> also do let me know any requirements you would like.
[20:38:07] <tyhicks> jmbl: it has been on my todo list for about 5 years now so you still have a ways to go before I can call you slow
[20:38:10] <tyhicks> ;)
[20:38:17] <jmbl> tyhicks, by done, i meant done with th higher priority tasks...
[20:38:27] <tyhicks> oh, I misunderstood
[20:38:28] <tyhicks> ok
[20:39:08] <jmbl> tyhicks, rofl, hopefully it wont take me that long :-) :-)
[21:06:14] <cboltz> some mkdir commands later, there's a nice comment on https://gitlab.com/apparmor/apparmor-profiles/merge_requests/40 ;-)
[21:16:20] <sarnold> thanks cboltz :)
[21:25:34] <cboltz> I'm looking forward for comments and answers to my questions ;-)
```
