 ```
(10:00:41 AM) jjohansen1: sbeattie, sarnold, jdstrand, cboltz, ottok meeting time
(10:01:20 AM) jjohansen1: first up is the question of whether we delay 24 hrs as I didn't get the meeting email sent out yesterday
(10:02:43 AM) ottok: you did send the calendar invite way ago, everybody on the list should have seen it
(10:03:17 AM) jjohansen1: ottok: yes but that is not everyone
(10:03:38 AM) jjohansen1: not saying we delay, the question is just open
(10:05:22 AM) jjohansen1: it certainly doesn't appear that we have any kind of quorum today
(10:05:36 AM) jjohansen1: only 3 of us have said anything
(10:06:28 AM) jjohansen1: ottok: another potential way to proceed is deal with your issues today and have a meeting tomorrow where we might have a chance of having more people show up
(10:07:07 AM) ***jdstrand is here
(10:07:14 AM) ottok: I'd prefer that if it is OK. There hasn't been any discussions here for a week so they will easilt see tomorrow what we write today :)
(10:07:15 AM) jjohansen1: oh hey jdstrand
(10:07:20 AM) jdstrand: hello :)
(10:07:35 AM) jjohansen1: ottok: okay lets do that
(10:07:54 AM) jjohansen1: ottok: you are up
(10:09:06 AM) ottok: OK, so I've prepared a new version of apparmor.net 6 months ago. Has been dormant since, kind of waiting for consensus: https://gitlab.com/apparmor/apparmor/issues/20
(10:09:16 AM) jjohansen1: I just sent out an email for tomorrows meeting
(10:10:05 AM) jjohansen1: ottok: yes, sorry I have been meaning to get to looking at that
(10:10:08 AM) ottok: live demo: https://ottok.gitlab.io/apparmor-website/
(10:10:30 AM) ottok: source: https://gitlab.com/apparmor/apparmor.net
(10:11:48 AM) jdstrand: the theme is pretty and the idea is great :)
(10:12:11 AM) jjohansen1: ottok: so I like the idea, and I agree, its nice and clean
(10:12:26 AM) ottok: biggest practical changes would be that apparmor.net would point here (instead of the gitlab project directory) and that cboltz and jjohansen1 would in future write release notes as markdown files to this repo
(10:12:59 AM) jjohansen1: its something that we can update as we have changes so its not like there is a reason not to get it setup
(10:13:02 AM) sbeattie: yeah, that's very nice.
(10:13:43 AM) jjohansen1: ottok: okay, I will work on getting apparmor.net and apparmor.dev redirected to this
(10:14:01 AM) ottok: I can send you instructions on what to do, you don't need to *work*
(10:14:12 AM) ottok: I also need to finalize it
(10:14:34 AM) jjohansen1: I need to look into letsencrypt certificates for apparmor.dev and really apparmor.net so that has to happen first
(10:14:47 AM) jjohansen1: and I need to contact richard about apparmor.org
(10:14:49 AM) ottok: But looks like we got the concensus? cboltz and sarnold have already expressed support in the gitlab issue
(10:15:04 AM) jjohansen1: so there is some busy work to happen before I am ready to do a redirect
(10:15:22 AM) jjohansen1: ottok: yeah I think we just need to do it
(10:15:43 AM) ***sbeattie is +1 as well
(10:15:44 AM) jjohansen1: jdstrand, sbeattie: you in favor?
(10:15:48 AM) jjohansen1: ah thanks
(10:15:48 AM) cboltz [~cb@ip4d15fddb.dynamic.kabel-deutschland.de] entered the room.
(10:16:00 AM) sbeattie: let me know if I can help
(10:16:03 AM) ottok: each releas note will be a markdown file in https://gitlab.com/apparmor/apparmor.net/-/tree/master/content/post - that is the real work you need to do. The publishing of the site is just a simple CNAME DNS change
(10:16:06 AM) jjohansen1: sbeattie: ack
(10:16:23 AM) ottok: I'll post in the issue what exactly needs to be done, I've done it many times before for other sites
(10:17:33 AM) jjohansen1: ottok: well simple CNAME change for http: https: is a little on a redirect due to the certificates etc
(10:18:08 AM) jjohansen1: but yeah, port 80 apparmor.net is any easy quick switch, I have done that one a few times for the different wiki backends
(10:18:24 AM) ottok: One thing you could help with is to check if there is a way to allow me to push to master in this apparmor.net repo: https://gitlab.com/apparmor/apparmor.net/commit/b26967df32c4f1012bde878478d608afc12b3656
(10:18:38 AM) jjohansen1: .dev requires a cert, and I still haven't done that so it isn't live yet
(10:19:31 AM) jjohansen1: ottok: I believe there is, I think I only need to enable the permissions for you
(10:19:38 AM) jjohansen1: give me a minute
(10:22:16 AM) ottok: I think Gitlab get's the certs automatically, no need to do extra work there
(10:23:21 AM) jjohansen1: gitlab has certs but I need certs for apparmor.net and apparmor.dev
(10:23:59 AM) jjohansen1: its not a big deal, I just haven't spent the 15 minutes or what ever it is to do it
(10:24:17 AM) ottok: when you activate apparmor.net for gitlab they will get the certs, no work needed from you
(10:24:22 AM) jjohansen1: ottok: so you want to directly push right
(10:24:54 AM) ottok: can you send me a screenshot of what you see at https://gitlab.com/apparmor/apparmor.net/pages ?
(10:25:45 AM) cboltz: jjohansen1: https://docs.gitlab.com/ee/user/project/pages/custom_domains_ssl_tls_certification/ could be helpful ;-)
(10:25:47 AM) ottok: and go to pages/domains/new to enter the domains. They won't be active until you actually make the CNAME DNS update, this is just part of preparation
(10:27:23 AM) cboltz: actually https://docs.gitlab.com/ee/user/project/pages/custom_domains_ssl_tls_certification/lets_encrypt_integration.html looks much better ;-)
(10:28:10 AM) ottok: just go to https://gitlab.com/apparmor/apparmor.net/pages/domains/new
(10:31:05 AM) ottok: apparently there hasn't been any new releases since  2.13.3
(10:31:22 AM) cboltz: right, known bug ;-)
(10:32:14 AM) jjohansen1: ottok: sadly true, we are working on it but uhmmm everyone has been busy so its taking longer than it should
(10:32:48 AM) jjohansen1: we are trying to get a 3.0 release ready for this month, but being realistic its going to slip into march
(10:33:51 AM) jjohansen1: ottok: you should be able to directly push to apparmor.net, I have made you a maintainer
(10:34:42 AM) sbeattie: jjohansen1: heh, I *just* got to the same point 
(10:35:03 AM) sbeattie: (to make ottok a maintainer of apparmor/apparmor.net)
(10:36:15 AM) ottok: It would also help if I had write access to the wiki, since then I can "view" the markdown source and copying selected contents from wiki to website source is easier
(10:36:48 AM) cboltz: you can even   git clone   the wiki - technically it's a git repo
(10:37:14 AM) cboltz: git clone git@gitlab.com:apparmor/apparmor.wiki.git   should work
(10:37:34 AM) ottok: true, I found git@gitlab.com:apparmor/apparmor.wiki.git, I'll use that
(10:37:57 AM) jjohansen1: ottok: you should now have wiki access
(10:38:19 AM) ottok: thanks
(10:38:47 AM) sarnold: ohooo if this means we can use vim to edit this thing rather than the website.. that's cool
(10:39:07 AM) sarnold: (I was going to say that it was going to change things, but, eh, who am I kidding, I don't know git eitehr..)
(10:39:26 AM) ***cboltz uses vim whenever updating the wiki
(10:40:25 AM) jjohansen1: sarnold: yeah you can edit the wiki locally and then either push or do merge requests
(10:40:36 AM) jjohansen1: no web interface required
(10:41:20 AM) sbeattie: sarnold: yes, that's how I did a fair amount of importing the old wiki.apparmor.net into the gitlab wiki.
(10:41:39 AM) sbeattie: (plus could run conversion utilities locally)
(10:47:28 AM) ottok: Tested, pushing, build and publish at https://apparmor.gitlab.io/apparmor.net/ works
(10:47:39 AM) ottok: I just need to finish it now :)
(10:48:45 AM) ottok: I need to go now. I'll ping you when I think it is ready to publish. Page https://gitlab.com/apparmor/apparmor.net/pages/domains/new should have embedded docs on what to do to CNAMEs to activate custom domain and https automatically
(10:50:41 AM) cboltz: hmm, " Posted on August 612, 18069" looks interesting ;-)
(10:52:08 AM) sbeattie: I *knew* I was behind the times, but still, to have it pointed out so clearly...
(10:55:55 AM) jjohansen1: ottok: okay thanks
(10:56:32 AM) jjohansen1: meeting adjourned, thanks everyone
 ```