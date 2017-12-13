WARNING WIP - NOT released targeted to fall 2017
================================================

This release has not happened and these notes will be revised

Introduction
============

AppArmor 4.0 is major release of the user space components of the
AppArmor security project. The kernel portion of the project is
maintained and pushed separately. This release resynchronizes
the versioning used for the kernel module and the userspace
components. While incremental in nature it supports new features that
allow fundamental shifts in how policy is used.

This version of the userspace should work with all kernel versions
from 2.6.15 and later (some earlier version of the kernel if they
have the apparmor patches applied), however dependent on the kernel
used not all features will be supported or available. And supports
features released in the 4.14 kernel and ubuntu 17.10 kernel.

**Note**: These release notes cover all changes between 2.11 and 4.0

What happened to AppArmor 3.x
=============================

3.x versioning was used during the development of the labeling
kernel module that has evolved over time into the 4.0 release. With
the 4.0 release the versioning of the userspace and kernel are being
synchronized to help indicate the relationship between the userspace
tools and the kernel module.

Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy within apparmor 4.0 have
been updated to account for this, but old policy brought forward to
newer kernels may have to be updated.

Highlighted new features
========================

-   extended change\_hat?
-   updated pam\_apparmor?
-   policy versioning and vars?
-   policy update revision support?
-   policy caching?
-   userspace matching?
-   fully virtualized policy namespaces
-   namespace views and scopes
-   default profiles
-   experimental X mediation

Detailed changelog
==================

parser
------

libapparmor
-----------

Utils
-----

aa-status: ?

aa-unconfined: ?

aa-logprof, aa-genprof, aa-mergeprof ?

Policy
------

abstractions: ?

dnsmasq profile: ?

Dovecot profiles: ?

Samba profiles: ?

other profile changes: ?

Documentation
-------------

apparmor.d manpage: ?

Utils: ?

Library manpages: ?

Build: ?

tests
-----

?

other changes
-------------

?
