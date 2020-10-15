Kernel vs. Userspace versions
=============================

The released versions documented below are for the AppArmor userspace
utils. The apparmor kernel module does not track versions the same way
as it primarily track Linux kernel releases. In general the apparmor
kernel module tries to support old versions of the apparmor userspace
(at this time versions 2.1 - 2.10), and the apparmor userspace supports
the current and previous releases of the kernel.

For new features to be supported, a version of the userspace utils
and a kernel that supports the feature are required. If the apparmor
userspace utils are too old they will fail to recognize the feature
and policy compilation will fail. If the kernel version is to old
either the apparmor utils will compile the policy to what is supported
by the kernel, thus dropping the unsupported feature, or the kernel
will ignore the unsupported feature, or the kernel will reject the
policy load if it is for an abi it does not support.

AppArmor kernel module versions
===============================

There kernel module breaks down into several development epochs.

-   Pre LSM kernel patch. Not upstreamed and lost long ago.
-   apparmor 2.0: LSM rewrite.
-   apparmor 2.1: dfa & and invasive VFS hooks patch
-   apparmor 2.5: creds & LSM path hooks rewrite
-   apparmor 3: labeling - a development series that was a precursor to type splitting. Carried by Ubuntu but never upstreamed
-   apparmor 3.5 - 3.6: stacking which exposes compound task labeling to user interfaces. Carried by Ubuntu but never upstreamed
-   apparmor 4: typesplitting?
-   apparmor 5: Delegation?

The 2.x series reworked the backend several times but kept the same
basic profile model.

The 3.x series transitioned to using a labeling model based on DTE that
allowed for more than one profile to be stored in a label associated
with a subject or object.

The 4.x series finished the transition to a DTE type splitting model,
which is a finer grained evolution of the labeling in the 3.x series.

# What release version does Distro X have
With the caveat that several distros (especially Ubuntu), have distro patches, backports etc on top of their version of apparmor, a mapping of the apparmor packaging to distros can be found [here](https://repology.org/metapackage/apparmor/versions)

# Available Features

The actual set of mediation available depends on both the userspace version and the kernel version available.

[Kernel Feature support](Kernel_Feature_Matrix)

[Userspace Feature support](Userspace_Feature_Matrix)

[Combined Feature support](Combined_Feature_matrix)

Released Versions of AppArmor Userspace Utils
=============================================

[AppArmor 3.0.1](Release_Notes_3.0.1)
-------------------------------------------

-   Release Date: 2020-10-01
-   [Release Notes](Release_Notes_3.0.1)
-   Development target: Bug Fixes

[AppArmor 3.0](Release_Notes_3.0)
-------------------------------------------

-   Release Date: 2020-10-01
-   Shipped in: Debian ?, Ubuntu ?, Suse ?
-   Kernels supported: 4.13 and upwards
-   [Release Notes](Release_Notes_3.0)
-   Development target: improved userspace libraries, new policy models leveraging stacking and namespaces, new upstream kernel module

[AppArmor 2.14](Release_Notes_2.14)
---------------------------------------------

-  Cancelled


[AppArmor 2.13.5](Release_Notes_2.13.5)
---------------------------------------------

-   Release Date: 2020-10-15
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.13.5)
-   Development target: Bug fixes and policy improvements for AppArmor 2.13.4

[AppArmor 2.13.4](Release_Notes_2.13.4)
---------------------------------------------

-   Release Date: 2020-03-12
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.13.4)
-   Development target: Bug fixes and policy improvements for AppArmor 2.13.3

[AppArmor 2.13.3](Release_Notes_2.13.3)
---------------------------------------------

-   Release Date: 2019-06-18
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.13.3)
-   Development target: Bug fixes and policy improvements for AppArmor 2.13.2

[AppArmor 2.13.2](Release_Notes_2.13.2)
---------------------------------------------

-   Release Date: 2018-12-21
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.13.2)
-   Development target: Bug fixes and policy improvements for AppArmor 2.13

[AppArmor 2.13.1](Release_Notes_2.13.1)
---------------------------------------------

-   Release Date: 2018-10-13
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.13.1)
-   Development target: Bug fixes and policy improvements for AppArmor 2.13

[AppArmor 2.13](Release_Notes_2.13)
---------------------------------------------

-   Release Date: 2018-04-15
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.13)
-   Development target: per kernel caches, and overlay directories for caches

[AppArmor 2.12.3](Release_Notes_2.12.3)
---------------------------------------------

-   Release Date: 2019-06-18
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.12.3)
-   Development target: Bug fixes and policy improvements for AppArmor 2.12

[AppArmor 2.12.2](Release_Notes_2.12.2)
---------------------------------------------

-   Release Date: 2018-12-21
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.12.2)
-   Development target: Bug fixes and policy improvements for AppArmor 2.12

[AppArmor 2.12.1](Release_Notes_2.12.1)
---------------------------------------------

-   Release Date: 2018-10-13
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.12.1)
-   Development target: Bug fixes and policy improvements for AppArmor 2.12

[AppArmor 2.12](Release_Notes_2.12)
---------------------------------------------

-   Release Date: 2017-12-25
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.12)
-   Development target: Bug fixes, policy improvements, new feature support

[AppArmor 2.11.3](Release_Notes_2.11.3)
-------------------------------------------------

-   Release Date: 2019-06-18
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.11.3)
-   Development target: Bug fixes and policy improvements for AppArmor 2.11

[AppArmor 2.11.2](Release_Notes_2.11.2)
-------------------------------------------------

-   Release Date: 2018-12-21
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.11.2)
-   Development target: Bug fixes and policy improvements for AppArmor 2.11.1

[AppArmor 2.11.1](Release_Notes_2.11.1)
-------------------------------------------------

-   Release Date: 2017-10-19
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.11.1)
-   Development target: Bug fixes and policy improvements for AppArmor 2.11

[AppArmor 2.11](Release_Notes_2.11)
---------------------------------------------

-   Release Date: 2017-01-08
-   Shipped in: Debian ?, Ubuntu 17.04, Suse ?
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.11)
-   Development target: support basic policy stacking, and lxc when combined with development kernel

[AppArmor 2.10.5](Release_Notes_2.10.5)
-------------------------------------------------

-   Release Date: 2019-06-18
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.10.5)
-   Development target: bug fix release for 2.10

[AppArmor 2.10.4](Release_Notes_2.10.4)
-------------------------------------------------

-   Release Date: 2018-12-21
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.10.4)
-   Development target: bug fix release for 2.10.3

[AppArmor 2.10.3](Release_Notes_2.10.3)
-------------------------------------------------

-   Release Date: 2017-10-19
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.10.3)
-   Development target: bug fix release for 2.10.2

[AppArmor 2.10.2](Release_Notes_2.10.2)
-------------------------------------------------

-   Release Date: 2017-01-08
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.10.2)
-   Development target: bug fix release for 2.10.1

AppArmor 2.10.1
---------------

-   Release Date: 2016-04-20
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.10.1)
-   Development target: bug fix release for 2.10

AppArmor 2.10
-------------

-   Release Date: 2015-07-14
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.10)
-   Development target: libapparmor apis for managing and working with cache loading of apparmor policy into the kernel

[AppArmor 2.9.5](Release_Notes_2.9.5) (Unsupported)
-----------------------------------------------

-   Release Date: 2017-10-18
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.9.5)
-   Development target: bug fix only release over 2.9.4

[AppArmor 2.9.4](Release_Notes_2.9.4) (Unsupported)
-----------------------------------------------

-   Release Date: 2017-01-08
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.9.4)
-   Development target: bug fix only release over 2.9.3

AppArmor 2.9.3 (Unsupported)
--------------

-   Release Date: 2016-04-15
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.9.3)
-   Development target: bug fix only release over 2.9.2

AppArmor 2.9.2 (Unsupported)
--------------

-   Release Date: 2015-04-23
-   Shipped in: Ubuntu 15.04
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.9.2)
-   Development target: bug fix only release over 2.9.1

AppArmor 2.9.1 (Unsupported)
--------------

-   Release Date: 2014-12-15
-   Shipped in: Ubuntu 14.10
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.9.1)
-   Development target: bug fix only release over 2.9.0

AppArmor 2.9.0 (Unsupported)
--------------

-   Release Date: 2014-10-17
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.9.0)
-   Development target: support dbus, unix abstract sockets, ptrace mediation, new python tools

AppArmor 2.8.5 (Unsupported)
--------------

-   Release Date: 2017-10-18
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.8.5)
-   Development target: bug fix only release over 2.8.4, add policy updates

AppArmor 2.8.4 (Unsupported)
--------------

-   Release Date: 2014-10-13
-   Shipped in:
-   Kernels supported: 3.3 and upwards
-   [Release Notes](Release_Notes_2.8.4)
-   Development target: bug fix only release over 2.8.3, add mod\_apparmor regression

AppArmor 2.8.3 (Unsupported)
--------------

-   Release Date: 2014-02-16
-   Shipped in:
-   Kernels supported: 3.3 through 3.6
-   [Features and Release Notes](Release_Notes_2.8.3)
-   Development target: bug fix only release over 2.8.2, new apparmor\_parser --create-cache-dir command line option

AppArmor 2.8.2 (Unsupported)
--------------

-   Release Date: 2013-08-16
-   Shipped in:
-   Kernels supported: 3.3 through 3.6
-   [Features and Release Notes](Release_Notes_2.8.2)
-   Development target: bug fix only release over 2.8.1

AppArmor 2.8.1 (Unsupported)
--------------

-   Release Date: 2013-01-09
-   Shipped in:
-   Kernels supported: 3.3 through 3.6
-   [Features and Release Notes](Release_Notes_2.8.1)
-   Development target: bug fix only release over 2.8.0

AppArmor 2.8.0 (Unsupported)
--------------

-   Release Date: 2012-05-31
-   Shipped in: Ubuntu 12.10, openSUSE 12.2
-   Kernels supported: 3.3, 3.4
-   [Features and Release Notes](Release_Notes_2.8)
-   Development target: incremental improvement over AppArmor 2.7.x, with more code cleanups and bug fixes to the userspace tools. Mount rules, and the start of a new introspection interface in the kernel.

AppArmor 2.7.2 (Unsupported)
--------------

-   Release Date: 2012-01-31
-   Shipped in: Ubuntu 11.10 (Oneiric Ocelot)
-   Kernels supported: 2.6.35 2.6.36 2.6.37 2.6.38 2.6.39 3.0 3.1 3.2
-   [Features and Release Notes](Release_Notes_2.7.1)
-   Development target - Bug Fix release to 2.7.0

AppArmor 2.7.1 (Unsupported)
--------------

-   Not released due to a problem with the generated tarball, superceded by 2.7.2

AppArmor 2.7.0 (Unsupported)
--------------

-   Release Date: 2011-12-15
-   Shipped in: Ubuntu 11.10 (Oneiric Ocelot)
-   Kernels supported: 2.6.35 2.6.36 2.6.37 2.6.38 2.6.39 3.0 3.1 3.2
-   [Features and Release Notes](Release_Notes_2.7)
-   Development target - this is an incremental improvement over AppArmor 2.6.x, with more code cleanups and bug fixes to the userspace tools.

AppArmor 2.6.1 (Unsupported)
--------------

-   Release Date: Mar 24, 2011
-   Shipped in: Ubuntu 11.04 (Natty Narwhal)
-   Kernels supported: 2.6.31 (patched), 2.6.32 (patched), 2.6.33 - 2.6.38
-   [Release Notes](Release_Notes_2.6.1)
-   Development target: this is a bug fix release of AppArmor 2.6 with bug fixes and minor improvements to userspace.

AppArmor 2.6.0 (Unsupported)
--------------

-   Release Date: Feb 24. 2011
-   Shipped in: Ubuntu 11.04 (Natty Narwhal)
-   Kernels supported: 2.6.31 (patched), 2.6.32 (patched), 2.6.33 - 2.6.38
-   [Features and Release Notes](Release_Notes_2.6.0)
-   Development target: this is an incremental improvement over AppArmor 2.5.x, with some more invasive cleanups to the userspace tools.

AppArmor 2.5.2 (Unsupported)
--------------

-   Release Date: Mar 7, 2011
-   Shipped in:
-   Kernels supported: 2.6.31 (patched), 2.6.32 (patched), 2.6.33 - 2.6.38
-   [Release Notes](Release_Notes_2.5.2)
-   Development target: this is a bug fix release of AppArmor 2.5 with Kernel module upstreaming changes, bug fixes and minor improvements to userspace.

AppArmor 2.5.1 (Unsupported)
--------------

-   Release Date: Sep 21, 2010
-   Shipped in: Ubuntu 10.10 (Maverick Meerkat)
-   Kernels supported: 2.6.31 (patched), 2.6.32 (patched), 2.6.33 - 2.6.37
-   [Release Notes](Release_Notes_2.5.1)
-   Development target - this is a bug fix release of AppArmor 2.5 with Kernel module upstreaming changes, bug fixes and minor improvements to userspace

AppArmor 2.5 (Unsupported)
------------

-   Release Date: May 1, 2010
-   Shipped in: Ubuntu 10.04 (Lucid Lynx)
-   Kernels supported: 2.6.31 (patched), 2.6.32 (patched), 2.6.33
-   [Features and Release Notes](Release_Notes_2.5)
-   Development target - this will be a slightly updated version of AppArmor 2.4 with Kernel module upstreaming changes, bug fixes and improvements in the parser dfa generation

AppArmor 2.4 (Unsupported)
------------

-   Release Date: Feb 1, 2010 (generic tarballs made available)
-   Shipped in: opensuse 11.2, Ubuntu 9.10 (Karmic Koala)
-   Kernels supported: 2.6.31, 2.6.32
-   [Features and Release Notes](Release_Notes_2.4)

In this version of AppArmor development of new features was largely halted and the kernel module was rewritten to use the new path\_permission hooks provided by the LSM. This necessitated some changes to user space as well and some features were lost.

-   Features added
    -   Profile names can now contain regular expressions allowing all profile to match against multiple binaries.
    -   pux profile transitions so that x transitions can fall back to unconfined if a profile is not present
    -   Better support of profile namespaces
-   Features lost
    -   The ability for an unconfined process to arbitrarily set a tasks profile
    -   chmod, chown mediation
    -   xattr mediation

AppArmor 2.3.1 (Unsupported)
--------------

-   Release Date:
-   Shipped in: opensuse 11.1, SLES11, Ubuntu 9.04 (Jaunty Jackalope)
-   kernels supported: 2.6.27 2.6.28
-   [Features and Release Notes](Release_Notes_2.3.1)

AppArmor 2.3 (Unsupported)
------------

-   Release Date: June 2008
-   Shipped in: openSUSE 11.0, opensuse 11.1, Ubuntu 8.04 (Hardy Haron), Ubuntu 8.10 (Intrepid Ibex)
-   kernels supported: 2.6.24, 2.6.25
-   [Features and Release Notes](https://web.archive.org/web/20091214065721/http://developer.novell.com/wiki/index.php/Apparmor_2_3)

AppArmor 2.3 builds incrementally one the AppArmor 2.1 release. The main thrust of its development was extending the profile and file rule features.

AppArmor 2.2 (Deprecated)
------------

AppArmor 2.2 was purposely skipped due to versioning conflicts in
the newer version of libapparmor in AppArmor 2.1, which for reasons
unknown was given a version of 2.2 instead of 2.1.x

AppArmor 2.1+ (Deprecated)
--------------------------

-   Release Date:
    -   2.1+
-   Shipped in: Ubuntu 8.04 (Hardy Heron)
-   kernels supported: 2.6.24
-   [AppArmor 2.1+ Features and Release Notes](http://developer.novell.com/wiki/index.php/Apparmor/Ubuntu8.04)

AppArmor 2.1+ is based on 2.1.1 plus some of the development for
2.3. Specifically it contains kernel and parser support for profile
namespaces, link pairs, and file rules conditional upon user. The
tools however do not support any of these features so they are of
limited use.

AppArmor 2.1 SLES10SP2 release (Deprecated)
-------------------------------------------

-   Release Date:
    -   2.1 SP2:
-   Shipped in: SLES10 SP2
-   kernels supported: 2.6.16
-   [Features and Release Notes](http://developer.novell.com/wiki/index.php/Apparmor/SLES10_SP2)

This is a back port of AppArmor 2.1 to SLES10SP2. It has the 2.1
feature set + a modified apparmor\_parser capable of loading both older
2.0/2.0.1 (pcre based) policy and the newer 2.1 (dfa based) policy.

AppArmor 2.1 (Deprecated)
-------------------------

-   Release Date: September 2007
    -   2.1.1 - bug fix for 2.1
    -   2.1.2 - bug fix for 2.1.1
-   Shipped in: openSUSE 10.3, Mandrivia ???
-   kernels supported: 2.6.23, 2.6.24
-   [Features and Release Notes](http://developer.novell.com/wiki/index.php/Apparmor_2_1)

AppArmor 2.0.1 (Deprecated)
---------------------------

-   shipped in: sles9, opensuse 10.1?, Ubuntu 7.10 (Gutsy Gibbon)

AppArmor 2.0 (Deprecated)
-------------------------

AppArmor versions prior to 2.0 exist only in the linux distro Immunix
and are not covered here.

