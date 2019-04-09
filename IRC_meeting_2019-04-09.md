 ```
(11:02:44 AM) jjohansen: meeting time
(11:02:59 AM) ericchiang: present :)
(11:03:06 AM) jdstrand_ is now known as jdstrand
(11:03:07 AM) sarnold: wow ;)
(11:03:37 AM) cboltz: I'm here, but I'll probably have to leave for dinner in the middle of the meeting
(11:05:00 AM) jjohansen: cboltz: sorry, would it be better to move meetings back to 21:00?
(11:05:58 AM) cboltz: for me later is better ;-)
(11:06:14 AM) jjohansen: understandable :)
(11:06:47 AM) jjohansen: I can't remember why/for who we moved the meetings to 18:00
(11:06:51 AM) ***sbeattie is here
(11:07:20 AM) sarnold: 2100 hits when I often eat lunch anyway :)
Sargun sarnold 
(11:08:36 AM) jjohansen: sarnold: I pretty sure that's a reason to have the meeting then ;-)
(11:09:08 AM) sarnold: :
(11:09:24 AM) sarnold: :) . sigh. I might have to face the fact that this keyboard isn't doing great
(11:10:00 AM) jjohansen: okay, its been 10 minutes and I guess we have enough people. Lets get started
(11:10:54 AM) jjohansen: First item on the agenda, is moving kernel dev to gitlab
(11:11:25 AM) jjohansen: the goal being to open up dev more and allow more people to directly commit
(11:11:48 AM) ericchiang: so I've gone ahead and created a kunit branch using their main branch
(11:12:02 AM) ericchiang: msalvatore also submitted his kunit patches https://gitlab.com/apparmor/apparmor-kernel/merge_requests/3
(11:12:57 AM) jjohansen: yes, thank you for that
(11:13:52 AM) jjohansen: commits/merges to apparmor-next are going to require an ack/approval of another kernel dev
(11:14:38 AM) jjohansen: and commits/merges need to be done via the cmd line, and should be rebased to apparmor-next when possible
(11:14:50 AM) msalvatore: ericchiang: I expect to address your comments tomorrow or Thursday.
(11:15:23 AM) jjohansen: other branches like the kunit branch ericchiang just mentioned don't have to be so strict
(11:15:30 AM) Talkless left the room (quit: Quit: Konversation terminated!).
(11:15:57 AM) jjohansen: the goal for apparmor-next is to avoid rebasing if possible
(11:16:08 AM) Talkless [~Talkless@hst-227-49.splius.lt] entered the room.
(11:16:15 AM) jjohansen: does anyone have concerns about the plan
(11:16:16 AM) sarnold: hmm, I would have expected all the kernel-related trees to require the same level of hygiene
Sargun sarnold 
(11:17:39 AM) jjohansen: sarnold: its good to have that level of hygiene but, if it serves development we are a small enough team that rebasing can be worth using some times
(11:17:59 AM) jjohansen: I don't want to exclude that option, from people
(11:18:32 AM) jjohansen: apparmor-next which is the basis for what flows into linus is going to need strict hygiene
(11:18:51 AM) sbeattie: is this a different workflow than the main userspace repo?
(11:19:14 AM) jjohansen: yes
(11:19:51 AM) jjohansen: at least a little, I have already set the apparmor-kernel tree to be rebase only
(11:21:02 AM) jjohansen: I can't force commits/merges to be not done through the gui, but this at least forces people to make sure there tree is updated before committing
(11:21:34 AM) jjohansen: This will help keep the kernel history cleaner
(11:22:02 AM) msalvatore: so, continuing the hygiene metaphor, a kernel developer would have to "cleanse" anything that got merged into the apparmor-next branch (and maybe some others)?
(11:22:03 AM) jjohansen: and we will strictly enforce the rules around signed-off-by, acked-by, ...
(11:23:00 AM) jjohansen: msalvatore: yes, the branch being merged needs to be "cleansed"
(11:23:58 AM) jjohansen: I also don't have problems with the bug fix ack commit from known kernel devs
(11:24:41 AM) jjohansen: so eg. ericchiang could commit a fix with an ack from mjg59
(11:27:20 AM) jjohansen: I haven't seen any objections, so I will send out an email to the list, and move apparmor-next over this week
(11:27:28 AM) msalvatore: jjohansen: within this team, will the preferred method for submitting a patch be 1) via e-mail or 2) branch and merge request?
(11:28:37 AM) jjohansen: msalvatore: either works for me, we have left both methods open for userspace, and I don't have a preference
(11:29:31 AM) jjohansen: if it becomes an issue we can revisit, and choose the preferred method
(11:29:32 AM) sbeattie: jjohansen: no objection from me, but there might need to be a wiki page no one reads to document the rules around this repo.
(11:30:17 AM) jjohansen: sbeattie: oh, good idea. Another page we can forget about and be surprised by when we rediscover it
(11:30:22 AM) jjohansen: :)
(11:31:02 AM) jjohansen: yeah, I create one before sending out the email, make sure they have the same basic content, etc
(11:31:46 AM) jjohansen: msalvatore: atm, like the userspace, I'd like to keep the friction for patch submission to a minimum
(11:32:29 AM) jjohansen: that does mean the person committing a patch might have to do some minor cleanup
(11:33:09 AM) jjohansen: but that is generally preferred by people over being told to resubmit after adding a comma ...
(11:33:45 AM) msalvatore: is there a way within the merge request to address small comments like that without force pushing?
(11:34:41 AM) msalvatore: other than a additional commit that just adds a comma?
(11:35:24 AM) ***cboltz leaves for a while
(11:35:28 AM) jjohansen: msalvatore: sure, you can note in the commit message you did a cleanup
(11:35:42 AM) sarnold: cboltz: guten apetite :)
(11:35:59 AM) jjohansen: I should be clear you can't do this to someone elses merge request from the gui
(11:36:27 AM) jjohansen: you pull their merge request in to your local tree, and merge and push from there
(11:37:06 AM) jjohansen: this does necessitate, adding a note in gitlab to the merge request and manually closing it
(11:37:32 AM) jjohansen: I have been doing this for some userspace requests
(11:38:41 AM) jjohansen: it would be nice if there was a way to tell gitlab a given commit is a merge of a particular request, but it won't pick it up automatically unless
(11:39:16 AM) jjohansen: the original submitter updates their merge request and repushes
(11:39:36 AM) jjohansen: this means they have to add the acked-by's etc
(11:40:17 AM) jjohansen: so like I said, doing it manually from the command line is going to be preferred
(11:40:27 AM) jjohansen: if you aren't sure, poke me
(11:40:42 AM) jjohansen: okay, lets move on to the next topic
(11:40:56 AM) jjohansen: its getting time for another round of stable releases
(11:41:54 AM) jjohansen: is there any bugs that you know need addressing before we roll out a new set
(11:42:59 AM) jjohansen: I am trying to finish up with https://bugs.launchpad.net/apparmor/+bug/1597017
(11:43:53 AM) jdstrand: I'm interested in https://bugs.launchpad.net/ubuntu/+source/apparmor/+bug/1820068
(11:44:03 AM) jdstrand: specifying -O no-expr-simplify results in cache miss
(11:44:30 AM) jjohansen: ah right, I kind of expected that :)
(11:44:34 AM) jdstrand: this is something I'm willing to look at perhaps late this week/next week
(11:44:41 AM) jdstrand: if that is helpful
(11:45:01 AM) jjohansen: jdstrand: yes, it would be
(11:45:22 AM) jdstrand: I'll pencil it in and escalate if I run into trouble/can't get to it
(11:45:59 AM) jjohansen: thanks, I will also try to get to it, but only after finishing up with the mount bug
(11:47:00 AM) jdstrand: I'll also say, with a few of the changes for the testsuite and couple patches (all upstream), 2.13.2 in Ubuntu 19.04 has done well
(11:47:16 AM) jjohansen: nice
(11:47:49 AM) jdstrand: we had a couple bugs, but they were all packaging and not apparmor itself
(11:48:06 AM) jjohansen: jdstrand: how big is the ubuntu delta now? I haven't looked at ubuntu's 2.13 yet
(11:48:33 AM) jdstrand: jjohansen: significantly smaller, still not 'small'
(11:48:52 AM) jjohansen: I would like to try to really get that down for 19.10 if we can
(11:48:54 AM) jdstrand: debian and ubuntu both have their patches. we share almost all of them
(11:49:02 AM) jdstrand: yeah
(11:49:23 AM) jdstrand: jjohansen: that might need to be raised to amurray or joe if I'm to work on it
(11:49:24 AM) jjohansen: okay, we will have to work with intrigeri and talkless
(11:49:42 AM) jjohansen: jdstrand: yeah, I will bring it up with them
(11:50:50 AM) jjohansen: okay, I don't see any other bugs to hold up the stable releases so lets move on
(11:51:04 AM) jjohansen: the apparmor 3.0 release is seriously late
(11:51:34 AM) jjohansen: I've had some major distractions, and just haven't been able to finish it up
(11:51:53 AM) jjohansen: I am now targeting the end of month
(11:52:06 AM) jjohansen: any and all help will be appreciated
(11:52:09 AM) jdstrand: looking at the series file, it really isn't too bad. 19 patches total, like 16 small policy updates. 
(11:52:22 AM) jdstrand: 19 beyond the things we cherrypicked that is
(11:52:31 AM) jjohansen: with that said, is there anything feature wise that we need to consider
(11:52:54 AM) jdstrand: so that isn't bad. the main thing is the systemd unit, and it is *way* better than before
(11:53:15 AM) sarnold: I think the bugs you're working on already are enough, no need to add more things to this
(11:53:34 AM) jjohansen: jdstrand: oh? we should make sure we look through those 19 patches and see how many we can get into 3.0
Sargun sarnold 
(11:54:13 AM) jdstrand: most are distro-y
(11:54:15 AM) jjohansen: sarnold: those bugs are holding up the stable releases, there are a few out standing merge requests for features etc ...
(11:54:21 AM) jdstrand: but there are some opportunities there
(11:54:26 AM) jjohansen: jdstrand: ack
(11:54:53 AM) ericchiang: I'll also dust off my patches to allow profiles to target IMA and EVM xattrs
(11:55:20 AM) jjohansen: ericchiang: yes, I would like to see that go in as well
(11:55:41 AM) jjohansen: unfortunately the secmark stuff isn't going to make 3.0
(11:55:43 AM) jdstrand: we are pretty much using rc.apparmor.functions from upstream (we have some cherrypicks). so, yeah, great progress. intregeri spearheaded that work, so big thanks there
(11:56:14 AM) ***jdstrand is done answering jjohansen's question :)
(11:56:16 AM) jjohansen: yes, he has done some good work there
(11:56:19 AM) jjohansen: :)
(11:56:55 AM) jjohansen: ericchiang: do have any problems with the time line? it gives us about 3 weeks
(11:57:23 AM) ***cboltz is back
(11:57:26 AM) ericchiang: no, I can re-submit everything today/tomorrow
(11:58:36 AM) jjohansen: I suppose I should note, we still plan on getting a 3.1 out the door in the fall, and would rather instead of delaying and pushing it back, have it be a small release of what ever we have managed to get in
(11:58:38 AM) jjohansen: thanks
(11:58:58 AM) jjohansen: cboltz: anything you need/want to get into 3.0 by the end of the month?
(11:59:33 AM) jdstrand: jjohansen: for your conversation with amurray and joe: note, the big 2.13 push in Ubuntu was to prepare for 3.0 in 19.10
(11:59:44 AM) jjohansen: cboltz: also stable release bugs you need (in case you missed it)
(11:59:56 AM) jjohansen: jdstrand: ack
(12:00:28 PM) cboltz: for stable releases, I don't have something that would block them
(12:00:44 PM) cboltz: for 3.0, https://gitlab.com/apparmor/apparmor/merge_requests/254 would be nice
(12:01:31 PM) cboltz: does the proposed syntax   # LOGPROF-SUGGEST: yes   look good for everybody?
(12:01:57 PM) cboltz: does it make sense only to propose abstractions that have that 'yes'?
(12:02:05 PM) sarnold: I think yes and yes
(12:02:14 PM) cboltz: and finally, which abstractions should / shouldn't be proposed?
(12:03:03 PM) jdstrand: fyi, I like that approach and would have opinions on what to not suggest
(12:03:12 PM) jdstrand: but I don't have those opinions formed right now
(12:03:22 PM) ***ericchiang stepping out, thanks yall
(12:03:37 PM) jjohansen: by ericchiang, thanks for showing up
(12:03:39 PM) sarnold: bye ericchiang
(12:03:41 PM) sbeattie: bye ericchiang 
(12:03:43 PM) cboltz: jdstrand: you can answer in the MR or write to the mailinglist ;-)
(12:04:09 PM) jjohansen: cboltz: hrmmm, I would like to see some kind of priority level option
(12:04:28 PM) jjohansen: which the tool could use to weight between different suggestions
(12:05:16 PM) jjohansen: that could also be used to black list certain items
(12:05:16 PM) sarnold: the main goal is to remove some entirely
(12:05:22 PM) jjohansen: right
(12:05:27 PM) jjohansen: suggest: never
(12:05:30 PM) sarnold: maaaybe weighting makes some sense..
(12:05:34 PM) jjohansen: suggest: -1
(12:05:43 PM) jjohansen: or something like that
(12:06:29 PM) jdstrand: levels would be good. I'd prefer the levels be defined and a first pass with something to review
(12:06:53 PM) jdstrand: I mean, I can say we should only suggest dbus-session-strict and not dbus-session
(12:06:54 PM) cboltz: I can see why you want that, but I'm not sure a) if it makes too much sense (any real-world examples?) and b) if we can express it as hardcoded rule (think about indirect include vs. only including the small abstraction)
(12:07:08 PM) jdstrand: but others will be more difficult
(12:08:07 PM) jjohansen: cboltz: really world example, is don't suggest
(12:08:16 PM) earthundead [~earthunde@188.170.72.118] entered the room.
(12:08:29 PM) cboltz: right, that's we can cover with yes/no
(12:08:35 PM) jjohansen: I think the level should be optional, so maybe it doesn't even get used at first
(12:08:40 PM) sbeattie: I mean, if we're talking wishlists here, it be nice to have meta-info recording what the abstraction is supposed to cover.
(12:08:53 PM) jjohansen: but you are always going to run into, A should be suggested over B
(12:09:03 PM) jjohansen: and this provides a mechanism to do it
(12:09:12 PM) sbeattie: the utils could then display or be prompted to display the explanation.
(12:09:31 PM) jdstrand: sbeattie: yeah. there might be several overlapping rules in the abstractions. knowing which to pick would certainly be handy
(12:09:39 PM) jjohansen: sbeattie: yes please!
(12:09:44 PM) Talkless left the room (quit: Quit: Konversation terminated!).
(12:09:45 PM) sbeattie: (e.g. explaining the difference between dbus-session-strict and dbus-session)
(12:09:54 PM) cboltz: sbeattie: I'll implement it in the tools as soon as you added the description to all abstractions ;-)
(12:10:05 PM) jdstrand: sbeattie: hey, I was typing that!
(12:10:19 PM) sbeattie: thanks for reminding me to keep my ideas to myself.
(12:10:31 PM) cboltz: lol
(12:10:43 PM) sarnold: sbeattie: yes please!
(12:13:08 PM) jjohansen: cboltz: so uhmm, I'm not sure what this means for the merge request besides, there are a lot of additional feature requests :)
(12:13:39 PM) jjohansen: I do think providing an optional priority note is worth doing
(12:13:54 PM) jjohansen: even if atm, its just to support do not suggest
(12:13:59 PM) cboltz: ok, so let me ask in another way:
(12:14:00 PM) jjohansen: suggest: never
(12:14:09 PM) cboltz: does someone see a problem with starting with yes/no
(12:14:15 PM) jjohansen: or something like that
(12:14:31 PM) cboltz: and later allow other values like "maybe", numbers, whatever
(12:14:33 PM) cboltz: ?
(12:15:04 PM) cboltz: staying backward compatible with yes/no (and mapping them to a default priority) should be easy ;-)
(12:15:15 PM) jjohansen: cboltz: yes/no as in yes this can be merged without levels?
(12:15:29 PM) sarnold: if we want priorities I think I'd rather start with that and not have booleans *and* levels at once
(12:15:38 PM) cboltz: yes/no as in   # LOGPROF-SUGGEST: yes/no
(12:16:00 PM) jjohansen: yeah
(12:16:06 PM) cboltz: sarnold: which syntax would you propose?
Sargun sarnold 
(12:16:11 PM) jjohansen: err, yeah to sarnold comment
(12:16:26 PM) jjohansen: though I could get behind, just no
(12:16:26 PM) sarnold: one moment, door
(12:16:28 PM) jjohansen: that is
(12:16:35 PM) jjohansen: SUGGEST: no
(12:16:42 PM) jjohansen: to not suggest
(12:16:55 PM) jjohansen: and leave it blank
(12:16:59 PM) jjohansen: SUGGEST:
(12:17:14 PM) jjohansen: or just not have the comment to get logprof to suggest it
(12:17:34 PM) jjohansen: that is I think supporting the negative case, is the important one atm
(12:17:48 PM) jjohansen: I don't particularly like
(12:17:54 PM) jjohansen: SUGGEST: yes
(12:18:00 PM) sarnold: I think trying to coordinate priorities amongst all the abstractions might be difficult, and ones without the tagging wouldn't be obvious if they ought to be at the top of the list or bottom of the list
(12:18:07 PM) jjohansen: we already have that when we don't have the comment
Sargun sarnold 
(12:18:44 PM) jjohansen: sarnold: of course its difficult
(12:19:00 PM) sarnold: we could give some rough guidelines like the severities.db but I think it'd never be perfect.. but there are some abstractions that we *really* don't want shown to users. that feels like a good goal.
(12:19:17 PM) jjohansen: its a kludge to deal with things when you notice a real problem in what is being suggested
Sargun sarnold 
(12:19:43 PM) sarnold: so I'm leaning slightly towards just the yes/no approach, but wouldn't want to nak a priorities approach
(12:19:47 PM) earthundead left the room (quit: Remote host closed the connection).
(12:19:48 PM) jjohansen: sarnold: exactly what I was thinking, something like the severitydb
(12:20:11 PM) jjohansen: but like I said, priorities could come later
(12:20:20 PM) jjohansen: suggest: no
(12:20:24 PM) jjohansen: is needed now
(12:20:29 PM) jjohansen: suggest: yes
(12:20:54 PM) jjohansen: is implicit currently and unneeded, unless there is some priority level
(12:22:27 PM) jjohansen: cboltz: so I think the conclusion is yes we want it, and we can argue about the semanitcs/syntax more later
(12:22:33 PM) jjohansen: lets move on
(12:22:34 PM) cboltz: ok, so I'll change the code to check for a 'no'
(12:23:03 PM) jjohansen: is there any other bugs/merge requests to target for the 3.0 release?
(12:24:40 PM) jjohansen: okay next topic - gitlab releases
(12:25:11 PM) jjohansen: we currently don't support gitlab releases, it only works through an api
(12:25:26 PM) jjohansen: which means we need to update the release tooling to support it
(12:25:45 PM) jjohansen: anyone who wants to look into doing the work to support it?
(12:26:56 PM) sarnold: yeah I can give it a look
(12:27:10 PM) jjohansen: oh nice, thanks sarnold
(12:27:34 PM) jjohansen: no rush on it, I don't expect it to be ready for 3.0 or anything
(12:27:52 PM) jjohansen: we have just had questions around supporting it, and it would be nice to have
(12:28:16 PM) jjohansen: alright does anybody have anything else they want to discuss
(12:32:01 PM) jjohansen: I'll take that as a no :)
(12:32:05 PM) jjohansen: meeting adjourned
(12:32:08 PM) jjohansen: thanks everyone
 ```