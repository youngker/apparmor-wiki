```
[22:01:05] <jjohansen> sbeattie, jdstrand, tyhicks, mdeslaur, sarnold, cboltz, mancha, darix: meeting time
[22:01:15] <sarnold> woot
[22:02:20] * sbeattie o/
[22:03:43] * cboltz waits for the 2.10 release
[22:04:03] <tyhicks> hello (I'll mostly be absent during the meeting but ping me if you need my attention)
[22:06:18] <sbeattie> cboltz: I'm doing brown paper bag testing now
[22:06:37] <jjohansen> alright lets get started, the meeting agenda is at
[22:06:37] <cboltz> sounds like an interesting test method...
[22:06:39] <jjohansen> http://wiki.apparmor.net/index.php/MeetingAgenda
[22:07:28] <jjohansen> first up, 2.10 - as sbeattie already mentioned it is under going some basic testing and should be released today
[22:07:49] <jjohansen> s/should/baring any unforseen issues should/
[22:08:07] <jjohansen> sorry it has taken so long
[22:08:20] <jjohansen> moving on to 2.11
[22:08:37] <jjohansen> so we still don't have any firm plans for this one
[22:09:05] <jjohansen> with 2.10 releasing so late, I question that we will have a 2.11 release in October
[22:10:00] <jjohansen> that would be only 3 months from now
[22:10:18] <jjohansen> of course I am not opposed to a smaller release
[22:10:37] <jjohansen> as I don't think we have any major features planned
[22:10:38] <sbeattie> hehe, 2.10 was supposed to be a smaller release, no?
[22:11:13] <jjohansen> well yes, but basic library support for systemd was a hard requirement, and that turned out to be more than expected
[22:11:34] <jjohansen> I can see rolling a 2.11 that would be mostly bug fixes
[22:11:46] <jjohansen> or improvements to what exists in 2.10
[22:12:09] <jjohansen> we still need to actually implement the systemd support on top of the new api in 2.10
[22:12:27] <jjohansen> and that may lead us to want to revise and get a new release out asap
[22:12:32] * cboltz wonders if we should call "mostly bugfixes" 2.11 or 2.10.1
[22:12:56] <jjohansen> cboltz: well I do have plans for a few things to hit in the next couple of months
[22:13:35] <jjohansen> I have more policy compile performance improvements brewing that should hit
[22:13:46] <cboltz> ah, so not only bugfixes
[22:13:49] <jjohansen> there is also the dconf work that should hit
[22:14:08] <jjohansen> right, it wouldn't be just bugfixes
[22:14:30] <jjohansen> but lets face it, in a couple of month 2.11 and 2.10.1 won't have a huge diff either
[22:14:34] <sbeattie> I expect we will have pure bugfix 2.10.x releases.
[22:14:39] <jjohansen> right
[22:14:43] <cboltz> the only problem I see with a major release is that we have to maintain more branches
[22:15:32] <jjohansen> well how long do we plan on supporting 2.8?
[22:15:56] <jjohansen> I think its unlikely there will be another 2.8 release
[22:16:16] <cboltz> indeed
[22:16:40] <jjohansen> 2.9 will have at least one more release
[22:16:54] <jjohansen> yes likely more than that
[22:17:41] <jjohansen> I guess it really comes down to whether we want to carry a 2.11 with only a few features or if ubuntu carries those changes locally
[22:17:55] <cboltz> right, 2.9 got quite some patches - and has some more in the candidate queue
[22:18:11] <cboltz> so I hope for 2.9.3 soon[tm]
[22:18:19] <jjohansen> I think the maintenance work will be there for the Ubuntu people whether or not it is in upstream apparmor
[22:19:05] <jjohansen> cboltz: right, there are several patches waiting to be backported/cherry picked from 2.10 as well
[22:20:59] <cboltz> yes, see the "2.9 backport candidates" mail (from June 6) for a (probably incomplete) list
[22:21:17] <sbeattie> cboltz: yeah, I'm still meaning to get to that list. :(
[22:21:43] <cboltz> getting 2.10 out was the priority, so I understand the delay
[22:22:03] <jjohansen> I expect we can get 2.9.3 out in a week or 2
[22:22:42] <cboltz> sounds good :-)
[22:23:19] <jjohansen> so for 2.11 atm, I am thinking we leave the call of whether to roll it as an October release until later
[22:23:30] <jjohansen> maybe in September or even October
[22:23:31] <cboltz> IMHO we shouldn't wait longer - better release it soon and miss backporting a patch than leaving 30 bugs unfixed for some more weeks
[22:24:15] <jjohansen> we will have a better view on what has been done, and whether its worth rolling a release then
[22:24:45] <jjohansen> cboltz: yep, agree sooner is better
[22:25:26] <jjohansen> so I don't have anything else for 2.11 beyond proposing we wait a couple months to see where we are at
[22:25:33] <cboltz> for the october release - basically agreed, but I'd say we "just" need to decide if we label it 2.10.1 or 2.11 ;-)
[22:26:21] <cboltz> I'm quite sure we'll have something better (less bugs, more features) until then
[22:26:21] <jjohansen> cboltz: well there will be a 2.10.1 and 2.10.2 for sure, it really is just a matter if we roll a 2.11 with a few new features
[22:26:44] <jjohansen> alright, lets move on to the next topic
[22:26:50] <jjohansen> debconf
[22:26:56] <jjohansen> cboltz?
[22:27:18] <cboltz> well, the questions are listed in the wiki -
[22:27:28] <cboltz> who will be there? on which days?
[22:27:42] <jjohansen> cboltz: so no one from the Ubuntu apparmor team
[22:28:02] <jjohansen> Canonical is sending people, but no one who is actively working on apparmor
[22:28:13] <cboltz> sounds like you need to kick your boss a bit...
[22:28:14] <jjohansen> there was a request made, and the answer was no
[22:28:43] <cboltz> FYI: my "AppArmor crash course" talk was accepted
[22:28:50] <jjohansen> cboltz: nice
[22:29:09] <sarnold> yay :)
[22:29:16] <jjohansen> cboltz: there real issue is that the request didn't come soon enough and conference allocations where already made
[22:29:57] <jjohansen> if we get the request in sooner, it is likely possible we can get one person too it next year, but then again who know who else will be there or where it will be
[22:30:18] <cboltz> yes, that's the problem - I doubt it will be in germany again
[22:30:28] <jjohansen> this year is a good location to pick up you, and a few others in europe
[22:30:32] <jjohansen> right
[22:31:09] * cboltz wonders if Canonical will send some people to the "10 years openSUSE" party^W conference in Nürnberg next year
[22:31:51] <cboltz> (you can declare that as an apparmor dev meeting ;-)
[22:32:32] <jjohansen> cboltz: we can make the request again, with the info that an apparmor presentation has been accepted but answer will probably still be no
[22:33:04] <cboltz> try it nevertheless, and add a note that I'll need some help with the new rule types (signal, mount, dbus etc.) ;-)
[22:33:06] <jjohansen> cboltz: hehe, ssshh, no no the apparmor dev meeting just happens to be at the smae time in the same place ;)
[22:33:34] <cboltz> yes, that's what I meant - Nürnberg is a nice place for such a meeting ;-)
[22:33:37] <jjohansen> cboltz: ack, help can definitely be given in preparing the talk for those
[22:34:00] <jjohansen> of Nurnberg, I think I hear sarnold already volunteering
[22:34:40] <jjohansen> s/of/oh/
[22:35:11] <jjohansen> alrigh, does any body have any thing else they would like to raise?
[22:35:50] <cboltz> well, I thought of an (maybe informal) session about cross-distro profile maintenance
[22:36:04] <jjohansen> cboltz: for debconf?
[22:36:17] <cboltz> why not? ;-)
[22:36:21] <jjohansen> or a meeting/session here
[22:36:54] <jjohansen> cboltz: well besides you need to kick our bosses, no reason at all :)
[22:36:56] <cboltz> that would also be an option, yes
[22:37:28] <jjohansen> cboltz: so yes we need to do something with a cross distro planning session
[22:37:48] <cboltz> I'll ask intrigeri and h0lger when I see them online again - maybe we can prepare some ideas at debconf
[22:37:59] <cboltz> and then have an IRC meeting about it afterwards
[22:38:03] <jjohansen> cboltz: that would be great
[22:39:23] <cboltz> maybe that's another detail you can add to your travel request ;-)
[22:39:29] <jjohansen> sure
[22:39:41] <jjohansen> okay anything else?
[22:40:00] <cboltz> yes, something totally different ;-)
[22:40:12] <jjohansen> cboltz: you have the floor
[22:40:17] <cboltz> translations - how much do we want to have translated?
[22:40:34] <cboltz> the german translations that came in recently translate really everything
[22:40:47] <cboltz> which means something like "Verbotsregeln" for deny rules
[22:41:07] <sarnold> I'd guess it depends on the context..
[22:41:13] <cboltz> while this is a correct translation, I wonder if it would be better to mention the english keyword there
[22:41:13] <jjohansen> cboltz: you mean in the actual policy files?
[22:41:32] <cboltz> no, in parser error messages etc.
[22:41:50] <jjohansen> so I would think for policy syntax the actual keywords are necessary, though there could be a supporting translation
[22:42:05] <jjohansen> so say for the error message
[22:42:22] <jjohansen> Verbotsregeln (deny) would work
[22:42:28] <jjohansen> or something like that
[22:42:42] <jjohansen> so could be
[22:42:42] <jjohansen>   deny (Verbotsregeln)
[22:42:58] <jjohansen> I would think something like that would be language dependent
[22:43:18] <cboltz> yes, I also thought about something about that (but would also accept "deny-Regeln")
[22:43:41] <cboltz> now the question is how we can give the translators a hint about this?
[22:43:56] <jjohansen> sure, different languages could have different conventions on how to deal with that
[22:43:59] <sarnold> I think I'd rather see john's suggestions than deny-Regeln
[22:44:11] <cboltz> having "deny rules (deny)" in the english text obviously isn't a solution ;-)
[22:44:25] <sarnold> mixing languages in one word is strange to me, hehe ;)
[22:44:25] <jjohansen> well giving the translators hints is tough
[22:45:00] <jjohansen> they have to understand the relation between text being translated and policy, and what the policy syntax is
[22:45:13] <jjohansen> and tbh I doubt many of them understand that
[22:45:20] <cboltz> right
[22:45:24] <sarnold> do the tools extract any comments near the source of the strings being used?
[22:45:35] <sarnold> I don't think they do, but it's been years since I've looked
[22:45:41] <jjohansen> sarnold: not that I know of
[22:45:55] <jjohansen> but it has been years since I looked as well
[22:46:26] <cboltz> IIRC I heard about a special comment format that gets extracted, but please don't ask for details ;-)
[22:46:37] <sarnold> if there is, it'd be worth using
[22:46:56] <jjohansen> cboltz: well knowing it exists is enough for one of us to go look it up
[22:47:08] <jjohansen> sarnold: would you be willing to look into it?
[22:47:11] <sarnold> sure
[22:47:23] <cboltz> is there an easy way to find out who submitted a translation on launchpad? (besides browsing the 30+ pages with 10 strings on each one)?
[22:47:38] <cboltz> if yes, sending a mail to the most active translators could also help ;-)
[22:47:58] <jjohansen> I am unsure but we can poke
[22:49:01] <jjohansen> https://launchpad.net/apparmor/+topcontributors
[22:49:17] <jjohansen> last category is Translations in Rosetta Karma
[22:49:30] * jjohansen thinks that is what we are looking for
[22:50:19] <cboltz> yes, looks like a good start
[22:50:36] <cboltz> but we should avoid mailing people who submitted some translations 5 years ago ;-)
[22:50:59] <jjohansen> oh come now, they are marked for life!
[22:51:09] <cboltz> ;-)
[22:51:31] <cboltz> a quick check for recently submitted translations shouldn't be too hard ;-)
[22:51:56] <jjohansen> yes it should be fairly easy to cull that list, there are only a handful of people to really look at
[22:53:23] <jjohansen> so we should put something together on the list first, and then we can send that out
[22:53:31] <cboltz> right
[22:53:51] <jjohansen> cboltz since you spotted the issue in the german translations do you want to take a first pass at it?
[22:54:26] <cboltz> I'll send something before 2.9.3 is out ;-)
[22:55:10] * cboltz hopes this will result in a race
[22:56:27] <jjohansen> hehe
[22:56:32] <jjohansen> so is there anything else?
[22:57:10] * cboltz hopes sbeattie will say "I'll get 2.9.3 out before cboltz sends this mail" ;-)
[22:58:27] <jjohansen> cboltz: well I can hope a 2.10.1 release won't be necessary but that doesn't mean it will happen :)
[22:59:03] <jjohansen> next meeting is tentatively Tuesday Aug 11 @20:00 UTC, scream load here or an the list if this is an issue
[23:00:03] <jjohansen> alright meeting adjourned
```
