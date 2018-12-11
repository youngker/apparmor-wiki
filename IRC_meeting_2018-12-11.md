
(10:01:39 AM) cboltz: meeting time ;-)
(10:04:24 AM) cboltz: jjohansen1, kees, sbeattie, sarnold, Talkless - where do you hide? ;-)
(10:04:46 AM) ericchiang: morning/afternoon/evening everyone
(10:04:50 AM) jjohansen1: cboltz: preferably where no one can find me
(10:05:04 AM) cboltz: lol
(10:05:11 AM) jjohansen1: cboltz: sorry I just didn't realize it was that time already
(10:06:25 AM) cboltz: sounds like the early version of "christmas always happens suddenly" ;-)
(10:06:26 AM) jjohansen1: hey ericchiang
(10:06:45 AM) sbeattie: hola
(10:06:58 AM) cboltz: jjohansen1: thanks for reviewing my merge requests - my git forest looks clearer again :-)
(10:07:01 AM) sbeattie: (for some reason, I thought the meeting was later, too)
(10:07:23 AM) jjohansen1: its that who dst shift
(10:08:26 AM) jjohansen1: 18:00 was 11:00 am for us not that long ago, and meetings used to be at 20:00 until we moved them to 18:00 because it was easier for some people to attend
(10:08:36 AM) jjohansen1: people who aren't here ...
(10:08:37 AM) jjohansen1: :)
(10:10:40 AM) jjohansen1: we don't have a quorum but I don't think we have anything that needs voting on so lets roll
(10:11:19 AM) jjohansen1: there are two items on the agenda, the next stable releases, and the apparmor 3
(10:12:24 AM) jjohansen1: for stable releases, anyone have anything outstanding that needs to go in?
(10:12:36 AM) jjohansen1: I'd like to roll the next set this weekend
(10:12:44 AM) cboltz: I have one thing
(10:12:54 AM) cboltz: we backported naming profiles to 2.12 and 2.13
(10:13:10 AM) jjohansen1: right
(10:13:17 AM) cboltz: and at least for dnsmasq this breaks libvirtd (which is maintained by libvirtd maintainers)
(10:13:44 AM) cboltz: therefore I propose to revert the name at least for dnsmasq in 2.12 and 2.13
(10:14:31 AM) jjohansen1: I am fine with that
(10:14:39 AM) cboltz: I also asked the openSUSE libvirtd maintainer to update the profile for future libvirtd releases - https://bugzilla.opensuse.org/show_bug.cgi?id=1118952
(10:14:39 AM) sbeattie: right, that's probably a bad change to make in a stable branch, it could be considered an abi breakage. Though, going forward, libvirt is possibly going to deal with both profile names anyhow.
(10:14:55 AM) jjohansen1: the only reason the name change was backported was trying to ease profile maintenance
(10:15:35 AM) jjohansen1: sbeattie: is right that it was a bad change to make for stable branches
(10:15:50 AM) jjohansen1: we seem to have been backporting way to much
(10:16:01 AM) cboltz: the {bin,sbin} alternations were another reason
(10:16:14 AM) jjohansen1: and need to have a discussion around what goes into stable releases
(10:16:15 AM) cboltz: (on the positive side, these alternations don't seem to break peer=)
(10:17:04 AM) jjohansen1: cboltz: yes, that is true, but since we already had that issue we could have selectively dealt with it
(10:17:57 AM) jjohansen1: regardless its done, and I don't think reverting the whole lot is worth doing
(10:18:19 AM) jjohansen1: I do think selective reverts like the libvirt case, makes sense
(10:18:57 AM) cboltz: right, it only breaks profiles mentioned in peer=... of other profiles
(10:19:07 AM) cboltz: AFAIK dnsmasq / libvirtd is the only problematic one
(10:19:38 AM) jjohansen1: the other issue is I don't know if we can have the what goes in stable discussion without intrigeri or at least a debian representative
(10:20:19 AM) jjohansen1: cboltz: ack, so I think with the revert we are good
(10:20:56 AM) jjohansen1: cboltz: we also need to consider Talkless's outstanding merge requests for the stable releases, since that is what they are proposed for
(10:20:58 AM) sbeattie: yes, ack on the dnsmasq revert.
(10:22:14 AM) sbeattie: but even {bin,sbin} changes wrt peer= have to be done carefully, due to what gets treated as a regex and what gets treated as a profile name.
(10:22:38 AM) jjohansen1: right, names need escaping etc, its a complete pita
(10:23:03 AM) cboltz: I tested, and {bin,sbin} works without problems even if the libvirtd profile only has peer=.../sbin/...
(10:23:48 AM) cboltz: https://gitlab.com/apparmor/apparmor/merge_requests/290 <-- dnsmasq name revert
(10:25:08 AM) jjohansen1: okay, cboltz can you get the revert proposed, and I will look at it asap
(10:25:16 AM) jdstrand: jjohansen1: wrt apparmor 3, my only comment is it would be nice to have sometime soonish after af_unix is upstreamed so people can start toaking advantage of all the upstream work
(10:25:29 AM) cboltz: jjohansen1: look up one line ;-)
(10:25:41 AM) jdstrand: jjohansen1: I don't know how that fits with timing or priorities or anything, but that's my only feedback
(10:26:11 AM) jjohansen1: jdstrand: ack, I'll answer that in a minute
(10:27:27 AM) jjohansen1: alright anything else about the stable releases?
(10:28:28 AM) sbeattie: which uh branches would we release for?
(10:28:44 AM) cboltz: ideally 2.10..2.13
(10:28:58 AM) jjohansen1: yeah 2.10..2.13
(10:29:06 AM) cboltz: 2.12 and 2.13 are the most important ones because the last minor releases had regressions
(10:29:10 AM) jjohansen1: 2.10, and 2.11 didn't get a release last time
(10:29:28 AM) sbeattie: okay
(10:29:29 AM) jjohansen1: and there is a higher priority fix in 2.12, 2.13
(10:29:55 AM) jjohansen1: thankfully most of the work is done for 2.10, and 2.11
(10:30:16 AM) jjohansen1: and 2.12 and 2.13 are smaller because they saw recent releases
(10:31:10 AM) jjohansen1: anything else or can we move on to apparmor 3?
(10:31:30 AM) cboltz: I'd say we can move on
(10:31:58 AM) sbeattie: let's move on
(10:33:18 AM) jjohansen1: okay, so we still have a lot of outstanding WI
(10:33:23 AM) jjohansen1: http://paste.opensuse.org/view//83156820
(10:33:56 AM) jjohansen1: I split out what I thought I could get away with into an apparmor 3.1 release schedule
(10:34:11 AM) sarnold: where would pam_apparmor using change_profile rather than change_hat fit?
(10:34:40 AM) jjohansen1: oh, is that still in there? that can go to 3.1 :)
(10:35:05 AM) sarnold: it was a feature request from the last debconf, and I'd feel pretty silly if I forgot it until the *next* debconf :(
(10:35:14 AM) sarnold: but if you won't mind 3.1 that's fine then, heh
(10:36:12 AM) cboltz: with "allow all", you mean "file, capability, network, dbus, ..." as one rule?
(10:36:56 AM) jjohansen1: ftr the apparmor 3.1 WI list http://paste.opensuse.org/view//10385421
Sargun sarnold 
(10:37:42 AM) sarnold: thanks
(10:38:16 AM) jjohansen1: sarnold: so yes pam_apparmor really needs an update, especially around user namespaces (the scopes and view work), but since that hasn't landed upstream yet, there is no point holding off 3.0 longer, it can wait for the next release
(10:38:34 AM) jjohansen1: cboltz: yes, it has been requested a lot
(10:38:44 AM) sarnold: I was just worried that you might think it too big a change for a 3.1. :)
Sargun sarnold 
(10:39:16 AM) jdstrand: jjohansen1: I don't see af_unix in there. is that implied by networking in 3.0?
(10:39:19 AM) jjohansen1: sarnold: while I'd like it in 3.0, its not a fundamental change to policy
(10:39:40 AM) cboltz: for the "allow all" syntax, can we please use   I am totally insane,   ? ;-)
(10:39:50 AM) ***jdstrand understands af_unix isn't upstream just yet, but its so close...
(10:39:56 AM) jdstrand: it's
(10:40:03 AM) jjohansen1: jdstrand: so maybe?
(10:41:04 AM) ericchiang: sorry since I'm new, is the plan for apparmor 3 to be the next release after 2.13?
(10:41:08 AM) jdstrand: jjohansen1: how far out is 3.1 from 3.0? if af_unix didn't make it in 3.0, would it be backport to 3.0.x?
(10:41:09 AM) jjohansen1: cboltz: it has a place, its basically giving a name to unconfined, with maybe a select black list, which may or may not be insane depending on what you are trying to black list
(10:41:25 AM) jjohansen1: ericchiang: yes
(10:41:52 AM) jjohansen1: ericchiang: it should have been out already but has been delayed, uhmm more than a little bit
(10:41:53 AM) ericchiang: cool, thanks :)
(10:42:29 AM) cboltz: jjohansen1: I get your point, but I'm still not a fan of the blacklist way ;-)
(10:42:35 AM) jjohansen1: ericchiang: so I would like to land support for both xattr matching and extended nnp rules in 3.0
(10:42:43 AM) jjohansen1: cboltz: agreed
(10:43:18 AM) jjohansen1: ericchiang: that does mean I need to look at your patches asap
(10:43:59 AM) jjohansen1: oh and we need to get a new revision of the nnp kernel patch together too
(10:44:03 AM) ericchiang: I can revamp the nnps one, the xattrs ones should be good to go
(10:44:38 AM) jjohansen1: ericchiang: unfortunately I had a realization and a larger change in mind for the kernel patch
(10:44:51 AM) jjohansen1: we can discuss outstide of the meeting
(10:45:02 AM) ericchiang: sounds good, no rush on our end
(10:45:06 AM) jjohansen1: I did want to discuss the xattrs one here
(10:45:15 AM) jjohansen1: specifically the syntax
(10:45:35 AM) ericchiang: this one? https://gitlab.com/apparmor/apparmor/merge_requests/270
(10:45:46 AM) jdstrand: jjohansen1: did you see my question re af_unix?
(10:45:56 AM) jdstrand: (sounds like we might be changing subjects...)
(10:46:23 AM) jjohansen1: jdstrand: yes, I thought I answered, but I guess I was too vague
(10:47:11 AM) jdstrand: jjohansen1: my followup
(10:47:26 AM) jdstrand: 12:41 < jdstrand> jjohansen1: how far out is 3.1 from 3.0? if af_unix didn't make it in 3.0, would it be backport to 3.0.x?
(10:47:34 AM) jjohansen1: apparmor 3 covers the userspace release, the userspace already supports af_unix, with the support for the new networking rules format we should be good unless further changes are needed to af_unix kernel side to land it
(10:47:45 AM) jjohansen1: landing af_unix kernel side is another issue
(10:48:02 AM) jdstrand: ok, I knew most of that
(10:48:18 AM) jjohansen1: jdstrand: 3.1 does not have a set release date yet
(10:48:23 AM) jdstrand: but the bit about no changes expected for af_unix I didn't know
(10:48:35 AM) jjohansen1: 3.0, we haven't discussed the revised target but soon I hope
(10:48:37 AM) jdstrand: jjohansen1: ok, thanks. I'm good
Sargun sarnold 
(10:49:17 AM) jjohansen1: jdstrand, sbeattie, sarnold: can you look at https://gitlab.com/apparmor/apparmor/merge_requests/270
(10:49:29 AM) ***jdstrand wasn't sure if af_unix got updates like the network rules did when it went upstream. realize things can change, but atm no
(10:49:32 AM) jjohansen1: specifically the xattr matching syntax
(10:50:01 AM) sarnold: I got worried when I saw the comment that there's possible NULs in the xattrs
(10:50:29 AM) jjohansen1: jdstrand: currently the af_unix patch doesn't have any changes that affect the abi
(10:51:15 AM) ericchiang: sarnold: yeah, evm and ima values are just bytes, not strings
(10:51:24 AM) ericchiang: we can discuss that on the merge request though
(10:51:30 AM) jjohansen1: my concern is it is exposing the full xattr set
(10:52:08 AM) jjohansen1: ericchiang: I was thinking it might be better atm to limit it to something more specific
(10:52:28 AM) ericchiang: I believe evm has an internal whitelist of values it'll measure, I can update it to only accept those if that works
(10:52:30 AM) jdstrand: ericchiang: I feel dim, but I'm not sure what this is doing (there is no man page update in the MR afaics): /usr/bin/* xattrs=(user.foo=bar user.bar=foo) {
(10:52:33 AM) jdstrand:     # ...
(10:52:36 AM) jdstrand: }
(10:52:37 AM) jjohansen1: but wanted feedback from other people who are involved in policy more
(10:53:20 AM) jjohansen1: jdstrand: it is making policy attachment conditional on xattr values
(10:53:21 AM) jdstrand: ericchiang: what are user.foo and user.bar in this context? are bar and foo profiles?
(10:53:39 AM) jjohansen1: remember mjg59's talk at LSS that I pointed you at
(10:53:54 AM) sarnold: jdstrand: extended attributes that may be on a file
(10:54:09 AM) jdstrand: jjohansen1: well, sure, but the example is too general for me (having just looked at this now)
(10:54:18 AM) sbeattie: jjohansen1: off the top of my head, I have some concerns -- we're moving to using named profiles, I'm not sure how they fit in with xattrs syntax; are there other types of matching that we might want to support for policy application, besides path regex and xattrs, that we should make possible to extend the language with?
(10:54:30 AM) ericchiang: jdstrand: the real xattrs we'd like to match on are "security.apparmor", "security.evm" and "security.ima"
(10:55:06 AM) jdstrand: ok, so 'user.foo' maps to say, security.ima
(10:55:26 AM) sbeattie: also, what should happen behaviorally if one profile matches via path regex and a different profile matches via xattr?
(10:55:54 AM) jdstrand: so if security.ima=<something> *and* the path matches /usr/bin/*, then transition to this profile?
(10:55:55 AM) jjohansen1: sbeattie: yes, uid is another that I have been looking at, both in the context around suid, and in the context of which user is running the application (sort of the same but broader than just suid)
(10:55:59 AM) jdstrand: ericchiang: ^
(10:56:11 AM) ericchiang: jdstrand: yes
(10:56:41 AM) jjohansen1: sbeattie: the current behavior, is the path match is done, and then the xattr match, the most specific match wins
(10:56:49 AM) jdstrand: gotcha, thanks. the user.foo=bar and user.bar=foo business wasn't clear, but I understand now
(10:57:13 AM) jjohansen1: most specific, is best path, followed by best xattr as a secondary consideration
(10:57:16 AM) ericchiang: I'll improve the description based on this conversation :)
(10:57:34 AM) jdstrand: ericchiang: thanks. a manpage update would be aces ;)
(10:58:05 AM) sarnold: spot the tennis player :)
(10:58:13 AM) jdstrand: ha
(10:59:53 AM) jdstrand: jjohansen1: which is more specific though? or do mean the most specific path match?
(10:59:58 AM) jjohansen1: so we don't have to beat on the syntax here, if everyone can take some time to look at it and think about it
(11:00:13 AM) jjohansen1: we can do this in the MR
(11:00:27 AM) jjohansen1: jdstrand: the path match is done first
(11:00:59 AM) jjohansen1: for most specific you are choosing between the best set that matches
(11:01:19 AM) jjohansen1: that is we are only choosing between the set of potential matches
(11:01:45 AM) jjohansen1: the path name is matched based on longest left
(11:02:00 AM) jdstrand: jjohansen1: sorry. what is in the set? is /usr/bin/foo {} more specific than /usr/bin/* xattrs=(security.ima=blah) when 'blah' matches?
(11:02:16 AM) jdstrand: that is rhetorical for this meeting
(11:02:36 AM) jdstrand: it isn't clear to me, otoh, which is more specific
(11:02:45 AM) jjohansen1: the kernel side has seen an update, that allows resolving most matches (it is much better than the static match it used to do), but it is possible to still get conflicts
(11:03:05 AM) jjohansen1: once a path match is chosen, its xattrs are looked at
(11:03:29 AM) jdstrand: it isn't entirely clear, again, otoh, which behavior is desirable, or if there are cases where you want to support both
(11:04:12 AM) jjohansen1: jdstrand: not to dodge the question but I think this is best answered with expanding the documentation and the MR
(11:04:18 AM) jjohansen1: description
(11:04:40 AM) jdstrand: ok, with that in mind you are saying that a policy author who wrote /usr/bin/* xattrs=... would need to review policy for /usr/bin/foo {} to see if that still makes sense or to add an xattrs= to /usr/bin/foo
(11:05:02 AM) jjohansen1: I have been trying but obviously failing to describe it and I just think it needs a better more consistent explanation with some examples
(11:05:07 AM) jdstrand: well, no. that's what I meant for 'rhetorical' for this meeting. let's take it to the MR
(11:06:37 AM) jjohansen1: ericchiang: so /me and you have some WI out of this, lets make sure to poke everyone once we have them done
(11:07:16 AM) jjohansen1: can we move on from the specifics of xattr match moving that the the MR
(11:07:33 AM) sbeattie: yes
(11:07:43 AM) ericchiang: jjohansen1: I'll poke the MR once I've update the docs, and provided tests that demonstrate what happens when there are conflicting profiles 
(11:07:54 AM) jjohansen1: thanks
(11:08:29 AM) jjohansen1: and I'll work on getting you the nnp change I was talking about, so we can get that hashed out and finalized
(11:09:09 AM) ericchiang: thanks :)
(11:09:33 AM) jjohansen1: okay, is there anything else on the apparmor 3 list that people want to bring up?
(11:10:01 AM) ***jdstrand has nothing
(11:10:15 AM) jjohansen1: jdstrand: you will note that apparmor 3, is scheduled to provide some basic prompting support which would be your other concern I think
(11:10:54 AM) jjohansen1: so now to the schedule, I would like to get a beta out before the end of the year
(11:11:19 AM) jjohansen1: which I would think would mean a release no sooner than end of January
(11:11:34 AM) jjohansen1: does anyone have problems with this?
(11:11:59 AM) jjohansen1: I will note, that they beta may not be entirely feature complete, so there may need to be a beta 2
(11:12:01 AM) cboltz: maybe you, when I push my next set of patches ;-)
(11:12:35 AM) jdstrand: jjohansen1: I did see that
(11:12:42 AM) jjohansen1: hahaha, cboltz thats okay I'll get you back with a round of C and C++ patches
(11:12:44 AM) sbeattie: jjohansen1: I have questions on the work items, but they're better asked/answered outside this meeting.
(11:12:53 AM) jjohansen1: sbeattie: ack
(11:13:13 AM) sbeattie: (they're mostly because I've been out of the loop on things)
(11:13:42 AM) jjohansen1: you aren't the only one, if you have questions I bet most everyone does
(11:14:14 AM) sbeattie: jjohansen1: I think I'm okay with that timeframe.
(11:15:12 AM) jdstrand: jjohansen1: (also, thank you :)
(11:15:39 AM) jdstrand: the timeframe sounds good
(11:16:26 AM) jjohansen1: ericchiang: does that work for you?
(11:16:52 AM) ericchiang: jjohansen1: yep that works for me
(11:17:09 AM) jdstrand: its rather exciting to think that by the end of this development cycle af_unix upstreamed *and* apparmor 3 is available so everyone can benefit and start integrating into distros
(11:18:06 AM) jjohansen1: jdstrand: err what do you mean by the end of this dev cycle?
(11:21:30 AM) jjohansen1: so I do have one more thing to bring up
(11:23:24 AM) jjohansen1: maybe two
(11:23:40 AM) jjohansen1: apparmor 3.1 release schedule, we need a tentative time line
(11:24:06 AM) jjohansen1: I'd like to see it released before debconf, maybe opensuse
(11:24:41 AM) jjohansen1: specifically, since we have been talking support for user defined policy I'd like to have a release supporting that
(11:25:16 AM) jjohansen1: or at least uhmm, supporting the base of it so it is possible if not uhmm refined
(11:25:31 AM) sarnold: it'd sure be nice to get some feedback on it
(11:25:54 AM) sarnold: but I'm hesistant to commit to anything specifically too quickly
(11:26:28 AM) jjohansen1: debconf is in July, opensuse end of May
(11:26:51 AM) jjohansen1: opensuse may be too early, but perhaps an alpha or beta could be presented
(11:27:09 AM) sbeattie: that uh seems aggressive given our historical "timeliness"
(11:27:24 AM) jjohansen1: so schedule wise, we are looking for 5-6 months after 3.0 for a 3.1
(11:27:31 AM) cboltz: sbeattie: where is your optimism? ;-)
(11:27:38 AM) jjohansen1: sbeattie: yes, very aggressive
(11:28:45 AM) jjohansen1: cboltz: does trying for a July release work for suse
(11:29:01 AM) cboltz: it doesn't matter too much IMHO
(11:29:02 AM) jjohansen1: realizing it may slip
(11:29:06 AM) cboltz: for Tumbleweed, every date is fine
(11:29:24 AM) jjohansen1: cboltz: ack okay, just making sure
(11:29:39 AM) cboltz: and for Leap - well, I probably won't hurry to get a new version into a Leap minor release (aka SP)
(11:29:59 AM) jjohansen1: lets shoot for a mid year release, who knows we might even make it by next christmas ;)
(11:30:49 AM) jjohansen1: last item
(11:31:04 AM) jjohansen1: I'd like to propose we give ericchiang dev status in the project
(11:31:19 AM) ericchiang: happy to help with project maintenance as we ramp up our apparmor usage internally :)
(11:31:47 AM) sarnold: works for me, I've liked reading ericchiang's descriptions and code so far :)
(11:32:28 AM) cboltz: same for me - I like the tools updates for xattrs :-)
(11:34:42 AM) jjohansen1: iirc unless there is an objection, that is enough
(11:34:51 AM) jjohansen1: ericchiang: welcome to the team
(11:35:07 AM) ericchiang: glad to be aboard!
(11:36:17 AM) jjohansen1: thanks everyone
(11:36:23 AM) jjohansen1: meeting adjourned
(11:36:39 AM) sarnold: thanks jjohansen1, everyone :)
(11:36:47 AM) sbeattie: thanks!
(11:36:49 AM) cboltz: thanks everybody!