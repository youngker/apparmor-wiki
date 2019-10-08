 ```
(11:02:29 AM) jjohansen: sarnold, sbeattie, cboltz, who ever else might be around: meeting time
(11:02:40 AM) sarnold: oh my
(11:02:48 AM) ***cboltz hides
(11:02:55 AM) jjohansen: indeed it snuck up on me
(11:03:43 AM) ***sbeattie waves hello
(11:04:21 AM) Talkless: hi
(11:04:33 AM) jjohansen: hey Talkless
(11:05:54 AM) chrisccoulson [~chrisccou@95.172.237.253.ip.static.uno.uk.net] entered the room.
(11:06:19 AM) jjohansen: well lets get started, this is going to be another meeting with not a lot of people attending
(11:06:40 AM) jjohansen: First up on the agenda is policy changes
(11:07:12 AM) jjohansen: basically changes for /etc -> /usr/etc/ that several distros are doing
(11:07:27 AM) jjohansen: the question is how we want to handle it
(11:07:41 AM) cboltz: there's another, related question - MR 212
(11:07:54 AM) jjohansen: just a straight-up /etc/ ->  {/usr,}/etc conversion or something else?
(11:08:04 AM) jjohansen: cboltz: yep, thank you for that
(11:08:53 AM) Talkless: maybe @{etc}  ?
(11:09:05 AM) cboltz: yes, that sounds like a valid option
(11:09:22 AM) sarnold: I'd find @{etc} much easier to read :)
(11:10:46 AM) cboltz: sounds like everybody likes @{etc} ;-)
(11:11:04 AM) cboltz: the reason why I mentioned MR 212 is that it will break when we change /etc/ to @{etc} everywhere
(11:11:10 AM) sbeattie: sadly, same. I think we were trying to avoid libtool style variables for everything, but it seems better than the alternatives.
(11:11:33 AM) cboltz: OTOH, I don't like all changes in it - especially @{lib} "accidently" adds quite some permissions
(11:11:52 AM) Talkless: cboltz: sorry, was too lazy to load that up :(
(11:12:22 AM) cboltz: well, the relevant (and IMHO problematic) part is that it defines   @{lib}={/usr,}/lib{,32,64,/@{multiarch}}
(11:12:59 AM) Talkless: oh that big mr, what's probleme there cboltz?
(11:13:10 AM) sarnold: https://gitlab.com/apparmor/apparmor/merge_requests/212
(11:13:43 AM) cboltz: IMHO @{lib} adds permissions to too many paths, while only a subset is really needed
(11:13:45 AM) Talkless: cboltz: and what it has to do with @{etc} ?
(11:13:53 AM) cboltz: in theory nothing
(11:14:12 AM) Talkless: "that it will break when we change /etc/ to @{etc} everywhere" I don't follow :/
(11:14:17 AM) Talkless: (sorry I'm kida tired)
(11:14:38 AM) cboltz: the @{etc} changes will be close to what 212 changes, and we'll end up with lots of merge conflicts
(11:14:39 AM) Talkless: oh, that MR will break, due to conflicts?
(11:14:43 AM) Talkless: right
(11:15:09 AM) Talkless: yeah so we would need to deal with it first.
(11:15:12 AM) jjohansen: meh, merge requests can be refreshed, its work but ...
(11:15:21 AM) sarnold: so.. do I recall that the rules *must* start with / and not @{foo}
(11:15:23 AM) sarnold: ?
(11:15:46 AM) jjohansen: sarnold: no @{foo} is allowed to start a rule
(11:15:50 AM) Talkless: Also, I would suggest in the future to move all these root dirs to wars, because some people has strange configs that gives these denials: 
(11:15:52 AM) Talkless: audit: type=1400 audit(1564927040.843:119): apparmor="DENIED" operation="chmod"
(11:15:54 AM) Talkless: > profile="thunderbird" name="/srv/ssd3/var/cache/fontconfig/" pid=3065 comm="thunderbird" requested_mask="w"
(11:15:55 AM) Talkless: > denied_mask="w" fsuid=1000 ouid=0
(11:15:57 AM) Talkless: from https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=934024#15
(11:16:03 AM) Talkless: so @{var} would also help
(11:16:15 AM) Talkless: to make tunable via.. tunables 
(11:16:21 AM) sarnold: Talkless: there's an alias functionality for admins that do *that*
(11:16:23 AM) cboltz: I'd argue that mount --bind (instead of symlinks) would help ;-)
(11:16:32 AM) Talkless: oh, right sarnold
(11:16:33 AM) sarnold: but yes bind mounts probably better still
(11:17:27 AM) jjohansen: even with alias, and bind mounts, the question still stands whether we update the policy to do @{var} etc ...
(11:17:33 AM) Talkless: but with @{lib}, @{etc} we are introducing more tunables, while others are left alone, so maybe better just get consistency?
(11:17:36 AM) jjohansen: s/do/use/
(11:17:51 AM) jjohansen: consistency is nice
(11:18:11 AM) jjohansen: it also gives a single place for people to go and update things
(11:18:36 AM) cboltz: Talkless: the reason for @{etc} is to make the profile more readable - compare with /{usr/,}etc/ ;-)
(11:19:10 AM) cboltz: AFAIK nobody has plans to move /var/ to another place, therefore introducing a variable for it sounds superfluous
(11:19:11 AM) jjohansen: that is one of the reasons
(11:19:35 AM) Talkless: cboltz: yes, and having all main root dirs available in @{foo} form might help even more with readibility?
(11:19:36 AM) jjohansen: s/no one/no distro/
(11:20:13 AM) Talkless: allso, why its @{PROC} instead of @{proc} ? :>
(11:20:54 AM) cboltz: Talkless: I tend to disagree - IMHO /var/ is more readable than @{var}, especially for people who don't read AppArmor profiles every day ("plain text" vs. "something magic")
(11:21:08 AM) sarnold: yes but once upon a time no one had plans to move everything from /bin to /usr/bin or the other way around and no one anywhere ever thought about moving /etc ;)
(11:22:00 AM) cboltz: well, I hate that /etc/ contains stuff I never touched since quite a while - but was too lazy to change that
(11:22:07 AM) cboltz: now that some people volunteer... ;-)
(11:22:26 AM) Talkless: cboltz: yeah but now we have @{etc} @{PROC} /var @{lib} /opt @{sys}, some kinda mess :)
(11:22:58 AM) Talkless: some are "magic", some are not.
(11:23:43 AM) cboltz: I know ;-)
(11:23:56 AM) cboltz: I like "magic" if it makes things easier
(11:24:21 AM) cboltz: but I'm not sure if I'd prefer magic "only" for consistency where a plain /var/ would also do
(11:24:22 AM) Talkless: yeah fair point
(11:25:02 AM) Talkless: but fore newcomer it might also raise question why it's /var instead of @{var}, "like all these other things"
(11:26:22 AM) cboltz: it's hard for me to speak from a newcomers' POV, but I'd *guess* newcomers worry more about "what's this @{lib} thing" than about plain paths
(11:28:05 AM) Talkless: yeah you could see it that way too
(11:28:58 AM) Talkless: but if he opens some other profile as example, and every root dir has @{foo}, then,  "welp, I guess it's the way it is"
(11:29:09 AM) Talkless: but.. then.. he tries writing @{mydir}/foo/bar fails :((
(11:29:18 AM) Talkless: and fails*
(11:29:24 AM) jjohansen: how about, this. We start the conversion on the ones we can agree on, and come back around and discuss again after we have had a chance to spend some time with it, and get a better feel
(11:29:57 AM) Talkless: sure, sorry for distration
(11:30:05 AM) cboltz: jjohansen: sounds like a good idea
(11:31:02 AM) jjohansen: not a distraction, its a valid point, I just don't think we are making any more progress hence the proposal to do what we can agree on atm
(11:31:07 AM) jjohansen: alright moving on
(11:32:07 AM) jjohansen: next item on the agenda is splitting out policy for the user space release, and moving to separate policy releases
(11:32:20 AM) cboltz: wait a second
(11:32:37 AM) cboltz: shouldn't we decide if we like the @{lib} definition in MR 212 or not?
(11:32:49 AM) cboltz: (instead of letting that MR hang around for some more months?)
(11:35:54 AM) jjohansen: cboltz: I am not sure we can come to a consensus
(11:36:45 AM) jjohansen: I in theory like it, but I don't like the additional permissions it can grant
(11:37:34 AM) Talkless: maybe it just should be used more sporadically, like, where we already have "{/usr,}/lib{,32,64,/@{multiarch}}" ? 
(11:37:44 AM) Talkless: or am I missing something?
(11:38:22 AM) jjohansen: that seems sensible
(11:38:23 AM) Talkless: "sporadically" is not the word
(11:38:47 AM) Talkless: (or is it?)
(11:39:10 AM) cboltz: my preference would be to have   @{lib} = /{usr/,}lib{,32,64}   so that it covers /usr/lib*/ and /lib*/
(11:39:27 AM) cboltz: but not multiarch
(11:39:52 AM) Talkless: that's nice for openSUSE, but not debian? :(
(11:39:56 AM) jjohansen: Talkless: no, but I understood what you meant
(11:40:01 AM) sbeattie: I guess it would be good to assert what @{lib} is supposed to represent.
(11:41:58 AM) Talkless: sbeattie: by documentation you mean, by better name? What do you have in mind?
(11:43:29 AM) sbeattie: (i.e. is it location where programs will usually find shared libraries (so inclusive of @{multiarch} or to be used as the replacement for /lib and in particular /usr/lib where there are a number of non-shared c library containing subdirectories.
(11:45:12 AM) Talkless: sbeattie: I didn't knew that distinction, thanks. So /usr/lib are kinda more application-specific libraries, as for "non-shared" ?
(11:46:05 AM) sbeattie: by "non-shared c libraries" I mean containing things that aren't necessarily lib*.so
(11:46:47 AM) sbeattie: OTOH /usr/lib/@{multiarch}/ contains a lot of subdirectories, too.
(11:47:08 AM) Talkless: it also has executables /usr/lib/@{multiarch}/glib-[0-9]*/gio-launch-desktop ix,
(11:47:09 AM) sbeattie: just trying to come up with some articulation for what the variable is supposed to represent.
(11:48:13 AM) sbeattie: anyhow
(11:48:45 AM) sarnold: I think @{lib} is trying to ask one variable to do too much
(11:48:49 AM) Talkless: sbeattie: maybe we should bring that quesion into that MR and keep moving here?
(11:50:50 AM) jjohansen: that works for me
(11:51:08 AM) sbeattie: +1
(11:51:39 AM) cboltz: agreed
(11:52:02 AM) cboltz: sarnold or sbeattie, can one of you please comment/ask in the MR?
(11:55:58 AM) Talkless: And so what about /etc? we skip that or I've missed decision?
(11:56:28 AM) cboltz: we'll introduce @{etc}
(11:56:39 AM) Talkless: ok
(11:56:41 AM) cboltz: but ideally wait until MR 212 is merged to avoid lots of merge conflicts
(11:56:54 AM) jjohansen: alright, back to splitting policy out of the userspace release
(11:57:26 AM) jjohansen: we need to decide if we are moving policy out, and if so where to put it
(11:57:45 AM) jjohansen: if we move it out, its clear that policy will get its own release
(11:59:29 AM) jjohansen: one potential location is the profile repo, but its current structure might not be ideal for that
(12:00:16 PM) ***sbeattie really does not like the current structure.
(12:00:19 PM) sarnold: I really like the idea of splitting policy from the rest of the userspace; I also agree the current policy repo isn't great for that.
(12:00:45 PM) Talkless: jjohansen: you mean apparmor-profiles ?
(12:00:57 PM) jjohansen: Talkless: yes
(12:01:39 PM) Talkless: yeah there was thread to suggest changing structure to base on apparmor versions or somthing like that, these ubuntu verisions does not help...
(12:01:44 PM) sbeattie: that said, would abstractions and tunables move out as well? They sort of give a baseline of policy bits...
(12:01:51 PM) jjohansen: if we do move them to the apparmor-profiles git tree it feels like we need to do some reorganization there
(12:02:35 PM) jjohansen: yes, I think abstractions and tunables would have to move as well
(12:02:56 PM) jjohansen: as you said they provide a baseline of policy bits
(12:03:21 PM) sarnold: sbeattie: I think abstractions and tunables should move out; it would be nice to have stronger lines between what they provide, vs the profiles supplied by apparmor, vs profiles supplied by vendors, vs profiles supplied by enthusiasts
(12:03:38 PM) jjohansen: and will only become more important to the policy in the future, splitting profiles and their base seems like a recipe for pain
(12:04:36 PM) cboltz: we have some dependencies between the policy and the code (especially in the tests) which will cause some pain
(12:04:54 PM) sarnold: cboltz: heh yeah, actually *doing* the move sounds painful.
(12:05:03 PM) cboltz: (but still less pain than only splitting out the profiles and keeping abstractions where they are)
(12:05:35 PM) cboltz: sarnold: depends - if you don't care about the history too much, and import them with a commit saying "see apparmor repo for git history", then it's easy ;-)
(12:06:41 PM) sarnold: cboltz: even that sounds painful :)
(12:07:06 PM) jjohansen: cboltz: for tests, we can keep clones of what is needed. The tests should be able to evolve separate from the policy
(12:07:13 PM) jjohansen: having them tied is worrisome
(12:07:25 PM) Talkless: how will package maintainer "know" which profiles release they can use for AppArmor they ship? In order not to use "too-recent" profiles that parser does not support...
(12:07:55 PM) cboltz: jjohansen: agreed, but we also have tests the other way round - "make check" in the profiles directory validates the profile syntax with the parser and aa-logprof
(12:08:09 PM) jjohansen: ah right
(12:08:59 PM) sbeattie: hrm, IIRC the debian packaging for the apparmor-profiles attempts to validate them against the "current" parser.
(12:09:49 PM) jjohansen: Talkless: good question, I don't have a good answer, other than we need to organize policy so that this can be dealt with, and even provide a make check on policy against an installed userspace version so that you can check that it supports the policy as part of the packaging build
(12:09:57 PM) jjohansen: assuming you are using packaging
(12:10:20 PM) jjohansen: sbeattie: yeah, I believe that is correct
(12:12:32 PM) jjohansen: it might be even possible, for packaging purposes to set it up so a build will select the right version of policy based on current
(12:12:43 PM) jjohansen: that is the current parser
(12:13:44 PM) jjohansen: let me backup, do we even have consensus that looking at moving policy out of the userspace release makes sense
(12:14:12 PM) jjohansen: and that before we do it, we would need to explore more how best to do it?
(12:14:51 PM) cboltz: I'm not against it, but I'm not sure if the work it causes (AFAIK: test dependencies in both directions) is worth the effort
(12:14:57 PM) sbeattie: I'm ... mildly dubious of the utility of doing so, but I'm not opposed.
(12:15:38 PM) Talkless: It seems complex, and would be nice to get intrigeri's input with his debian hat on.
(12:16:33 PM) jjohansen: some potential benefits, is more frequent releases, around distros as its the policy that changes the most
(12:16:33 PM) jjohansen: smaller SRU patchset for the code portion of the project making it easier for distros to adopt the point releases
(12:16:47 PM) jjohansen: also same goes for policy
(12:17:15 PM) jjohansen: when everything is smashed together it makes it difficult to separate and choose what you are willing to take
(12:18:02 PM) jjohansen: I expect separating them will make this type of decision easier for debian
(12:18:09 PM) jjohansen: and probably ubuntu
(12:18:42 PM) jjohansen: I know intrigeri has asked for the separation in the past, specifically around trying to do SRUs
(12:18:46 PM) sarnold: jjohansen: is there something motivating bringing it up today? I've thought before that it'd be nice to separate policy from the rest of the userspace but couldn't quite articulate why I thought that, so never proposed it myself :)
Sargun sarnold 
(12:19:24 PM) jjohansen: sarnold: its been on the agenda for more than 6 months and just keeps getting kicked down the road
(12:19:41 PM) jjohansen: it was brought up around SRUs before buster was released
(12:20:09 PM) jjohansen: but it has been brought up by various people in the past as well
(12:21:58 PM) Talkless: Sorry, but gotta go, bye all.
(12:23:01 PM) jjohansen: bye Talkless
(12:23:16 PM) jjohansen: anyways, lets table this as to be further investigated
(12:23:18 PM) sarnold: bye Talkless
(12:23:37 PM) jjohansen: Talkless: one question, do you care about a 2.14 release happening this fall?
(12:23:43 PM) jjohansen: I expect not
(12:23:51 PM) Talkless: I do not know anything about that
(12:24:37 PM) jjohansen: Talkless: hasn't been discussed yet :)
(12:24:58 PM) jjohansen: next topic a 2.14 release
(12:25:14 PM) jjohansen: it has been 1.5 since our last release (not good)
(12:25:16 PM) Talkless left the room (quit: Quit: Konversation terminated!).
(12:25:19 PM) jjohansen: 3.0 is just not ready
(12:25:35 PM) jjohansen: and won't be able to rollout until some time in the new year
(12:26:17 PM) cboltz: thinking about the usual AppArmor timelines, 2021 is also a new year, right? *g,d&r*
(12:26:18 PM) jjohansen: should we do an interim 2.14 release, with all the current fixes and few additions that are currently avaiable
(12:27:28 PM) sbeattie: I would be okay with that.
(12:27:43 PM) sbeattie: I'd like us also to reduce the number of existing releases we maintain.
(12:27:52 PM) cboltz: do you have a quick overview of what is only in master (not backported to 2.13)?
(12:28:00 PM) sarnold: I'm leaning towards yes, but only if we can EOL one or two of our older releases
(12:28:44 PM) jjohansen: cboltz: no I don't, though it will have support for googles xattrs based attachment
(12:29:04 PM) jjohansen: I should note google has asked us for a more up to date release
Sargun sarnold 
(12:29:23 PM) jjohansen: sarnold: and we can't drop any of our older releases yet
(12:30:20 PM) jjohansen: 2.10 isn't 5 years old yet, and a variant of it is in one of the ubuntu supported releases
(12:30:51 PM) jjohansen: I can't remember where things stand wrt to 2.10 for suse atm
(12:31:47 PM) cboltz: openSUSE currently needs 2.12 in Leap (might be upgraded to 2.13 in the next release) and 2.13 in Tumbleweed
(12:32:06 PM) cboltz: AFAIK SLE 12 still uses 2.8.x, but - not my bug ;-)
(12:32:53 PM) jjohansen: :)
(12:33:17 PM) cboltz: to my knownledge, 2.9, 2.10 or 2.11 are not included in any supported SUSE or openSUSE distro
(12:33:35 PM) jjohansen: okay, thanks cboltz
Sargun sarnold 
(12:34:20 PM) jjohansen: so generally we have been trying to provide 5 years of upstream support
(12:34:35 PM) jjohansen: we can certainly re-evaluate this
(12:35:36 PM) jjohansen: but we want to make sure it doesn't affect any distros who have chosen a specific version
(12:36:00 PM) jjohansen: going forward we certainly could look at having dev releases vs lts releases
(12:36:37 PM) jjohansen: but I always question the value of dev releases, when they don't get much support and you can just take a snapshot of dev
(12:37:11 PM) jjohansen: I don't think we have been doing releases often enough to need the separation of dev vs lts either
(12:37:57 PM) cboltz: one usecase for dev snapshots would be rolling releases like Tumbleweed - no need for getting them patched, just upgrade to the next dev release
(12:38:38 PM) sarnold: hmm that reminds me of the old buildbots that built new tarballs on every checkin..
(12:39:15 PM) cboltz: that might be a bit ;-) too often
(12:40:25 PM) sbeattie: infrastructure is still there to do the same (I use it for the coverity uploads).
(12:40:27 PM) jjohansen: cboltz: sure I can see that
Sargun sarnold 
(12:42:52 PM) jjohansen: okay, so we can look into some kind of dev snapshot for the future, that tumbleweed etc could use
(12:43:22 PM) jjohansen: those would not be supported releases
(12:43:45 PM) jjohansen: as you would just replace it with a new snapshot with any fixes that went into the dev tree
(12:44:29 PM) jjohansen: I am really uncomfortable with the 1.5 year release lag we are at atm, so I think we need a 2.14 release
(12:44:39 PM) sarnold: 2.14 might make most sense if we can pop it into ubuntu -devel shortly after eoan is released; would it make most sense to do that directly or would we want to sync it in via debian?
(12:44:44 PM) jjohansen: its really unfortunate, as I would rather just go 3.0
Sargun sarnold 
(12:45:44 PM) jjohansen: sarnold: I don't know, jdstrand would be the one to answer that
(12:45:49 PM) cboltz: well, then just implement whatever is missing for 3.0 ;-)
(12:46:19 PM) jjohansen: but I expect F to have a hard requirement on 3.0 for some reason
(12:46:47 PM) sarnold: jjohansen: as do I, but it'd be nice to have our fallback position all ready to go at the start of the cycle rather than the end
(12:46:52 PM) jjohansen: hahaha, if only I could set the priorities
(12:46:55 PM) sarnold: :D
Sargun sarnold 
(12:47:31 PM) jjohansen: sarnold: yeah, I think having 2.14 at the start of the cycle is a good idea
(12:47:58 PM) jjohansen: considering 2.13 landed after feature freeze, and what a pita that was
(12:49:29 PM) jjohansen: okay, well I will try to get a 2.14 release out in Nov.
(12:49:49 PM) jjohansen: Nov. because I can't see having time before LSS-EU
(12:50:03 PM) jjohansen: does anybody have anything else to bring up?
(12:50:33 PM) sarnold: nothing from me
(12:50:49 PM) cboltz: just the usual "please review the pending merge requests" reminder ;-)
(12:51:28 PM) jjohansen: cboltz: ack, I will try to get to some of them, insanely busy atm though
(12:54:06 PM) sarnold: alrighty, seems like good time for lunch then :)
(12:54:11 PM) sarnold: thanks for the merge request reminder cboltz
(12:54:20 PM) jjohansen: alright thanks everyone, meeting adjourned
(12:54:20 PM) sarnold: thanks for running the meeting jjohansen
(12:54:25 PM) sarnold: thanks everyone :D
(12:54:51 PM) cboltz: thanks everyone, and enjoy your lunch ;-)
 ```
