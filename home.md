[AppArmor](About)
=====================================

Welcome to the AppArmor security project wiki, the wiki for users
and developers of the AppArmor security project.

Description
-----------

AppArmor is an effective and easy-to-use Linux application security
system. AppArmor proactively protects the operating system and
applications from external or internal threats, even zero-day attacks,
by enforcing good behavior and preventing even unknown application
flaws from being exploited. AppArmor security policies completely
define what system resources individual applications can access,
and with what privileges. A number of default policies are included
with AppArmor, and using a combination of advanced static analysis
and learning-based tools, AppArmor policies for even very complex
applications can be deployed successfully in a matter of hours.

More details about AppArmor can be found in the [documentation](Documentation)

Getting AppArmor
================

Distributions and Ports
-----------------------

Distributions that include AppArmor:

-   [Annvix](http://annvix.org)
-   [Arch Linux](https://www.archlinux.org/)
-   [Debian](http://http://www.debian.org/)
-   [Gentoo](http://www.gentoo.org/)
-   [Mandriva](http://http://www.mandriva.com/)
-   [openSUSE](http://www.opensuse.org) (integrated in default install)
-   [Pardus Linux](http://www.pardus.org.tr)
-   [PLD](http://www.pld-linux.org)
-   [Ubuntu](http://www.ubuntu.com) (integrated in default install)

Any derivatives of these distributions should also have AppArmor available. [Updated RPMS](http://download.opensuse.org/repositories/security:/apparmor/) can be found at the [openSUSE Build Service](http://en.opensuse.org/Build_Service). These are not limited to SUSE distributions.

Source code
-----------

The AppArmor project source is split between the kernel module, available in the Linux kernel and git development tree and the user space tools available in launchpad.

#### Kernel

AppArmor is in the upstream kernel as of 2.6.36. Earlier releases are available in the kernel module git tree:

-   [How to get the AppArmor kernel source](gittutorial)

    Note: the master branch is not stable and will be rebased from time to time. Release branches will be stable and will not be rebased.

The AppArmor v2.4 compatibility patches are available in the stable kernel branches. eg v3.4-aa2.8 or in the release tarballs in the kernel-patches directory.

#### Userspace

Development release: AppArmor 4.0

-   in development
-   targeting
    -   support of gsettings
    -   query caching
    -   extensible rule support so older parsers don't break on new rule types

Current stable release: 2.12.0

-   <https://launchpad.net/apparmor/2.12/2.12.0/+download/apparmor-2.12.tar.gz>
-   sha256sum: 8a2b0cd083faa4d0640f579024be3a629faa7db3b99540798a1a050e2eaba056
-   signature: <https://launchpad.net/apparmor/2.12/2.12.0/+download/apparmor-2.12.tar.gz.asc>
-   [ 2.12.0 release notes](Release_Notes_2.12.0)

Prior supported release: 2.11.1

-   <https://launchpad.net/apparmor/2.11/2.11.1/+download/apparmor-2.11.1.tar.gz>
-   sha256sum: e8e2b22c18e6b6741c1f96942398923b97316b53d86408629f922d5689ec3507
-   signature: <https://launchpad.net/apparmor/2.11/2.11.1/+download/apparmor-2.11.1.tar.gz.asc>
-   [ 2.11.1 release notes](Release_Notes_2.11.1)

Prior supported release: 2.10.3

-   <https://launchpad.net/apparmor/2.10/2.10.3/+download/apparmor-2.10.3.tar.gz>
-   sha256sum: 794a377a93b661c92877f99f386896a2c46730bdf4446ba859343a181a56ba6a
-   signature: <https://launchpad.net/apparmor/2.10/2.10.3/+download/apparmor-2.10.3.tar.gz.asc>
-   [ 2.10.3 release notes](Release_Notes_2.10.3)

Prior release: 2.9.5

-   <https://launchpad.net/apparmor/2.9/2.9.5/+download/apparmor-2.9.5.tar.gz>
-   sha256sum: 007e660323790aaa5fa098f39dbc09c43a074ff17c32cf0dbbb4e492010af485
-   signature: <https://launchpad.net/apparmor/2.9/2.9.5/+download/apparmor-2.9.5.tar.gz.asc>
-   [ 2.9.5 release notes](Release_Notes_2.9.5)

#### Development

tentative release schedule for version 3.0

-   Alpha 1: week of Oct 24.
-   Alpha 2: late November
-   Alpha 3/beta 1: late December
-   beta1/beta2: in mid January
-   beta2/3 start of Feb
-   release: late Feb with possible slip to late march.

<!-- -->

-   [User space tools](https://launchpad.net/apparmor)

    [How to get the AppArmor user space tools](launchpadtutorial)

Profiles
--------

See the [Profiles page](Profiles) for information about AppArmor profiles.

Documentation
=============

AppArmor documentation for the project, including manuals, tutorials, technical documentation and more:

-   [Documentation about the AppArmor security project](Documentation)

Documentation and notes about using AppArmor in a given distribution:

-   [AppArmor on CentOS](Distro_CentOS)
-   [AppArmor on Debian](distro_debian)
-   [AppArmor on Suse](distro_suse)
-   [AppArmor on Ubuntu](distro_ubuntu)

Reporting Bugs
==============

-   Bug Tracking is hosted in Launchpad at <https://bugs.launchpad.net/apparmor>

Joining AppArmor
================

-   [Mailing list](https://lists.ubuntu.com/mailman/listinfo/apparmor)
    for discussing AppArmor development and use.
-   The IRC channel is \#apparmor on irc.oftc.net
-   Bug Tracking - project [apparmor](https://launchpad.net/apparmor) on launchpad.net

Contributions to AppArmor are welcome. Anyone can pull the code
from the git repository or from launchpad, and begin hacking on the
code. Patches can be contributed by posting them to the mailing
list for review. Please see the [CommitPolicy](CommitPolicy) and
[Versioning](Versioning) before sending patches.

If you are a launchpad member, or wish to join launchpad, launchpad
allows creating custom branches of AppArmor and you can submit
merge requests from your own custom branch (see [Using Launchpad
with AppArmor](launchpadtutorial))

Commit privileges to the git tree and launchpad master repository
are restricted, but can be earned by any developer who is involved
in the project.

Meetings are held regularly on the IRC channel and are open to the
everyone. Please see [MeetingAgenda](MeetingAgenda) for times.

What happened to the profile repository?
========================================

[AppArmor profile repository](profile_repo)
