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

Current stable release: 3.0.1
- gitlab release
  - https://gitlab.com/apparmor/apparmor/-/releases/v3.0.1
- launchpad
  -   <https://launchpad.net/apparmor/3.0/3.0.1/+download/apparmor-3.0.1.tar.gz>
  -   sha256sum: 8fada772d9a60989525594346d9aa22af938daafc1781adce9a1acb3c75bdf24
  -   signature: <https://launchpad.net/apparmor/3.0/3.0.1/+download/apparmor-3.0.1.tar.gz.asc>
-   [ 3.0.1 release notes](Release_Notes_3.0.1)

Prior supported release 2.13.6
- gitlab release
  - https://gitlab.com/apparmor/apparmor/-/releases/v2.13.6
- launchpad
  - [code](<https://launchpad.net/apparmor/2.13/2.13.6/+download/apparmor-2.13.6.tar.gz>)
  - [signature](<https://launchpad.net/apparmor/2.13/2.13.6/+download/apparmor-2.13.6.tar.gz.asc>)
  -   sha256sum: 4426b8b73f368889aea69cbc224ae666d4145b9520a2866cc2a915908e87ba8f
-   [ 2.13.6 release notes](Release_Notes_2.13.6)

Prior supported release: 2.12.3
-   <https://launchpad.net/apparmor/2.12/2.12.3/+download/apparmor-2.12.3.tar.gz>
-   sha256sum: 60e245abfc1b62424b88df1bf8e3aee5aa858c8c12c145882a11e306c6ef8de4
-   signature: <https://launchpad.net/apparmor/2.12/2.12.3/+download/apparmor-2.12.3.tar.gz.asc>
-   [ 2.12.3 release notes](Release_Notes_2.12.3)

Prior supported release: 2.11.3

-   <https://launchpad.net/apparmor/2.11/2.11.3/+download/apparmor-2.11.3.tar.gz>
-   sha256sum: 17bcf767925ef4df00bfb20b940e2206fae6e4d959b57398874cd1f1a8d090f0
-   signature: <https://launchpad.net/apparmor/2.11/2.11.3/+download/apparmor-2.11.3.tar.gz.asc>
-   [ 2.11.3 release notes](Release_Notes_2.11.3)

Prior release: 2.10.5

-   <https://launchpad.net/apparmor/2.10/2.10.5/+download/apparmor-2.10.5.tar.gz>
-   sha256sum: 94d6db4789423ed6f025e939b12720e1b8536a1dfabb0ff5f3265a584314bf8a
-   signature: <https://launchpad.net/apparmor/2.10/2.10.5/+download/apparmor-2.10.5.tar.gz.asc>
-   [ 2.10.5 release notes](Release_Notes_2.10.5)



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
