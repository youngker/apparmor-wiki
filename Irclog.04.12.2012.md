
(12:00:58 PM) jjohansen: sbeattie, jdstrand, sarnold, cboltz, kees, mdeslaur: apparmor meeting time
(12:01:11 PM) mdeslaur: hi
(12:01:13 PM) ***sbeattie waves
(12:01:14 PM) sarnold: hello :)
(12:01:24 PM) ***cboltz is awake
(12:01:46 PM) jjohansen: alirght good enough for /me
(12:01:51 PM) jjohansen: lets get started
(12:02:12 PM) jjohansen: so there is not a lot of the agenda http://wiki.apparmor.net/index.php/MeetingAgenda
(12:02:55 PM) jdstrand: hey
(12:02:56 PM) jjohansen: First up, Alpha1: I am working on rolling it out today, it will consist of the updated interface, lock rework and dbus
(12:03:16 PM) jjohansen: for todays release, I will stick it in the apparmor-dev ppa
(12:03:21 PM) jjohansen: and post a tarball
(12:04:09 PM) jjohansen: cboltz: I am skipping the opensuse build service for alpha1, but would like to work with you to make sure we have an alpha2 build that works with suse
(12:04:50 PM) jjohansen: alright moving on
(12:04:51 PM) cboltz: if time permits, I'll also create packages for alpha1 ;-)
(12:05:12 PM) jjohansen: Alpha2 target: I'd like to set this for Dec 21
(12:05:14 PM) cboltz: but I'll probably do it in a separate project (or home:cboltz) and keep the 2.8 "stable" packages for now
(12:06:19 PM) jjohansen: cboltz: sounds good, I'd like to keep the alphas and stable very separate. That is essentially what I am doing with the alphas for ubuntu (ie. I created a ppa just for this)
(12:06:47 PM) sbeattie: jjohansen: Dec 21> hunh, okay. I'll likely be off a few days before that.
(12:07:20 PM) cboltz: sbeattie: Get back there in front of the computer NOW. Christmas can wait.
(12:07:25 PM) cboltz: (just quoting Linus ;-))
(12:07:32 PM) jjohansen: My reasoning for alpha2 on the 21 is that it will let us get a few more things on top of alpha1 and there isn't going to be a whole lot of work done in the following week
(12:07:58 PM) jjohansen: sbeattie: technically I'll be away for 4 days before the 21 too
(12:08:13 PM) sbeattie: "technically"
(12:08:39 PM) jjohansen: yeah
(12:08:45 PM) sbeattie: anyway, that's a fine date by me
(12:09:07 PM) cboltz: no objections from me ;-)
(12:09:23 PM) cboltz: but the more interesting question is: do you have a date for beta1 already?
(12:09:31 PM) jjohansen: it lets people play with something if they want, if we do it after the holidays there won't be any more work done and I'll have to remember where I was at
(12:09:54 PM) jjohansen: cboltz: lets move on to the next topic then :)
(12:10:22 PM) jjohansen: alright, so the suse feature freeze is more than a month before the ubuntu freeze
(12:10:38 PM) cboltz: yes, alignment with the openSUSE schedule is why I asked
(12:10:52 PM) cboltz: well, I'd say two months for the major version number
(12:11:15 PM) cboltz: but updating "$major beta" to "$major final" is possible for the RC
(12:11:18 PM) jjohansen: we really can't afford to cut that much time out of our dev schedule but of course we want suse to have the updated release too
(12:11:49 PM) jjohansen: cboltz: right, so I had the idea of releasing a subset of 3.0 as 2.9
(12:12:22 PM) jjohansen: basically we look at what we have mid January and take most of that as 2.9
(12:12:37 PM) jjohansen: 2.9 and 3.0 will get the same updates for the same features
(12:13:00 PM) jjohansen: just 3.0 would pickup additional work that isn't ready by mid january
(12:13:04 PM) sbeattie: jjohansen: so we'd have two branches to support after 3.0 is released?
(12:13:12 PM) jjohansen: sbeattie: yes
(12:13:31 PM) jjohansen: though ideally a lot of the patches would be the same
(12:14:19 PM) jjohansen: its that or I don't think we will be able to offer this release to suse, I know our schedule is real tight already
(12:14:21 PM) sbeattie: jjohansen: do you have an estimate of things you think wouldn't land in time for 2.9?
(12:14:54 PM) sbeattie: it's hard to judge the support burden in advance without having an idea of what the difference between the branches would be.
(12:15:07 PM) jjohansen: sure, new interface, lock rework, base labeling, probably stacking
(12:15:14 PM) jdstrand: is what would make it in 2.9 all that valuable to suse at this time?
(12:16:03 PM) jjohansen: sbeattie: yep I understand, and I am not saying we even commit to a 2.9 yet. That is one way to offer suse part of the release
(12:16:26 PM) cboltz: new features are always valueable ;-) - but the question is if they are worth the price (more maintenance work for you)
(12:16:30 PM) jjohansen: we won't be able to roll the same beta in january that we will be doning for ubuntu at the start of march
(12:17:10 PM) cboltz: the alternative is to go the easy way and keep 2.8 for another openSUSE release
(12:17:23 PM) jjohansen: cboltz: right, it really does come down to what we can offer by january 17
(12:17:26 PM) cboltz: (a 2.8.1 release would be nice then)
(12:17:42 PM) jjohansen: cboltz: at the very least you will get a 2.8.1
(12:18:01 PM) cboltz: one more reason to package the alphas, so that I'll see what works and what not ;-)
(12:18:09 PM) jjohansen: anyways 2.9 is just an idea to kick around at the moment
(12:18:15 PM) jjohansen: cboltz: indeed
(12:18:40 PM) jjohansen: its something we should reevaluate in the january meeting
(12:18:53 PM) jjohansen: speaking of which that is my last topic
(12:19:41 PM) jjohansen: I'd like to set the next meeting for Tuesday January 8, same time same place
(12:20:14 PM) cboltz: works for me
(12:20:20 PM) qengho left the room (quit: Ping timeout: 480 seconds).
(12:20:30 PM) sbeattie: jjohansen: seems reasonable
(12:21:08 PM) jjohansen: okay, thats two votes for. I'll call that tentatively set
(12:21:28 PM) jjohansen: Alright does anyone have anything else to discuss?
(12:21:54 PM) sbeattie: I don't have anything
(12:22:00 PM) cboltz: where to report bugs against the meeting reminder? *g,d&r*
(12:22:56 PM) jdstrand: +1
(12:23:27 PM) cboltz: (maybe sending it two days in advance works better...)
(12:23:49 PM) jjohansen: hehe, sigh that is just something I have todo. I forgot to put it in my calendar last time. I'll do that first thing even before I go to lunch
(12:24:00 PM) jjohansen: cboltz: sure, np.
(12:25:04 PM) cboltz: another question: the mediawiki version on apparmor.net ;-)
(12:25:30 PM) cboltz: 1.16.2 is very old and probably has security issues (at least, there were security updates for later versions)
(12:25:56 PM) cboltz: can you please remind the admin to update it?
(12:26:08 PM) jjohansen: cboltz: right, thanks for the reminder I need to look into it. I'll poke our hosting and see what we can do
(12:28:56 PM) jjohansen: alright, if thats it, I'll call the meeting adjourned
(12:28:59 PM) jjohansen: thanks everyone
(12:29:10 PM) sarnold: thanks jjohansen :)
(12:29:26 PM) sbeattie: thanks jjohansen 
(12:41:09 PM) jdstrand: thanks jjohansen :)
(01:01:21 PM) qengho [~quassel@184.88.166.159] entered the room.
(02:37:25 PM) qengho left the room (quit: Ping timeout: 480 seconds).
(02:41:28 PM) kees: jjohansen: sorry I missed it! was trying to get rain water to stop running TOWARDS my foundation. :P
(02:41:54 PM) jjohansen: oh ouch
(02:41:57 PM) sarnold: kees: uhoh
(02:42:04 PM) sarnold: kees: did you have any luck?

