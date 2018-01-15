```
[19:00:52] <jjohansen1> sarnold, sbeattie, tyhicks, jdstrand, cboltz, goldwyn, intrigeri meeting time
[19:00:58] <sarnold> \o/
[19:01:17] * cboltz hides
[19:01:55] <tyhicks> I've got a conflicting meeting - not sure how much I'll be able to pay attention
[19:02:12] <jjohansen1> oh cboltz, does this mean we get to postpone the meeting again
[19:02:48] <cboltz> no, I was just joking ;-)
[19:02:55] <jjohansen1> tyhicks: np, I think this one is mostly for suse anyways
[19:03:59] * goldwyn is here.. waves hello
[19:04:14] <jjohansen1> hey goldwyn
[19:05:02] <goldwyn> jjohansen1: hi
[19:05:30] <jjohansen1> Okay we have more than two people this time, so lets get started
[19:05:51] <jjohansen1> cboltz: where are you at with the 2.12 release exception
[19:06:16] <cboltz> not where I planned/hoped to be :-(
[19:06:38] <cboltz> some other stuff appeared on my TODO list, and unfortunately it qualified as more important
[19:06:45] <jjohansen1> okay, the question then becomes do we roll a release this/next week or continue waiting
[19:07:10] <jjohansen1> I know goldwyn has expressed the preference of asap
[19:07:17] <goldwyn> cboltz: could you reiterate what you were planning on adding for 2.12?
[19:07:38] <cboltz> I was mostly hoping to get support for unix rules into the tools
[19:07:49] <cboltz> (more than the current "we won't break existing unix rules")
[19:08:41] <jjohansen1> cboltz: well not the biggest loss in that they aren't going to land until 4.16/17 now, and will require an updated parser etc. So its more than just the tools that will have to be updated
[19:08:55] <goldwyn> cboltz: ok, what are you doing for the holidays? ;)
[19:09:08] <jjohansen1> we have to bump the kernel abi to land it, so that old tools will not work with that feature on newer kernesl
[19:09:27] <cboltz> goldwyn: you don't want to see my TODO list ;-)
[19:09:32] * sbeattie is here, had a partially conflicting meeting
[19:09:42] <jjohansen1> cboltz, goldwyn: in light of the above info, should we just roll 2.12
[19:10:03] <jjohansen1> as the parser and tools will not support unix rules either, in the 2.12 release
[19:10:14] <goldwyn> i would vote for yes
[19:10:23] <cboltz> goldwyn: do you know if SUSE plans to backport the unix rule patches to the SLE15 kernel?
[19:10:25] <jjohansen1> sbeattie: sorry
[19:10:48] <jjohansen1> goldwyn: what kernel will SLE15 be?
[19:11:12] <goldwyn> 4.12 based and no, it won't have unix rules
[19:11:19] <jjohansen1> and please note upstream will provide backport patches so, its a matter if you want to pull those patches in
[19:11:52] <cboltz> ok, then I also vote to release 2.12 without unix rule support in the tools
[19:12:00] <goldwyn> yes, but i am not sure if we would have enough time to test.
[19:13:06] <jjohansen1> ack, I will work on getting a 2.12 release out within the next week, it will be interesting because uhm we have never done a release since the transition to gitlab
[19:13:29] <sbeattie> jjohansen1: I'll work on getting the release infra updated
[19:13:48] <sbeattie> at least enough that we can do releases
[19:13:59] <jjohansen1> cboltz: if you could scan the patches that have gone in since 2.12 branched to make sure we have everything that would be helpful
[19:14:11] <jjohansen1> sbeattie: thanks
[19:14:46] <jjohansen1> Okay lets move on, if there anything around the gitlab conversion/move that needs to be brought up?
[19:14:50] <cboltz> as far as I can see, nobody created a 2.12 branch yet, so 2.12 == master
[19:15:14] <jjohansen1> cboltz: oh really? hrmm I was sure I branched it
[19:15:31] <cboltz> git branch -a   disagrees ;-)
[19:16:02] <jjohansen1> it does look that way
[19:16:47] <jjohansen1> cboltz: thanks for being so quick on checking which patches needed to be picked for 2.12
[19:16:51] <jjohansen1> :P
[19:17:06] <cboltz> ;-)
[19:17:20] <cboltz> BTW: for the release notes, http://wiki.apparmor.net/index.php/ReleaseNotes_2_11_95 is probably a good start
[19:17:35] <cboltz> (choose yourself if you copy&paste or if you simply rename the page for the final release)
[19:17:47] <jjohansen1> ack, thanks
[19:17:59] <jjohansen1> back on topic, does anyone have an issue with gitlab that they would like to discuss
[19:18:01] <sbeattie> oh, that reminds me: I converted most of the wiki over to https://gitlab.com/apparmor/apparmor/wikis/home 
[19:19:07] <sbeattie> I haven't put in place the embedded images for some of jjohansen1's documents, and there's some quirks around markdown not being able to include one markdown file into another, but it's 95% there.
[19:19:41] <jjohansen1> sbeattie: when do you think it will ready for people to start editing
[19:20:09] <sbeattie> I'd rather we just cut over now and start editing there, IMO.
[19:20:25] <jjohansen1> or does 95% mean its as close as we are going to get with automatic conversion and the rest is for us to take care of
[19:20:31] <sbeattie> I'll put the 2.11.95 release notes over.
[19:20:35] <sbeattie> jjohansen1: yeah
[19:21:08] <sbeattie> that's as far as we'll get with auto tooling (I used pandoc to do conversions, then some manual cleanup on the result)
[19:21:08] <jjohansen1> alright, so I will point wiki.apparmor.net at it, and we can shutdown the old code hosting
[19:21:17] <jjohansen1> err wiki hosting
[19:21:20] <sbeattie> heh
[19:22:49] <cboltz> since we'll probably break all external links with this move, could we also do some page renames?
[19:23:28] <cboltz> things I have in mind are
[19:23:41] <jjohansen1> cboltz: sure, if you get in there and do them soonish
[19:23:46] <sbeattie> cboltz: absolutely.
[19:23:48] <cboltz> - use yyyy-mm-dd for the IRC logs so that they are sorted in the page list
[19:24:15] <jjohansen1> makes sense
[19:24:15] <cboltz> - use dots instead of underscore for version numbers (for example in the release notes pages)
[19:24:41] <sbeattie> cboltz: yes, though... autogenerating page lists might have to occur outside of gitlab.
[19:25:06] <sbeattie> (but yeah, the wrong way round dates bugged me)
[19:25:31] <cboltz> I was just looking at the "More Pages" button in the right column, and that looks autogenerated ;-)
[19:26:05] <sbeattie> cboltz: it's a matter of how do you do it in markdown
[19:27:12] <sbeattie> the nice thing about the gitlab wiki is that the backing store is a... git tree, so you can make changes locally in your editor of choice, and then just push the tree.
[19:27:53] <cboltz> nice, that should make mass-renaming pages quite easy :-)
[19:27:55] <sarnold> NICE
[19:28:41] <cboltz> does someone want to review my changes, or should I just push them when they are ready?
[19:30:23] *** Joins: spider_ (~spider@103.92.43.168)
[19:30:47] <jjohansen1> cboltz: well we never had a review policy on the wiki, and atm I am not inclined to start one
[19:30:54] <sbeattie> there's an exception to that: images uploaded through the web interface. You can store them in the wiki git and refer to them in docs, it's just if they're uploaded via the web interface, do they get stored elsewhere.
[19:31:02] <jjohansen1> if you feel it needs review feel free to ask for one
[19:31:15] <sbeattie> cboltz: I'd say if you want to ask for a review... ah jjohansen1 is saying the same thing.
[19:31:35] <sbeattie> images> s/do they/they do/
[19:31:40] <cboltz> sounds like I should just push my changes
[19:31:49] <sbeattie> cboltz: yeah.
[19:31:50] <cboltz> and in worst case, we have git to revert them ;-)
[19:31:56] <sbeattie> exactly!
[19:32:38] <sbeattie> though it's a little weird; it's not a first class git tree/project in gitlab, so there's no merge requests, etc.
[19:33:44] <spider_> hi
[19:34:09] <jjohansen1> hi spider_
[19:34:33] <spider_> how are u doing all 
[19:34:36] <jjohansen1> okay, so anything else that anyone one wants to bring up on the gitlab conversion
[19:35:00] <jjohansen1> spider_: well kind of busy we are in the middle of a meeting
[19:35:12] <cboltz> I'd like to have the option to require zero approvals for a merge request
[19:35:26] <cboltz> (but the default should still be to require an approval)
[19:35:58] <jjohansen1> so I don't know if we can do that in gitlab, we will have to mess around with controls some more
[19:35:59] <sbeattie> cboltz: for what things? I'm not opposed for 'trivial fixes
[19:36:15] <sbeattie> also what jjohansen1 said
[19:36:15] <jjohansen1> sbeattie: acked-by: time out
[19:36:20] <cboltz> reasons are a) avoiding paperwork (for cherry-picking with merge requests, for example to adjust the commit message for the merge
[19:36:31] <cboltz> and yes, b) acked-by: timeout
[19:37:02] <sbeattie> yeah, having the cherry-pick make a whole new MR is great for inflating our dev stats but uhhh
[19:37:38] <cboltz> there's the option to cherry-pick instantly (without a merge request)
[19:38:00] <cboltz> but I had one or two cases where I used a merge request
[19:38:27] <cboltz> IIRC once I wanted to adjust the commit message (because I forgot to add the Acked-by in master, and wanted to have it at least in the branches
[19:39:19] <cboltz> just forgetting to uncheck the "create merge request" checkbox while cherry-picking is another reason ;-)
[19:40:15] <spider_> @all once you are done with the gitlab concern let me know i need to ask something 
[19:41:00] <sbeattie> cboltz: okay, jjohansen1 or I will look into seeing what we can do with gitlab controls
[19:41:19] <sbeattie> anything else re: gitlab conversion?
[19:42:16] <cboltz> well, in general, it works quite good, even if I had to learn a few things about git
[19:42:38] <cboltz> ... which maybe I should already have learned years ago ;-)
[19:43:42] <jjohansen1> alright, lets move on
[19:44:21] <jjohansen1> I think we should just bump the meeting schedule, and release schedule discussion until the January meeting
[19:45:01] <jjohansen1> which if we follow the regular pattern would be January 9, 2018 @18:00 UTC (assuming we stick with the new time)
[19:45:59] <sarnold> tyler and jamie may be taking that day/week off
[19:46:10] *** Joins: ToffeeYogurtPots (~ToffeeYog@4JHAAAFZY.tor-irc.dnsbl.oftc.net)
[19:46:32] <cboltz> i'll have another IRC meeting an hour later, so if we stay under an hour, that day/time is fine
[19:46:35] <tyhicks> yeah, that's probably bad timing
[19:47:52] <jjohansen1> alright, that looks bad what about Jan 16
[19:48:51] <tyhicks> Jamie and I will be at a sprint but there's at least a chance that one of us can pop in
[19:49:36] <cboltz> Jan 16 should work
[19:50:29] <sbeattie> either date works for me
[19:51:00] <jjohansen1> Alright lets tentatively go with the 16th
[19:51:40] <goldwyn> I am fine with any date as long as the time is 18:00 UTC or before.
[19:51:53] <jjohansen1> does anyone have anything else they would like to bring up in the meeting (note, not support related Qs which can wait until after the meeting)
[19:52:01] <jjohansen1> goldwyn: ack thanks
[19:52:08] <cboltz> maybe I have something that should go into 2.12
[19:52:15] <cboltz> owner support for file rules
[19:53:02] <cboltz> I sent that patch a while ago
[19:53:45] <cboltz> (where "a while ago" means 2017-07-30 ;-)
[19:53:53] <sarnold> oof
[19:54:00] <jjohansen1> ouch
[19:54:17] <jjohansen1> cboltz: bring it up again, I'll review it today or tomorrow
[19:54:30] <cboltz> back then, one issue was this check:   if event.ouid != 18446744073709551615:  # 2^64 - 1
[19:55:48] <jjohansen1> ermmm, yeah
[19:57:03] <cboltz> my guess is that this number is meant to be -1, but libapparmor and/or the python bindings disagree on signed vs. unsigned int
[19:57:27] <jjohansen1> cboltz: you are going to make me cry
[19:57:40] <sarnold> cboltz: see what you made him do?
[19:57:54] <jjohansen1> I'll spend some time looking into it, but I am not deep diving into swig
[19:58:25] <cboltz> ;-)
[19:58:38] <cboltz> do you see a real-world problem with keeping this number as an exception?
[19:58:40] <sarnold> no kidding, next thing you know he'd be knee-deep into a cffi replacement 
[19:59:32] <jjohansen1> cboltz: atm not really, but give me some time to look at it and think about it more
[20:00:50] <jjohansen1> unless there is something else I think we can end the meeting
[20:01:16] <cboltz> speaking about the real world -
[20:01:21] <cboltz> -rw-r--r-- 1 root root 372G Dez 13 18:59 /var/log/lastlog
[20:02:12] <jjohansen1> cboltz: no, no logs that big are definitely not real
[20:02:26] <jjohansen1> ;-)
[20:02:28] <cboltz> http://www.noah.org/wiki/Lastlog_is_gigantic explains it ;-)
[20:03:04] <cboltz> it's a sparse file and reserves 256 per user id - and the size depends on the biggest user id it has seen
[20:03:12] <jjohansen1> I have a feeling you are going to make me cry again
[20:03:37] <jjohansen1> O_o
[20:03:37] <cboltz> lol
[20:03:47] <sarnold> that's nothing ... a project I've been kicking around would have a sparse file in the "gotta look up what comes after peta-" range
[20:04:07] <cboltz> there's also a good thing on the page about the gigantic lastlog
[20:04:23] <cboltz> it says the max user id is 2^32
[20:04:51] <cboltz> so assuming this is correct, we shouldn't see conflicts with our big exception which is 2^64 - 1
[20:04:52] <jjohansen1> yes, it used to be 2^16
[20:05:00] <jjohansen1> right
[20:06:39] <jjohansen1> meeting adjourned, thanks everyone
[20:06:53] <sbeattie> jjohansen1: thanks!
[20:07:04] <cboltz> thanks everybody!
[20:08:52] <sarnold> thanks!
[20:09:36] <goldwyn> thanks
[20:09:54] <goldwyn> jjohansen1: did you try apparmor + overlayfs?
[20:10:10] <jjohansen1> goldwyn: no I haven't gotten to it yet
[20:10:27] <jjohansen1> blame cboltz, he keeps finding bugs to occupy my time with
[20:10:44] <jjohansen1> spider_: okay what is your Q?
[20:10:57] <goldwyn> ok, no prob
[20:11:37] <jjohansen1> goldwyn: I'll put it ahead of today's cboltz requests
[20:11:57] <sarnold> lol
[20:12:03] <cboltz> luckily I reported the signal kernel issues yesterday ;-)
[20:28:21] <cboltz> jjohansen1: FYI - owner support in logprof is https://gitlab.com/apparmor/apparmor/merge_requests/34
[20:28:58] <cboltz> I'll send another merge request to add a "drop owner conditional" button in aa-logprof over the weekend
[20:29:45] <jjohansen1> thanks
[20:30:11] <cboltz> I'll also check my local profiles for things that should go upstream (IIRC I had to add a few signal rules for dovecot)
[20:30:19] <sarnold> there's something ever so slightly unsettling about changing the existing test cases
[20:31:19] <jjohansen1> depends, changing a xpass/xfail to pass/fail is extremely satisfying
[20:31:38] <sarnold> :D
[20:32:03] <cboltz> do you have the libapparmor version number on your release checklist, or do you want a merge request for it?
[20:32:24] <cboltz> 2.11.95 had a _lower_ number than 2.11.1, and I'd like to avoid that for 2.12 ;-)
[20:33:23] <jjohansen1> cboltz: a merge request wouldn't hurt, it would avoid that accidentally being misse
[20:33:42] <jjohansen1> but checking/bumping it is in the steps for the full release process
[20:33:47] <jjohansen1> so it should happen
[20:34:42] <jjohansen1> mind you those need a little revision because they lead to the whole 2.11.1 higher than 2.12 in the first place
[20:35:36] <jjohansen1> basically we need to make sure branches if they bump the libapparmor version need to get propagated into the main branch
[20:39:46] <cboltz> https://gitlab.com/apparmor/apparmor/merge_requests/35 is for the libapparmor revision
[20:42:07] <cboltz> hmm, the gitlab wiki uses lowercase in the right column page list (but not in the list you get after clicking "More Pages")
[20:42:32] <cboltz> should we ignore that, or rename all pages from CamelCase to a Name_with_underscores?
[20:42:47] <cboltz> ("ignore" could also mean a bugreport to gitlab ;-)
[20:47:51] <jjohansen1> cboltz: hrmm, certainly a bugreport to gitlab, but that doesn't stop us from doing a rename either
[20:48:23] <jjohansen1> I am happy with either so, feel free to do the rename if you want
[20:53:34] <cboltz> IRC logs renamed
[20:53:42] <cboltz> using mmv to mass-rename pages is nice :-)
[20:54:03] <cboltz> and using a regex in vim to adjust the links pointing to all IRC logs is even better
[21:03:57] * sarnold writes down 'cboltz' as the guy who can figure out vim's regex suppor
[21:28:13] <darix> i hate vim's regexp syntax
[22:01:54] <cboltz> sarnold: it was a simple
[22:01:57] <cboltz> :%s/irclog.\([0-9][0-9]\)\.\([0-9][0-9]\)\.\(20[0-9][0-9]\)/IRC_meeting_\3-\2-\1/
[22:02:05] <cboltz> in MeetingAgenda.md
[22:02:08] <sarnold> yes so simple :)
[22:02:43] <cboltz> it only has 3 \(match groups\), so what's your problem?
[22:03:25] <sarnold> my problem is I can never figure out which bits of regex syntax need the \
[22:38:59] <cboltz> hmm, just wondering - would using "gitlab pages" instead of the wiki be a good idea?
[22:39:44] <sarnold> would it be too nuanced to put "polished" stuff in the gitlab pages, and "our ideas" on the wiki?
[22:40:31] <cboltz> I'd call it too confusing ;-) - having the content spread over two places is annoying
[22:40:55] <cboltz> so whatever we do, let's keep everything at one place please
[22:41:00] <jjohansen1> doesn't necessarily have to be spread, the wiki could point to finished documents
[22:41:24] <tyhicks> there are a lot of people that are confused by the wiki referring to unimplemented features
[22:41:47] <sarnold> yes :(
[22:42:23] <cboltz> people will find those pages everywhere
[22:42:48] <cboltz> the fix is to add a clear warning to those pages, not to "hide" them somewhere
[22:43:15] <tyhicks> I don't disagree but I think polished documentation isn't a bad thing, either
[23:40:05] <sbeattie> well, and having clear documentation page(s) for what does exist for the policy language would be helpful, too.
[23:41:13] <cboltz> I won't object, but -
[23:42:00] <cboltz> we already suck  in maintaining the wiki, so we'll suck even more if we spread it over two places (which might include at least a bit duplication and additional work)
[23:43:35] <tyhicks> I personally see the wiki as pretty much hopeless, since it contains so much wrong information, and don't spend time trying to maintain it
[23:44:09] <tyhicks> the man pages are much better maintained and could be the basis for more polished documentation
[23:45:50] * cboltz wonders if it's possible to render gitlab pages from the manpages
[23:48:05] <tyhicks> this should do it (I haven't used it): http://search.cpan.org/~rwstauner/Pod-Markdown-3.005/bin/pod2markdown
[23:48:36] <tyhicks> (looks like it is available in Debian/Ubuntu in the libpod-markdown-perl package)
```
