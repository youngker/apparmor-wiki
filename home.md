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

-   [Annvix](https://annvix.org)
-   [Arch Linux](https://www.archlinux.org/), documentation and Arch specific [notes](https://wiki.archlinux.org/index.php/AppArmor)
-   [CentOs](https://www.centos.org/), documentation and CentOS specific [notes](Distro_CentOS)
-   [Debian](https://www.debian.org/), documentation and Debian specific [notes](distro_debian)
-   [Gentoo](https://www.gentoo.org/)
-   [openSUSE](https://www.opensuse.org) (integrated in default install), documentation and Suse specific [notes](distro_suse)
-   [Pardus Linux](https://www.pardus.org.tr)
-   [PLD](https://www.pld-linux.org)
-   [Ubuntu](https://www.ubuntu.com) (integrated in default install), documentation and Ubuntu specific [notes](distro_ubuntu)


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

Development release: AppArmor 3.0

-   in development
-   targeting
    -   policy versioning
    -   query caching
    -   extensible rule support so older parsers don't break on new rule types

Current stable release: 2.13.2
-   <https://launchpad.net/apparmor/2.13/2.13.2/+download/apparmor-2.13.2.tar.gz>
-   sha256sum: 844def9926dfda5c7858428d06e44afc80573f9706458b6e7282edbb40b11a30
-   signature: <https://launchpad.net/apparmor/2.13/2.13.2/+download/apparmor-2.13.2,tar.gz.asc>
-   [ 2.13.2 release notes](Release_Notes_2.13.2)

Prior supported release: 2.12.2
-   <https://launchpad.net/apparmor/2.12/2.12.2/+download/apparmor-2.12.2.tar.gz>
-   sha256sum: a316355e9f14c55831856350ef5f4bb99e0a34ecde6f7d7b07719079fdda5866
-   signature: <https://launchpad.net/apparmor/2.12/2.12.2/+download/apparmor-2.12.2.tar.gz.asc>
-   [ 2.12.2 release notes](Release_Notes_2.12.2)

Prior supported release: 2.11.2

-   <https://launchpad.net/apparmor/2.11/2.11.2/+download/apparmor-2.11.2.tar.gz>
-   sha256sum: e22a6369e6d377dbb606fb8e732fe5eef7d7055c9bf4f69e671aa60cda289c74
-   signature: <https://launchpad.net/apparmor/2.11/2.11.2/+download/apparmor-2.11.2.tar.gz.asc>
-   [ 2.11.2 release notes](Release_Notes_2.11.2)

Prior release: 2.10.4

-   <https://launchpad.net/apparmor/2.10/2.10.4/+download/apparmor-2.10.4.tar.gz>
-   sha256sum: 990aafd91905bcced2a8386bc5e45507e847b825c7c693a3c2cbeb5c0b9e7da3
-   signature: <https://launchpad.net/apparmor/2.10/2.10.4/+download/apparmor-2.10.4.tar.gz.asc>
-   [ 2.10.4 release notes](Release_Notes_2.10.4)


#### Development

tentative release schedule for version 3.0

-   Alpha 1: week of May 1.
-   Alpha 2: late May
-   Alpha 3/beta 1: June
-   beta1/beta2: in mid July
-   beta2/3 start of August
-   release: Oct-Nov

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

Reporting Bugs
==============

-   Bug Tracking is hosted in Launchpad at <https://bugs.launchpad.net/apparmor>

Reporting Security Vulnerabilities
==================================

There are 2 ways that security bugs can be reported

### In the bug tracker (preferred)
Security issues should be reported as a bug on launchpad.
-   Bug Tracking is hosted in Launchpad at <https://bugs.launchpad.net/apparmor>

When creating the bug change the
  ```
This bug contains information that is:
  Public
```

to
  ````Private Security````

this will allow **only** you and the apparmor security team to see the bug, until it status is changed to Public Security by either you or the apparmor security team.

### email (no account needed)

If the security issue contains information that is public or can be public. Send an email to

  ```apparmor@lists.ubuntu.com```

Emails to the list from addresses without an account will go into moderation, so there will be a delay before they hit the list but any email that isn't spam will be moderated through. There is no need to signup to be on the mailing list.

If the issue should may need an embargo you can send an email to

   ```security@apparmor.net```


Joining AppArmor
================

-   [Mailing list](https://lists.ubuntu.com/mailman/listinfo/apparmor)
    for discussing AppArmor development and use.
-   The IRC channel is \#apparmor on irc.oftc.net
-   Bug Tracking - project [apparmor](https://launchpad.net/apparmor) on launchpad.net
-   Translations - project [apparmor](https://translations.launchpad.net/apparmor) on launchpad.net
-   Code - project [apparmor](https://gitlab.com/apparmor) on gitlab

Meetings are held regularly on the IRC channel and are open to the
everyone. Please see [MeetingAgenda](MeetingAgenda) for times.

How to Contribute
=================
Contributions to AppArmor are welcome. Anyone can pull the code
from the git repository or from launchpad, and begin hacking on the
code. Patches can be contributed by posting them to the mailing
list for review or submitting a merge request on gitlab. Please see the [CommitPolicy](CommitPolicy), 
[Versioning](Versioning), and [Coding Style](Coding Style) before sending patches.

Commit privileges to the git tree and gitlab master repository
are restricted, but can be earned by any developer who is involved
in the project.


What happened to the profile repository?
========================================

[AppArmor profile repository](profile_repo)
