```
(12:00:48 PM) cboltz: meeting time ;-)
(12:01:45 PM) jjohansen: yes, though I don't know how many will make it today
sbeattie sbeattie_ 
(12:02:10 PM) jjohansen: tyhicks, jdstrand_, mdeslaur, sbeattie, sarnold, cboltz: meeting time
(12:02:23 PM) ***sbeattie waves
(12:02:29 PM) sarnold: dagnabbit I never got around to reading the lsm mail.. sorry :/
(12:02:42 PM) mdeslaur: \o
(12:03:26 PM) jjohansen: alright lets get started
(12:03:47 PM) kshitij8 left the room (quit: Read error: Connection reset by peer).
(12:03:49 PM) jjohansen: 2.10 progress - sadly not much has happend
(12:04:24 PM) jjohansen: there has been some iteration of an API for systemd to use but it hasn't made progress the last couple of weeks
(12:04:57 PM) jjohansen: there are plans to discuss it this week, but it can't be now because of other issues taking tyler away
(12:05:27 PM) jjohansen: I think the only think currently on schedule for 2.10 is the library/systemd work
sbeattie sbeattie_ 
(12:06:20 PM) jjohansen: so current plans for 2.10 are a very small incremental release over 2.9
(12:07:07 PM) jjohansen: anyone else want to kick in anything around 2.9/2.10 before we move on?
(12:07:37 PM) cboltz: I still have an unanswered mail about the internal layout of NetworkRule class ;-)
(12:08:04 PM) sbeattie: cboltz: sorry, I've been occupied with other things for a while.
(12:08:25 PM) cboltz: that sounds like you'll answer on the ML soon[tm], right?
(12:08:57 PM) sbeattie: hopefully.
(12:09:06 PM) cboltz: just let me note that option b) would make programming more difficult without giving us a real advantage
(12:09:44 PM) cboltz: so if you prefer this option, I'd like to hear a _very_ good reason ;-)
(12:10:29 PM) jjohansen: does because it makes cboltz cringe count :P
(12:11:03 PM) sbeattie: re 2.9, there's a couple of fixes that have been applied (aa-unconfined bug noticed by MoC, and log parsing in the utils) that might warrent making the 2.9.2 release soonish.
(12:11:09 PM) cboltz: jjohansen: no ;-)
sbeattie sbeattie_ 
(12:11:54 PM) cboltz: the /etc/pki patch that someone sent last week also looks like a 2.9 candidate
(12:11:56 PM) jjohansen: sbeattie: yeah soonish, I have some patches to check-in yet and there are a couple of other fixes still pending
(12:12:28 PM) cboltz: (I never used /etc/pki, but on the german opensuse ML someone already hit this problem ;-)
(12:12:50 PM) sbeattie: jjohansen: okay. perhaps outside of the meeting, we can briefly sort out what's outstanding for 2.9.2/
sbeattie sbeattie_ 
(12:13:10 PM) jjohansen: sbeattie: sure
(12:13:22 PM) jjohansen: alright moving on
(12:13:53 PM) jjohansen: the next item is the discussion around the context, profile name/label, and modes
(12:14:29 PM) jjohansen: this is all around stacking/compound labels
(12:14:54 PM) jjohansen: sadly /me has a feeling cboltz is the only one to have read the intro mail
(12:15:51 PM) jjohansen: anyways it breaks down into a few things
(12:15:51 PM) cboltz: jjohansen: you should include "free $whatever if you respond until $date" in your mails ;-)  (hidden in the middle of the text, of course)
(12:16:23 PM) sarnold: "free cboltz's wine"  :)
(12:16:39 PM) jjohansen: cboltz: can I add "excluding cboltz" to that clause, otherwise it will end up costing me
(12:16:45 PM) jjohansen: :)
(12:17:24 PM) ***cboltz hopes that jjohansen will forget the "excluding cboltz" one day
(12:17:37 PM) jjohansen: :)
(12:17:37 PM) jjohansen: 1. we pickup a new mode "unconfined"
(12:18:10 PM) jjohansen: 2. a profile name is actually a label and can be a compound name
(12:18:10 PM) jjohansen:   profile1//&profile2
(12:19:22 PM) jjohansen: 3. there is a question around whether to include visible profiles from subnamespaces
(12:19:22 PM) jjohansen:   lxc-profile   vs.    lxc-profile//&:ns1://apache
(12:20:29 PM) sarnold: I think I liked cboltz's suggestion of not showing other namespaces by default
(12:20:30 PM) jjohansen: 4. hrmm (should have been switched with 3) with multiple profiles, there are multiple modes present
(12:20:30 PM) jjohansen:   profile (enforce)    can now be     profile1//&profile2  (EE)
(12:21:02 PM) jjohansen: 5. can we get rid of the stupid trailing \n that is currently appended
(12:21:35 PM) jjohansen: sarnold: this is not something you will be able to control easily
(12:21:37 PM) sarnold: I thought we added the \n because something broke badly without it
(12:21:38 PM) cboltz: don't you like cat(s)? ;-)
(12:22:06 PM) jjohansen: sarnold: the interface is read only, there is no way to select an option of mode
(12:22:34 PM) jjohansen: sarnold: so to see other namespace info we will have to grow a new parallel interface
(12:23:06 PM) sarnold: jjohansen: yeah, I'm aware it adds complexity elsewhere to keep this interface 'simple'
(12:23:15 PM) jjohansen: I am not saying that going with current ns names only isn't the way to go, just making you aware of the other side
(12:23:36 PM) cboltz: (just in case my cat comment wasn't clear - if you   cat /proc/$pid/attr/current    the \n is very useful)
(12:23:45 PM) jjohansen: sarnold: nor does it mean you won't see compound names (labels) on the interface
(12:23:56 PM) jjohansen: just not the info from sub namespaces
(12:24:16 PM) jjohansen: cboltz: no its actually BAD
(12:25:04 PM) cboltz: without it, you get the result appended to the /proc content in the same line - why would you want that?
(12:25:12 PM) jjohansen: its an extra character that needs to be stripped some times but you can't use a simple tr or such because \n is a valid character within
(12:25:35 PM) jjohansen: cboltz: not saying I do
(12:26:00 PM) sarnold: it turns into something like if (strlen(foo) == '\n') foo[strlen(foo)]='\0'; -- it's annoying to deal with :)
(12:26:17 PM) jjohansen: I'm saying its bad form to include the separator in the data instead of letting the handler choose the appropriate separator
(12:27:01 PM) jjohansen: it makes 1 case easy at the expense of others
(12:27:28 PM) jjohansen: anyway, I am not sure we can even get rid of it now because of backwards compat concerns
sbeattie sbeattie_ 
(12:28:10 PM) jjohansen: sbeattie: do you have any leanings on the namespacing issue?
(12:29:19 PM) sbeattie: jjohansen: I have thoughts but they're ill-formed.
sbeattie sbeattie_ 
(12:29:39 PM) sarnold: jjohansen: ps -Z overwrites the final byte with a 0
(12:30:06 PM) jjohansen: sbeattie: can you reply to the mail when they are better formed
(12:30:12 PM) sarnold: http://sources.debian.net/src/procps/2:3.3.9-8/ps/output.c/#L1334
(12:30:12 PM) sbeattie: yeah
(12:30:19 PM) jjohansen: sarnold: always?
(12:30:27 PM) sarnold: yes
(12:30:41 PM) jjohansen: well that is broken
(12:31:28 PM) jjohansen: could include a \0 though and it would overwrite that
(12:31:49 PM) sbeattie: right, isn't that what selinux does?
(12:32:04 PM) jjohansen: but some other apps don't handle that well (nm I'm looking at you)
(12:34:35 PM) sbeattie: ('less' makes it look pretty ugly, which is IIRC how I noticed it for selinux)
(12:34:56 PM) sarnold: heh
(12:35:48 PM) jjohansen: yeah selinux has a \0 at the end
(12:36:22 PM) jjohansen: I'm not sure why \0 is problematic for somethings but it is, and smack noticed it as well
(12:36:47 PM) jjohansen: it could be because those apps try processing it like its an selinux context
(12:37:21 PM) sarnold: the \0-based variants might screw up memcmp-based label comparisons
(12:37:32 PM) jjohansen: it was enough of a problem that appending \0 to the end of the security sockopt would prevent booting
(12:37:48 PM) sbeattie: ow
(12:37:52 PM) jjohansen: sarnold: explain why they work without it
(12:38:29 PM) sarnold: jjohansen: good point.
(12:40:05 PM) jjohansen: I think the \n \0 end is something we can play with some more but we don't have the info to make a decision on
(12:40:27 PM) jjohansen: for the rest of it, just please read the mail and respond
(12:41:03 PM) jjohansen: We don't have anything else on the agenda, does anyone have anything they would like to bring up
(12:41:19 PM) cboltz: hmm, if dropping or replacing \n already causes such fun, I wonder if it's a good idea to change "unconfined" to something else
(12:43:38 PM) jjohansen: cboltz: ?
(12:43:54 PM) cboltz: I'm talking about /proc/*/attr/current
(12:44:28 PM) cboltz: and when I see how much fun you can have with \n, I can imagine how much fun we'll have by using "profile (unconfined)"
(12:44:38 PM) jjohansen: right, are you referring to unconfined (unconfined)
(12:44:44 PM) cboltz: yes
(12:45:03 PM) jjohansen: I don't think we will switch to unconfined (unconfined)
(12:45:13 PM) jjohansen: but profile (unconfined) should happen
(12:45:34 PM) cboltz: IMHO it's the best to keep /proc/*/attr/current unchanged and add a new interface (with new syntax) somewhere else
(12:45:48 PM) jjohansen: that isn't new syntax
(12:46:04 PM) cboltz: well, depends if "profile (unconfined)" and "unconfined" can co-exist on the same system
(12:46:11 PM) jjohansen: sure
(12:46:45 PM) cboltz: I had hoped for a no ;-)
(12:46:56 PM) jjohansen: btw: there used to be more than two values for mode
(12:47:15 PM) jjohansen: so having an alternative value isn't new
(12:47:28 PM) sarnold: what's troubling you with both "unconfined" vs "profile (unconfined)"?
(12:47:57 PM) cboltz: I'm afraid some external applications could parse it in a wrong/broken way
(12:48:22 PM) cboltz: like   "^.* (.*)$ means that this program has a profile"
(12:49:30 PM) cboltz: I don't have hard facts if someone does parsing in that way, but _if_ someone does, he'll report everything with "profile (unconfined)" as protected by AppArmor
(12:50:28 PM) astaff_ [~astaff@198.0.204.249] entered the room.
(12:50:29 PM) astaff left the room (quit: Read error: Connection reset by peer).
(12:50:45 PM) cboltz: BTW: which other flags did exist in the past? I only know enforce and complain, so everything else must be from stone age ;-)
(12:50:56 PM) badiane1 [~gdurand@D8FF67fa.cst.lightpath.net] entered the room.
(12:50:59 PM) jjohansen: cboltz: hopefully, they aren't already reporting   profile (complain)  as protected
(12:51:13 PM) jjohansen: cboltz: debug  and audit
(12:52:05 PM) cboltz: the good thing about complain is that it's quite common, so I hope everybody who finds our proc interface has heard about it ;-)
(12:52:39 PM) jjohansen: there are now profile flags controlling the audit behavior but it isn't a mode, the profile is enforcing or complaining
(12:53:34 PM) jjohansen: cboltz: hopefully if they have gotten to the point where they are processing our context line, they are using libapparmor to split it appart
(12:54:14 PM) jjohansen: not that that seems to be what happens ...
(12:54:42 PM) jjohansen: any ways I am not terribly worried about the unconfined change
(12:54:52 PM) cboltz: you are too optimistic ;-)
(12:55:00 PM) jjohansen: I haven't run into any problems with it yet
(12:55:01 PM) cboltz: even aa-status uses a regex to parse it...
(12:55:19 PM) jjohansen: yes, I know.  Hence the whole ...
(12:55:52 PM) cboltz: so I wouldn't expect "external" readers to use libapparmor
(12:56:21 PM) sarnold: aa-status predates libapparmor by some margin :)
(12:56:51 PM) jjohansen: its rewrite doesn't :P
(12:57:03 PM) sarnold: oh :/
(12:57:46 PM) cboltz: looks like sarnold wants to write a patch to let it use libapparmor ;-)
(12:58:48 PM) jjohansen: alright, with sarnold running away as fast as he can, is there anything else to discuss
(12:58:56 PM) cboltz: ;-)
(01:03:26 PM) jjohansen: cboltz: btw, the current aa-status sort of works with   profile (unconfined),  it reports processes as having a profile but being unconfined
(01:03:40 PM) sarnold: nice
(01:04:15 PM) sbeattie: jjohansen: I don't have anything
(01:04:24 PM) jjohansen: though I think thats more a function of unconfined not matching complain or enforce
(01:04:25 PM) cboltz: I wouldn't be too surprised if it breaks more if you have a profile named "profile" loaded ;-)
sbeattie sbeattie_ 
(01:04:37 PM) jjohansen: sbeattie: you need to boot with apparmor.unconfined=0
(01:05:14 PM) cboltz: a little status question - is there any progress on upstreaming the kernel patches?
(01:05:30 PM) jjohansen: I still need to make the revisions that where discussed long ago
sbeattie sbeattie_ 
(01:05:47 PM) jjohansen: sbeattie: with that set you boot in
(01:05:47 PM) jjohansen:   default (unconfined)
(01:06:04 PM) jjohansen: and you can replace the default profile if you are brave enough
(01:06:09 PM) sbeattie: hunh
(01:06:13 PM) jjohansen: s/brave/crazy/
(01:06:25 PM) sbeattie: s/crazy/in a throwaway vm/
sbeattie sbeattie_ 
(01:07:29 PM) sbeattie: jjohansen: hrm, do we report what the default is somewhere in apparmorfs?
(01:07:54 PM) jjohansen: sbeattie: basically the feature is there but never been updated to what we discussed from its initial implementation
(01:07:57 PM) sbeattie: or is it always assumed to be called default? I can't remember the results of the prior discussion.
sbeattie sbeattie_ 
(01:08:15 PM) jjohansen: sbeattie: atm it is always default
(01:08:23 PM) jjohansen: the discussion was to change that
(01:08:38 PM) jjohansen: well and other bits around the default behavior
(01:09:35 PM) astaff_ left the room (quit: Quit: My Mac has gone to sleep. ZZZzzz…).
(01:10:24 PM) jjohansen: okay I a going to take that as a nothing else
(01:10:50 PM) jjohansen: Next meeting is scheduled for Tue March 10 @20:00 UTC
(01:11:12 PM) cboltz: looks good
(01:11:24 PM) jjohansen: if there are schedule problems, just shout in IRC or on ml
(01:11:38 PM) jjohansen: meeting adjourned
(01:11:41 PM) jjohansen: thanks everyone
(01:12:02 PM) sarnold: thanks jjohansen :)
``` 
