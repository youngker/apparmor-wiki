```
(12:00:53 PM) jjohansen: sbeattie, sarnold, tyhicks, jdstrand, mdeslaur, cboltz: meeting time
(12:00:59 PM) mdeslaur: \o
(12:01:04 PM) tyhicks: hello
(12:01:20 PM) cboltz: hi
(12:01:32 PM) ***sbeattie waves
(12:02:22 PM) jdstrand: hi
(12:02:36 PM) jjohansen: alright lets get started
(12:03:14 PM) jjohansen: cboltz: do you have anything you would like to cover in regards to the broken translations, and GSoC
(12:03:27 PM) jjohansen: err s/and/or/
(12:03:39 PM) cboltz: yes
(12:03:51 PM) cboltz: for the translations:
(12:04:13 PM) cboltz: it seems it's not only one commit that introduced mistranslations
(12:04:25 PM) jjohansen: right
(12:04:51 PM) cboltz: reverting everything that could be wrong (which are quite some revisions) is very difficult
(12:05:02 PM) cboltz: and I doubt it's worth the time
(12:05:12 PM) cboltz: because there could be another broken revision we didn't notice
(12:05:19 PM) cboltz: so in the end my proposal is:
(12:05:53 PM) cboltz: mark all translations as fuzzy and let the translators verify (or fix) them
(12:06:10 PM) cboltz: I also propose to delete the en_GB and en_US translations because I don't see any value in them
(12:06:26 PM) cboltz: (they only contain some mistranslations ;-)
(12:06:45 PM) cboltz: what do you think about this proposal?
(12:07:30 PM) jjohansen: if our translations are that broken, then it seems reasonable
(12:07:55 PM) sbeattie: yeah, that seems okay.
(12:08:00 PM) jjohansen: the other alternative would be to delete all the translations and have them start from scratch
(12:08:46 PM) sbeattie: that said, we probably have added strings that need translations as well.
(12:08:52 PM) jjohansen: yes
(12:10:38 PM) cboltz: adding strings is quite easy - run xgettext
(12:11:00 PM) cboltz: BTW: only the translations for the utils are that broken
(12:11:15 PM) sbeattie: I've honestly forgotten how the whole translation process is supposed to work
(12:11:28 PM) cboltz: the parser translations seem to be better (but I didn't check all languages, nor do I understand them ;-)
(12:11:31 PM) sbeattie: (from a development process, not runtime, that is)
(12:12:20 PM) cboltz: xgettext extracts the texts into a *.pot file
(12:12:30 PM) cboltz: and then you have to merge them into the *.po files
(12:12:44 PM) cboltz: there are tools for that, but I'd have to look up the name
(12:12:49 PM) jjohansen: yeah
(12:13:09 PM) jjohansen: I always have to look it up but its not hard
(12:13:43 PM) jjohansen: cboltz: if its just the utils, how much of this changes because of the new tools from kshitj8?
(12:14:43 PM) cboltz: kshitj8 recycled lots of texts, but also added several new texts
(12:14:50 PM) mdeslaur: do we ever import translations from launchpad?
(12:15:18 PM) jjohansen: I haven't, but we should
(12:15:26 PM) cboltz: however, I can't give you a value like "xx% new texts, xx% recycled" ;-)
(12:15:46 PM) jjohansen: cboltz: alright, thanks
(12:16:48 PM) cboltz: so the question is if we just drop all utils translations (except german, which I've already fixed)
(12:16:49 PM) jjohansen: so looking at it, for apparmor we never setup translations in launchpad
(12:17:02 PM) sbeattie: oh
(12:17:03 PM) cboltz: or if we mark everything as fuzzy and hope the translators really proofread it
(12:17:20 PM) mdeslaur: I get all the translations for free for pasaffe, even from users that don't use it because they just click on their language and type in the blanks
(12:17:51 PM) sbeattie: mdeslaur: hunh. that's cool.
(12:18:00 PM) sbeattie: I guess we should set that up.
(12:18:20 PM) sbeattie: jjohansen: you want me to take that task>
(12:18:20 PM) sbeattie: ?
sbeattie sbeattie_ 
(12:18:29 PM) jjohansen: sbeattie: yes please
(12:18:42 PM) cboltz: we should first decide how we handle the broken translations IMHO
(12:19:11 PM) cboltz: otherwise translators might start with the broken files, and we'll end up with a merge nightmare
(12:19:22 PM) cboltz: (or we end up with throwing away translator's work, which is even worse)
(12:19:50 PM) mdeslaur: what is broken exactly? are they not regenerate properly, or some stuff is obviously in the wrong strings?
(12:20:09 PM) cboltz: lots of mistranslations
(12:20:26 PM) cboltz: (see the ML for some examples, some of them are quite funny[tm] ;-)
(12:21:13 PM) cboltz: Subject: Revert r1225 mistranslations (utils/po/*.po)   (from 2013-09-17 and 2013-10-27)
(12:21:24 PM) jjohansen:  msgid "Reading log entries from %s." -msgstr "%s Mailserver-Domains werden eingelesen..."                                                                                  +msgstr "Protokolleinträge von %s werden eingelesen."  
(12:21:38 PM) jjohansen: bah why didn't that wrap
(12:22:07 PM) jjohansen: cboltz: unfortunately not speaking german I'm not sure how bad/funny that is
(12:22:34 PM) cboltz: well, the german string means "reading mailserver domains..."
(12:22:58 PM) cboltz: that's slightly ;-) different from "Reading log entries from %s"
(12:23:18 PM) mdeslaur: ugh, I forget where the main trunk is now
(12:23:24 PM) mdeslaur: sbeattie: what's the url?
(12:23:48 PM) cboltz: you can read the en_GB or en_US translations if you want something you understand - they are also quite funny...
(12:24:07 PM) mdeslaur: is it lp:apparmor?
(12:24:16 PM) jjohansen: mdeslaur: yep
(12:24:35 PM) cboltz: msgid "Setting %s to audit mode."
(12:24:37 PM) cboltz: msgstr "Setting %s to complain mode."
(12:24:38 PM) mdeslaur: most files haven't chaned since 2011
(12:24:41 PM) cboltz: ;-)
(12:24:45 PM) mdeslaur: did they get b0rked before then?
(12:25:14 PM) mdeslaur: the only file that's recent in utils/po is de
(12:25:27 PM) cboltz: yes - some of the infamous translations came in in r1225, others in earlier revisions
(12:26:41 PM) cboltz: and another yes - de is up to date because I fixed it two months ago
(12:27:37 PM) jjohansen: cboltz: ouch is the en_engrish?
(12:27:40 PM) mdeslaur: the r1225 french changes look sane
(12:28:12 PM) cboltz: jjohansen: yes, the en_* are english
(12:28:52 PM) cboltz: mdeslaur: that's part of the problem - some translations are ok, but several others aren't
(12:28:52 PM) jjohansen: haha, yeah not quite what I was getting at but yeah
(12:29:07 PM) cboltz: and without knowing all languages, it's impossible to know which are wrong
(12:29:29 PM) jjohansen: we still are in that situation if we get new translations
(12:29:36 PM) mdeslaur: I think we should just set them up on launchpad, and mark them, and people will go through them
(12:29:51 PM) jjohansen: yeah
(12:29:57 PM) mdeslaur: starting from scratch is hard if someone is unfamiliar with apparmor
(12:30:05 PM) cboltz: jjohansen: yes, but I'm quite sure it can't get worse ;-)
(12:30:08 PM) mdeslaur: but fixing stuff is easier if they can look at the other strings
(12:30:26 PM) jjohansen: maybe, the other string can be completely misleading
(12:30:37 PM) jjohansen: (12:24:35 PM) cboltz: msgid "Setting %s to audit mode."
(12:30:37 PM) jjohansen: (12:24:37 PM) cboltz: msgstr "Setting %s to complain mode."
(12:31:06 PM) mdeslaur: hrm, perhaps
(12:31:09 PM) cboltz: exactly - that's why I thought about completely dropping the translations
(12:33:13 PM) cboltz: BTW: utils/po contains 142 texts, so it's not too much work for translators even if we drop everything
(12:33:29 PM) cboltz: and we make sure no old buggy translations are overlooked
(12:34:57 PM) mdeslaur: well, I feel like we should just push them and they will get fixed...but that's just my 2c, I'll defer to the people who actually do apparmor stuff instead of just lurk :)
(12:35:46 PM) jjohansen: mdeslaur: do you have any feel for how quickly the translation might be done?
(12:35:54 PM) cboltz: BTW: the gsoc code comes with 214 texts
(12:36:22 PM) jjohansen: having poor translations might be better than no translations, except in the cases where the translation are just wrong
(12:36:55 PM) sbeattie: cboltz: what does marking translations 'fuzzy' do?
(12:37:15 PM) cboltz: it's a flag for translators saying "review this"
(12:37:20 PM) mdeslaur: jjohansen: I usually get some updates right after I push string changes up, as it turns red on some language webpage somewhere
(12:37:23 PM) cboltz: users will still get the translated version
(12:37:28 PM) mdeslaur: and triggers people's OCD
(12:37:42 PM) cboltz: (or the mistranslated one ;-)
(12:38:01 PM) tyhicks: I wonder if launchpad knows about the fuzzy flag
(12:38:22 PM) sbeattie: tyhicks: yeah, I was wondering the same thing
(12:38:41 PM) mdeslaur: I think it does, yes
(12:39:13 PM) tyhicks: this bug sounds like it doesn't: https://bugs.launchpad.net/launchpad/+bug/493084
(12:39:21 PM) sbeattie: mdeslaur: which launchpad group do you use for pasaffe?
(12:39:25 PM) mdeslaur: we can definitely drop the english translations
(12:39:31 PM) sbeattie: err s/launchpad/translation/
(12:39:57 PM) mdeslaur: sbeattie: not quite sure...it says "Pasaffe is translated by Launchpad Translators with Open permissions. "
(12:40:06 PM) mdeslaur: so "Launchpad Translators" I guess
(12:40:08 PM) mdeslaur: which is all languages
(12:40:28 PM) sbeattie: oh, I missed it when looking at the list under the launchpad group
(12:40:47 PM) mdeslaur: tyhicks: hrm
(12:41:02 PM) cboltz: the bugreport sums up to "Instead, the translator has to translate the string like a completely new string"
(12:41:13 PM) mdeslaur: it's easy to see, we mark them all as fuzzy, see if the graph turns red
(12:41:22 PM) jjohansen: yep so its effectively dropping the translations
(12:41:22 PM) mdeslaur: if it doesn't turn red, then the hell with it, we purge them all
(12:41:33 PM) cboltz: which means: if we mark everything as fuzzy, the translators get them in their "please translate this" list
(12:41:42 PM) cboltz: probably _without_ seeing the old translation
(12:41:50 PM) jjohansen: right, without the old translation
(12:41:54 PM) cboltz: which in this case is a good thing ;-)
(12:42:04 PM) jjohansen: its treated as a new translation in lp, so its the same thing
(12:42:09 PM) jjohansen: I say just drop them
(12:42:46 PM) jjohansen: but I guess if its easier to mark them fuzzy, its the same thing
(12:43:00 PM) cboltz: I agree - better no translation than broken translations
(12:43:19 PM) mdeslaur: ok, fair enough
(12:43:35 PM) cboltz: deleting is easier - basically you can just drop the *.po files
(12:43:49 PM) mdeslaur: I can write a cheque for 5$ and we can get amazon mechanical slave turk to do them all :P
(12:44:06 PM) jjohansen: right, we should make sure to regenerate the pot file too
(12:44:12 PM) jjohansen: make sure it is up to date
(12:45:32 PM) jjohansen: alright, moving on cboltz anything to cover for the GSoC utils?
(12:46:21 PM) cboltz: well, we should integrate them into trunk ;-)
(12:47:04 PM) cboltz: I didn't see Kshitij online in the last time, and there are still some bugs he has promised to fix
(12:47:08 PM) sbeattie: is there anything blocking that at the moment?
(12:47:11 PM) sbeattie: ah
(12:47:20 PM) cboltz: but in general they look good
(12:47:27 PM) cboltz: the two most critical bugs are
(12:47:39 PM) cboltz: - logprof/genprof only propose ix (not px etc.)
(12:48:03 PM) cboltz: - logprof/genprof ignore the -f parameter and always use /v/l/audit/audit.log and /v/l/messages
(12:49:01 PM) cboltz: the first one is a regression (I remember I've seen it working) so maybe the fix is easy to find by bisecting
(12:49:07 PM) cboltz: and the second should be quite easy to fix
(12:50:18 PM) jjohansen: cboltz: well then I think its time to merge them and move the old tools to deprecated
(12:51:43 PM) jjohansen: alright lets move on
(12:51:59 PM) cboltz: speaking about deprecated -
(12:52:23 PM) cboltz: someone at SUSE decided to remove the genprof/logprof part from YaST because it's unmaintained
(12:53:11 PM) cboltz: which means YaST probably(!) isn't using the perl modules anymore
(12:53:24 PM) jjohansen: well that makes sense
(12:53:25 PM) cboltz: (but I have to check it to be sure)
(12:53:31 PM) sbeattie: hunh
(12:55:19 PM) jjohansen: cboltz: action: check YaST isn't using the perl modules anymore
(12:55:36 PM) jjohansen: with that said we will still just move them to deprecated for the next release
(12:56:21 PM) jjohansen: Alright, I don't think there is anything to annouce for 3.0 atm, its moving forward slowly and we hope to have a new alpha up soon
(12:56:33 PM) jjohansen: does anybody have anything else?
(12:56:46 PM) cboltz: just a question - who does the work of
(12:57:00 PM) cboltz: a) deleting the utils translations (except de)
(12:57:12 PM) cboltz: b) enabling the translation stuff on lp
(12:57:21 PM) cboltz: c) integrating the gsoc code
(12:57:23 PM) cboltz: ?
sbeattie sbeattie_ 
(12:57:41 PM) mdeslaur: don't erase fr either, I'll double check them once their in launchpad
(12:57:46 PM) mdeslaur: they're
(12:57:48 PM) jjohansen: sbeattie: has already volunteered for b
(12:58:03 PM) jjohansen: cboltz: I'll delete the .po files
(12:58:20 PM) jjohansen: cboltz: did you want to take on c) ?
(12:58:52 PM) cboltz: I could, but I don't know bzr good enough to keep the version history
(12:59:01 PM) cboltz: so I'd prefer if someone who knows bzr better does it
(12:59:15 PM) sbeattie: Okay. I can probably take that task, then.
sbeattie sbeattie_ 
(12:59:29 PM) jjohansen: sbeattie: thanks
(12:59:33 PM) cboltz: :-)
sbeattie sbeattie_ 
(12:59:59 PM) jdstrand: before we get to merging, have they been verified to not cause regressions?
(01:00:08 PM) jjohansen: sbeattie: or feel free to kick me, I'd like to get the new versions into wider testing early
(01:00:10 PM) jdstrand: ie, do we have tests for them?
(01:00:28 PM) jdstrand: istr that the userspace utils had poor test coverage
(01:00:49 PM) cboltz: the gsoc code contains some testcases
(01:00:51 PM) jjohansen: jdstrand: we have no tests that I know of, everything has been manually done
(01:01:03 PM) cboltz: they are not complete, but I'm quite sure the coverage is better than for the old utils ;-)
(01:01:25 PM) jdstrand: perhaps, but the old utils are a known entity. the new ones are not
(01:01:34 PM) jjohansen: heh yes, the new stuff has tests but they are all new
(01:01:57 PM) jdstrand: since they are python, we should be able to have python unit tests for them. like we do with easyprof
(01:02:49 PM) sbeattie: there are some python unit tests; they have some... issues (last I looked), and are incomplete.
(01:03:32 PM) jdstrand: I'm not saying they should have 100% coverage right off the bat, but I think it is important that we have reasonable automated coverage
(01:05:15 PM) jdstrand: I'm quite certain once these are checked in, we are going to start moving stuff in to python libraries, getting all the python tools to use the libraries, etc. we need tests
(01:06:51 PM) jdstrand: I didn't mean to halt the meeting on that. I just wanted to make sure we have tests :)
(01:06:56 PM) jdstrand: feel free to move on
(01:07:23 PM) sbeattie: No worries, it's an important point, and one I agree with.
(01:07:40 PM) jdstrand: cool
(01:08:47 PM) jjohansen: alright, anything else?
(01:09:35 PM) cboltz: not from me
(01:09:59 PM) sbeattie: not from me, either
(01:10:00 PM) cboltz: BTW: grepping through the YaST code found several places using the perl modules
(01:10:09 PM) cboltz: but I don't know if this code is still active
(01:10:14 PM) jjohansen: okay next meeting tentatively set for Tues Dec 10 @ 20:00
(01:10:30 PM) jjohansen: cboltz: can you poke the YaST people over it?
(01:10:46 PM) cboltz: yes, of course ;-)
(01:10:54 PM) jdstrand: jjohansen: before we set the next meeting
(01:11:08 PM) jjohansen: jdstrand: shoot?
(01:11:28 PM) jdstrand: jjohansen: I have one question. I realize it could turn in to a long discussion, but it doesn't have to
(01:11:53 PM) jdstrand: jjohansen: so, IPC and LXC are coming and aiui, we should have something his month in trunk
(01:12:18 PM) jdstrand: jjohansen: should we be doing an v3 alpha when those land?
(01:12:26 PM) jjohansen: yes, that was the next alpha soon I was referring too
(01:12:36 PM) jdstrand: oh, I missed that
(01:12:37 PM) jdstrand: ok
(01:12:57 PM) jdstrand: I'd like to get the Ubuntu packaging on some version of 2.9 and drop like 70 patches :P
(01:13:06 PM) sbeattie: +1
(01:13:13 PM) jdstrand: so if we have a stabilization point, that would be nice :)
(01:13:18 PM) jjohansen: well it was uhm very vague, just a "new alpha coming soon"
(01:13:30 PM) jjohansen: +1
(01:13:45 PM) jdstrand: that is good enough for me, so long as we are thinking the alpha will coincide with IPC and LXC
(01:13:52 PM) jdstrand: roughly coincide that is
(01:14:24 PM) jjohansen: well IPC anyways, I'm not sure we will get everything needed for LXC just yet
(01:15:09 PM) ***jdstrand nods
(01:15:17 PM) jdstrand: the date you suggested is fine with me
(01:16:09 PM) jjohansen: alright with that does anybody have issues with the proposed (default) meeting time?
(01:16:44 PM) cboltz: I'm not sure if I'm online on Dec 10
(01:17:22 PM) sbeattie: (I should be available then)
(01:18:54 PM) jjohansen: cboltz: well as we get closure to the date and if its an issue, let us know and we can reschedule
(01:19:10 PM) cboltz: ok
(01:19:15 PM) jjohansen: meeting adjourned
``` 
