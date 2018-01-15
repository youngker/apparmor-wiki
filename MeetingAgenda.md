IRC Meetings
------------

We try to meet once a month on the 2nd Tuesday of the month, in
\#apparmor on oftc.net

If there are reasons to move the time or date we will, and
announcements will be sent out to the apparmor mailing list

Meeting Logs are linked off of the month agenda title.

Next Meeting: IRC Meeting, 18:00 UTC, Tuesday Jan 16 2018
---------------------------------------------------------

-  ...
-  anything else

[IRC Meeting, 18:00 UTC, Wednesday Dec 13 2017](IRC_meeting_2017-12-13)
---------------------------------------------------------

-   git conversion, gitlab. progress checkin. IFF we need to revisit
    -   how will translations work?
    -   bug tracking? switch away from lp?
-   Regular release schedule/cadence for maintenance releases
-   meeting schedule?
    -   do we need to resuscitate the monthly irc meeting? Or would switching to a bi-monthly, or tri-monthly be adequate
-   anything else
    -   next meeting 18:00 UTC, Tues ?. ?, 2018

[IRC Meeting, 18:00 UTC, Wednesday Oct 25 2017](IRC_meeting_2017-11-25)
-----------------------------------------------------------------------------

-   4.0 release (target Nov 2017)
-   git conversion of apparmor project
-   moving source repository to github/gitlab
    -   how will translations work?
    -   bug tracking? switch away from lp?
-   Regular release schedule for maintenance releases
-   anything else
-   meeting schedule?
    -   do we need to resuscitate the monthly irc meeting? Or would switching to a bi-monthly, or tri-monthly be adequate
    -   next meeting 20:00 UTC, Tues Nov. 14, 2017?

[IRC Meeting, 20:00 UTC, Tuesday Oct 10 2017](IRC_meeting_2017-10-10)
---------------------------------------------------------------------------

-   4.0 release (target Nov 2017)
-   git conversion of apparmor project
-   moving source repository to github/gitlab
    -   how will translations work?
    -   bug tracking? switch away from lp?
-   Regular release schedule for maintenance releases
-   status of the YaST2 AppArmor module
-   wiki update (and move)
-   anything else
-   meeting schedule?
    -   do we need to resuscitate the monthly irc meeting? Or would switching to a bi-monthly, or tri-monthly be adequate
    -   next meeting 20:00 UTC, Tues Nov. 14, 2017?

[IRC Meeting, 20:00 UTC, Tuesday Sep 13 2016](IRC_meeting_2016-09-13)
---------------------------------------------------------------------------

-   2.11 release
-   next release 4.0?
    -   feature set planning
    -   target date Feb. 2017
-   apparmor-profiles conversion to git evaluation
-   mod\_apparmor
-   anything else
-   next meeting 20:00 UTC, Tues Oct. 11, 2016

[IRC Meeting, 20:00 UTC, Tuesday Jan 19 2016](IRC_meeting_2016-01-19)
---------------------------------------------------------------------------

-   2.9.3
-   2.10.1
-   caching update
-   when to move apparmor development to git
-   anything else
-   next meeting Feb 9 or March 8

[IRC Meeting, 20:00 UTC, Nov 17 2015](IRC_meeting_2015-11-17)
--------------------------------------------------------------------

-   2.11
-   2.9.3
-   compiler abi for 2.10.1 due to cache timestamp issues?
-   drop Python 2 support in the aa-\* tools?
-   cross-distro profile repository (see gobby.debian.org debconf15/bof/AppArmor)
-   systemd support
    -   stop behavior
    -   restart in parser
-   cache behavior
    -   lots of cache control knobs, not as necessary now
    -   should we wait for policy hashing check, or cleanup now?
    -   in kernel policy sha1
        -   switch to sha1sum friendly?
        -   keep current scheme
            -   push aa-sha1 bash hack? or wait for better tool
-   next meeting Jan 12?

IRC Meeting, 20:00 UTC, Aug 11 2015 --- CANCELLED
-------------------------------------------------

-   2.11
-   2.9.3
-   next meeting Sept 8

[IRC Meeting, 20:00 UTC, July 14 2015](IRC_meeting_2015-07-14)
--------------------------------------------------------------------

-   2.10 status (it had better be released)
-   2.11
-   2.9.3
-   DebConf - who, what, when
-   next meeting Aug 11

[IRC Meeting, 20:00 UTC, June 09 2015](IRC_meeting_2015-06-09)
--------------------------------------------------------------------

-   2.10 status
    -   caching issue vs just using mtime
    -   policy load api
    -   outstanding patches
-   2.11/3.0 planning features and timeline
    -   October (4-4.5 months) or January (6-7 months)
        -   suse schedule - 13.?: 2015, with 8-9 month cadence 13.2 was released Nov. 4, so July?
        -   ubuntu schedule - Wily: FF Aug 20, October 22 release
        -   debian schedule - Stretch: no release date set
    -   stacking (at least within policy ns)
    -   better support of ns issues
    -   better support for overlay issues
        -   overlayfs
        -   aliases for aufs (among other things)? likely not
    -   better support of stacking in userspace
        -   mixed moded (as discussed last time)
    -   policy hashing for cache
    -   multiple-caches
-   anything else
-   next meeting July 14

[IRC Meeting, 20:00 UTC, May 12 2015](IRC_meeting_2015-05-12)
-------------------------------------------------------------------

-   wiki update
-   2.9
-   2.10/3.0 progress on targets
-   2.11 planning
-   anything else
-   next meeting June 9

[IRC Meeting, 20:00 UTC, April 14 2015](IRC_meeting_2015-04-14)
---------------------------------------------------------------------

-   wiki update
-   2.9
-   2.10/3.0 progress on targets
-   anything else
-   next meeting May 12

[IRC Meeting, 20:00 UTC, March 10 2015](IRC_meeting_2015-03-10)
---------------------------------------------------------------------

-   wiki update
-   debian
-   2.9.2?
-   2.10/3.0 progress on targets
    -   dbus
    -   kernel upstreaming
    -   systemd
        -   load of policy
        -   specify policy for unit being loaded
-   unioning issues
    -   overlayfs
    -   aliases
-   inet mediation
-   2.10 when? 15/04?
-   anything else
-   next meeting April 14

[IRC Meeting, 20:00 UTC, Feb 10 2015](IRC_meeting_2015-02-10)
-------------------------------------------------------------------

-   2.10/3.0 progress on targets
-   AppArmor interface modifications around unconfined, default profiles and stacking

` The unconfined mode extension`
` - clear extension of current interface`
`     default (unconfined)`
`   however do we report unconfined or profile (unconfined) on the interface`
` - stacking, currently the profile name is reported in as a fq profile, mode is converted to single character for each profile`
`     profile_1//:ns://profile_2 (EE)`
`   Do we apply this to the proc interface, keep the proc interface only reporting current namespace and add a new interface`
`   Note: it is currently possible to stack within current ns`

-   anything else
-   next meeting March 10

IRC Meeting, 20:00 UTC, Jan 20 2015
-----------------------------------

-   Meeting cancelled - no log

[IRC Meeting, 20:00 UTC, Dec 9 2014](IRC_meeting_2014-12-09)
-----------------------------------------------------------------

-   2.10/3.0 development direction targets
-   anything else
-   next meeting Jan 20

[IRC Meeting, 20:00 UTC, Nov 5 2014](IRC_meeting_2014-11-05)
-----------------------------------------------------------------

-   2.9
-   2.10/3.0 development direction targets
-   general direction for python tools improvements, maintenance and cleanup.
-   anything else
-   next meeting Dec 9

[IRC Meeting, 20:00 UTC, Oct 14 2014](IRC_meeting_2014-10-14)
-------------------------------------------------------------------

-   dbus upstreaming
-   2.8.4
-   2.9 beta2
-   general direction for python tools improvements, maintenance and cleanup.
-   anything else
-   next meeting Nov 11, should it be moved

[IRC Meeting, 20:00 UTC, Sept 9 2014](IRC_meeting_2014-09-09)
-------------------------------------------------------------------

-   dbus upstreaming
-   2.8.4
-   2.9 beta2
-   anything else
-   next meeting

[IRC Meeting, 20:00 UTC, July 8 2014](IRC_meeting_2014-07-08)
-------------------------------------------------------------------

-   dbus upstreaming
-   2.8.4
-   2.9 beta2
-   anything else
-   next meeting Aug 12, @ 20:00 UTC

[IRC Meeting, 20:00 UTC, June 10 2014](IRC_meeting_2014-06-10)
--------------------------------------------------------------------

-   2.8.4
-   2.9
-   anything else
-   Next meeting

[IRC Meeting, 20:00 UTC, February 24 2014](IRC_meeting_2014-02-24)
------------------------------------------------------------------------

-   2.8.4
-   2.9
-   3.0

[IRC Meeting, 20:00 UTC, November 13 2013](IRC_meeting_2013-11-13)
------------------------------------------------------------------------

-   GSoC
    -   when/where to land
-   Reverting broken translations
-   Next meeting

[IRC Meeting, 20:00 UTC, October 8 2013](IRC_meeting_2013-10-08)
----------------------------------------------------------------------

-   GSoC
    -   when/where to land
-   Reverting broken translations
-   Next meeting

IRC Meeting, 20:00 UTC, September 10 2013
-----------------------------------------

-   Cancelled

[IRC Meeting, 20:00 UTC, August 13 2013](IRC_meeting_2013-08-13)
----------------------------------------------------------------------

-   GSoC
    -   Kshitij - status report
-   Debian/Ubuntu policy sync and maintenance
    -   Intrigeri (if available)
-   2.8.2 release
-   2.8.3 schedule
    -   alias patch
    -   log review
    -   anything else
-   3.0 development status
    -   alpha4/5
-   Next meeting

[IRC Meeting, 20:00 UTC, July 09 2013](IRC_meeting_2013-07-09)
--------------------------------------------------------------------

-   GSoC
    -   Kshitij - status report
-   2.8.2 release
    -   

-   3.0 development status
    -   alpha3 - is sitting in ubuntu, do we want to push this into suse
    -   next target
-   Permissions for interpreter in abstractions
-   IPC -implied or explicit permissions to talk to self
-   Next meeting

[IRC Meeting, 20:00 UTC, June 11 2013](IRC_meeting_2013-06-11)
--------------------------------------------------------------------

-   GSoC
    -   Kshitij - status report
-   2.8.2 release
    -   alias patch
-   3.0 development status
    -   alpha 3 when? pushout again or wait for changes?
        -   dbus/ipc syntax still not resolved
        -   default profile being reworked
-   Next meeting

[IRC Meeting, 20:00 UTC, May 14 2013](IRC_meeting_2013-05-14)
-------------------------------------------------------------------

-   3.0 development status
    -   alpha 3
-   2.8.2 release
-   GSoC
    -   Kshitij Gupta - <http://www.google-melange.com/gsoc/proposal/review/google/gsoc2013/kshitij8/1>
    -   Kaan Özdinçer - <http://www.google-melange.com/gsoc/proposal/review/google/gsoc2013/kozdincer/4001>
-   Dev discussion
    -   dbus rules - grouping proposal
    -   default profile
-   Next meeting
-   open - any none scheduled items

IRC Meeting, 20:00 UTC, April 09 2013
-------------------------------------

Canceled

[IRC Meeting, 20:00 UTC, Mar 01 2013](IRC_meeting_2013-03-01)
-------------------------------------------------------------------

-   3.0 beta status
    -   current status
        -   labeling/stacking/dbus/environment filtering
    -   new beta target date
    -   release date
-   2.8.2 - items
    -   bug fixes only or do we slip in a couple minor things with 3.0 being delayed
-   packaging - split apparmor-profiles RPM to apparmor-abstractions (everything except the profiles) and apparmor-profiles (only the profiles)
    -   advantage: any RPM shipping a profile could (should?) require apparmor-abstractions, and would not drag in the other profiles
    -   really needed?
-   next meeting
-   open - any none scheduled items

[IRC Meeting, 20:00 UTC, Feb 05 2013](IRC_meeting_2013-02-05)
-------------------------------------------------------------------

-   Release status
    -   2.8.1 - released thanks sbeattie
    -   3.0
        -   reduced feature set, should we do as 2.9?
            -   basic labeling and stacking
            -   dbus support
            -   new base network
            -   new introspection interface
            -   env vars
            -   what is missing? full extended perms/conditionals, full networking, better variable chroot/ns support, user side permission caching, gsettings, and other desktop confinement support, parser improvements, genprof/logprof updates
        -   alpha2 - late new target Feb 5
        -   alpha3 - target Feb 15?
        -   alpha4 - target March 1?
        -   beta1 - target March 4
    -   mediwiki update - still need to request

[IRC Meeting, 20:00 UTC, Jan 08 2013](IRC_meeting_2013-01-08)
-------------------------------------------------------------------

-   Release
    -   2.8.1
        - in progress, patches pulled back from trunk
        - do 2.8.1 without candidate roll 2.8.2 if necessary

-   -   Alpha 1 - this week
    -   Alpha 2 - target Jan 25
    -   Reevaluate 2.9 - No. Just 2.8.1 and 3.0
    -   mediwiki update - still need to request

[IRC Meeting, 20:00 UTC, Dec 04 2012](IRC_meeting_2012-12-04)
-------------------------------------------------------------------

-   Release
    -   Alpha 1 status
    -   Alpha 2 target
    -   cboltz - openSUSE 12.3 feature freeze date?
        -   current plan is: 2013-01-17: Beta1, 2013-02-07: RC1, 2013-02-28: RC2
    -   Ubuntu 13.04 feature freeze date: 2013-03-07, Beta Freeze: 2013-03-21, Kernel Freeze: 2013-04-11, Freeze: 2013-04-18, Release: 2013-04-25
    -   More than a 1 month difference in dev time. Possibility release
        -   2.9 subset release of 3.0, userspace + kernel with lock rework and new interface, other features that have stablized
        -   3.0 full release with dbus (requires patched dbus), prototype display server, extended kernel features that didn't make it for 2.9 cut off

[IRC Meeting, 20:00 UTC, Nov 06 2012](IRC_meeting_2012-11-06)
-------------------------------------------------------------------

-   Alpha 1: not ready yet. This can't be good we are slipping schedules already. When then?
    -   We could roll alpha 1 this week with 2.8 + a couple of patches + dbus prototype
    -   We could roll alpha 1 in a couple of weeks with, basic parser C++ conversion, kernel lock rework, aafs profile directories, implicit labeling and stacking
    -   Move Beta 1 (feature freeze) to February
-   UDS recap - Lots discussed, basic recap
    -   Continue with core rework (stacking, labeling, extended permissions)
    -   Continue work on env var filtering
    -   Improve the userspace library and API to add the ability to do permission queries and cache them
    -   Work on extending apparmor to better support the desktop and sandboxed applications using trusted userspace apps
        -   DBus
        -   gsettings
        -   file picker
        -   Display server

Discussions to continue from previous meeting

-   do we want a special location for auto-generated sniplets? (for example the samba on openSUSE, which currently lives in local/)
    -   what to call it
        -   policy-extensions.d
        -   snippets/autogenerated
        -   snippets/dhcpd.d
        -   snippets/foo.d
        -   snippets/autogenerated/smbd.d
-   how magical should aa-enable be? (should it look into the extras/ dir?)

[IRC Meeting, 20:00 UTC, Sep 25, 2012](IRC_meeting_2012-09-25)
--------------------------------------------------------------------

Decisions

-   next meeting Nov.6 (monthly basis)
-   next version 3.0
-   tentative release schedule for version 3.0
    -   Alpha 1: week of Oct 24.
    -   Alpha 2: late november
    -   Alpha 3/beta 1: late december
    -   beta1/beta2: in mid january
    -   beta2/3 start of Feb
    -   release: late Feb with possible slip to late march.
-   jj: investigate cost of version tagging more. One potential solution require version tag on older policy.
-   we will carry kernel compat patches for a few more releases
-   some tools will be deprecated for 3.0 release, we will try to update enough that they don't break new policy (ie don't manipulate), but is not release critical. Will try to address in 3.1
-   cboltz: Move /etc/apparmor/profiles/extras to /usr/share/apparmor/extra-profiles/
    -   distros can symlink there from docs dir if desired
    -   update logprof to look at new location
    -   update spec file to make a symlink from old /etc location to new location

Agenda

-   Do we need a regular/semi-regular meeting and if so what schedule (weekly, bi-monthly, monthly)
    -   Is the meeting worth while over just using the ml
-   None release features
    -   autotools rework
    -   testing infrastructure update
    -   documentation
-   Next release planning
    -   release schedule (alpha, beta, final)
    -   expected feature set
        -   do we release features with out tool support? We are still missing tool support for several features. Worry about catching up in the next release (3.1)?
        -   Features: expected state, libraries, kernel, parser, utils? (preview vs. ready)
        -   profile templating
        -   sandbox
        -   dbus
        -   aafs interface?
        -   stacking/implicit labeling
        -   Namespace changes (disconnected paths)
        -   version tag (again). see policy matrix below (forcing policy upgrade, and new parser removes need)
    -   how long to carry compatibility patches once new interface is upstream
-   where to package extra profiles? (/etc/apparmor/profiles/extras/ isn't the best place)
-   do we want a special location for auto-generated sniplets? (for example the samba on openSUSE, which currently lives in local/)
-   how magical should aa-enable be? (should it look into the extras/ dir?)

|                                                                                                                                                   | old kernel | new kernel                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------|------------|------------------------------|
|                                                                                                                                                   | old policy | new policy without new rules |
| old parser                                                                                                                                        | Unchanged  | Fail(1)                      |
| new parser                                                                                                                                        | Unchanged  | Unchanged(4)                 |
| Notes                                                                                                                                             |
| (1) Fails because intent of policy no rule applied is not kept but this is the backwards compatible behavior so could be considered to be correct |
| (2) Fails because parser fails to parse unix domain socket rule                                                                                   |
| (3) Fails because kernel applies rule of no unix domain socket when policy did not specify this                                                   |
| (4) new policy is applied like unix domain rules do not exist which is all the old kernel can enforce                                             |
| (5) old policy is enforced as new policy without unix domain rule breaking backwards compat                                                       |

IRC Meeting, 1800 UTC, Mar 02, 2011
-----------------------------------

-   2.7/3.0 planning

IRC Meeting, 1900 UTC, Feb 03, 2011 \#apparmor OFTC

-   2.5.2 Status
-   2.6.0/3.0.0 status
    -   sprint decisions

IRC Meeting, 1700 UTC, Nov 17, 2010
-----------------------------------

-   2.5.1 kernel status update
    -   jjohansen - nominate patches for 2.5.1 kernel
    -   jjohansen - patches to work with upstream kernels

<!-- -->

-   Action item status
    -   jjohansen - hybrid policy white paper
        -   progress
        -   check into vcs
    -   jjohansen - basic profile, load/remove/replace validation tests
    -   jjohansen - improve wiki work item list for test suites
    -   sbeattie|/jjohansen - examine and reenable networking tests.
    -   sbeattie/jjohansen - 2.5 release notes
    -   sbeattie, jdstrand, jjohansen - brainstorm on the list about improving the regression testsuite
    -   sbeattie - log parsing regression tests suite
    -   sbeattie - find out about Nox's legal status DONE
    -   sbeattie - cleanup grammatical nitpicks in release notes
    -   sbeattie - investigate adding language binding regression tests to libapparmor
    -   sbeattie - find biggest version of Nox in archive
    -   mdeslaur - modify user-space tools to use new profile project
    -   mdeslaur - mod\_apparmor testcase in the apparmor test script in qart
    -   mdeslaur - take a look at FBAC lsm policy creation tool
    -   jdstrand - write pam-apparmor documentation: DONE

<!-- -->

-   Logo

` - place holder currently angling for Nox (sbeattie - inquiring), if not then will reopen the item`

-   UDS - update on what was decided there
-   proposal - reversion 2.6 to AppArmor 3.0, as we are break language compatibility
    -   are there any other language changes we want to make at this time beyond the semantic incompatibilities we are going to introduce
        -   require permissions at the front of the rules?
        -   rule blocks
        -   removal of profile state flags (complain, enforce)
        -   is the version tag required as part of every include file

IRC Meeting, 1700 UTC, Wed 20 Oct, 2010
---------------------------------------

-   2.5.1 kernel status update
    -   jjohansen - nominate patches for 2.5.1 kernel
    -   jjohansen - patches to work with upstream kernels

<!-- -->

-   Action item status
    -   jjohansen - hybrid policy white paper
        -   progress
        -   check into vcs
    -   jjohansen - basic profile, load/remove/replace validation tests
    -   jjohansen - improve wiki work item list for test suites
    -   sbeattie|/jjohansen - examine and reenable networking tests.
    -   sbeattie/jjohansen - 2.5 release notes
    -   sbeattie - log parsing regression tests suite
    -   sbeattie - find out about Nox's legal status DONE
    -   sbeattie - cleanup grammatical nitpicks in release notes
    -   sbeattie - investigate adding language binding regression tests to libapparmor
    -   sbeattie - update wiki/lp with end-of-year target for 2.5.2 release DONE
    -   sbeattie, jdstrand, jjohansen - brainstorm on the list about improving the regression testsuite
    -   mdeslaur - setup new project DONE (https://launchpad.net/apparmor-profiles)
    -   mdeslaur - modify user-space tools to use new profile project
    -   mdeslaur - mod\_apparmor testcase in the apparmor test script in qart
    -   mdeslaur - take a look at FBAC lsm policy creation tool
    -   mdeslaur - cookies DONE

<!-- -->

-   Logo

` - place holder currently angling for Nox (sbeattie - inquiring), if not then will reopen the item`

-   UDS planning
-   2.6 planning
    -   Anything new?

IRC Meeting, 1700 UTC, Wed 06 Oct, 2010
---------------------------------------

-   bumped to Oct 20

IRC Meeting, 1700 UTC, Wed 22 Sep, 2010
---------------------------------------

-   2.5.1 status update
    -   jjohansen - nominate patches for 2.5.1 kernel
    -   jjohansen - patches to work with upstream kernels
    -   jjohansen - caching patch?
    -   sbeattie - check that all 2.5.1 nominations are in
    -   sbeattie - roll test tarballs
    -   sbeattie - 2.5.1 release notes

<!-- -->

-   Action item status
    -   jjohansen - hybrid policy white paper
        -   progress
        -   check into vcs
    -   jjohansen - basic profile, load/remove/replace validation tests
    -   jjohansen - get the regression tests working with current upstream kernel
    -   jjohansen - improve wiki workitem list for test suites
    -   sbeattie/jjohansen - 2.5 release notes
    -   sbeattie - move crowd sourcing notes from gobby into wiki
    -   sbeattie - log parsing regression tests suite
    -   sbeattie - find out about Nox's legal status
    -   sbeattie - cleanup grammatical nitpicks in release notes
    -   mdeslaur - setup new project, and modify user-space tools
    -   mdeslaur - mod\_apparmor testcase in the apparmor test script in qart
    -   medslaur - take a look at FBAC lsm policy creation tool

<!-- -->

-   Logo

` - Nox?`
` - Laser sharks?`
` - another candidate?`

-   Regression Test suite, and testing documentation
    -   We were bit by the regression test suite deficiencies again.
    -   Proposals
        -   Add new stub tests noting that a test is missing or disabled. This will remind us of things that are missing
        -   Update to get listing of skipped and failed tests at the end of a run
        -   Set up testing documentation on how to test
            -   parser tests
            -   libapparmor tests
            -   stress tests
            -   regression tests
            -   tests that aren't integrated yet

<!-- -->

-   2.5.2 planning
    -   Time line
    -   Any outstanding items that are already known and should be included?

<!-- -->

-   2.6 planning
    -   Time Line
    -   Target Features
-   Tools
    -   common back end
    -   update wiki with work items

IRC Meeting, 1700 UTC, Wed 8 Sep, 2010
--------------------------------------

-   postponed

IRC Meeting, 1700 UTC, Wed 25 Aug, 2010
---------------------------------------

-   upstreaming status update
-   2.5.1 status update
-   Action item status
    -   jjohansen - nominate patches for 2.5.1 kernel
    -   jjohansen - hybrid policy email - not done yet, taking a while to write
    -   kees - bug keybuck for ideas about how to handle policy state
        -   symlinks (jjohansen doesn't like current scheme)
        -   mixed into policy (nobody seems to like)
        -   alternate scheme
    -   kees - to write email/kick off discussion of dynamic profiles
    -   sbeattie - roll test tarballs for 2.5.1
    -   sbeattie/jjohansen - 2.5 release notes
    -   sbeattie - 2.5.1 release notes
-   Test suites
    -   Many action items, in place, are in desperate need of improvements
-   Tools - userspace tools are becoming useless, in desperate need of updates. We need to plan what we want to do with the going forward.
-   2.6 planning
    -   Time Line
    -   Target Features
-   Logo

` - Nox?`
` - Laser sharks?`
` - another candidate?`

IRC Meeting, 1700 UTC, Wed 16 Aug, 2010
---------------------------------------

-   upstreaming status update
-   2.5.1 status update
-   Action item status
    -   jjohansen - nominate patches for 2.5.1 kernel
    -   jjohansen - hybrid policy email - not done yet, taking a while to write
    -   kees - to write email/kick off discussion of dynamic profiles
    -   jdstrand - site specific customization of profiles
        -   1. create /etc/apparmor/local dir
        -   2. modify profile to make use of it
    -   sbeattie - roll test tarballs for 2.5.1
    -   sbeattie/jjohansen - 2.5 release notes
    -   sbeattie - 2.5.1 release notes
-   crowd souring of policy - where are we?
-   /etc/apparmor.d/libvirt should this be moved into /etc/apparmor.d/local/
-   Test suites
    -   Many action items, in place, are in desperate need of improvements
-   Tools - userspace tools are becoming useless, in desperate need of updates. We need to plan what we want to do with the going forward.
-   2.6 planning
    -   Time Line
    -   Target Features
-   Logo

` - Nox?`
` - Laser sharks?`

IRC Meeting, 1700 UTC, Wed 04 Aug, 2010
---------------------------------------

-   upstreaming status
-   kernels for 2.6 - <http://kernel.org/pub/linux/security/apparmor/AppArmor-2.6/>
-   site-specific customization of profiles
-   Action item status
    -   jjohansen - semantic change - dir read implies file meta read - email sent
    -   jjohansen - nominate patches for 2.5.1 kernel - still a todo
    -   kees - to write email/kick off discussion of dynamic profiles
    -   jjohansen - hybrid policy email - not done yet, taking a while to write
    -   jjohansen - raise using equivalence class, do performance testing. Deferred until can test from user space.
-   crowd sourcing of policy - where are we?

IRC Meeting, 1700 UTC, Wed 28 Jul, 2010
---------------------------------------

-   postponed until Wed 04 Aug, 2010

IRC Meeting, 1700 UTC, Wed 7 Jul, 2010
--------------------------------------

-   2.5.1 status
-   Upstreaming status
-   Bugs/Issues
    -   [LP Bug \#599450](http://launchpad.net/bugs/599450)
    -   [LP Bug \#602261](http://launchpad.net/bugs/602261)
    -   [LP Bug \#581525](http://launchpad.net/bugs/581525)
-   AA Development
    -   Default to using equivalence classes?
    -   dynamic profiles - how they interact with policy reloads
    -   hybrid policy
    -   parser memory usage
    -   semantic change - dir read implies file meta read
    -   crowd sourcing policy
-   Next meeting Wed 21 Jul or Wed 4 Aug ?
-   Open Discussion

IRC Meeting, 1700 UTC, Wed 23 Jun, 2010
---------------------------------------

-   review of <https://blueprints.launchpad.net/ubuntu/+spec/security-m-apparmor>
-   2.5.1 release planning
-   Longer term roadmap item priority review

