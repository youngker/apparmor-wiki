Introduction
============

AppArmor 2.13 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.16
kernel and ubuntu 16.10 kernel with the apparmor 3 development patches.

Note: These release notes cover all changes between 2.12 (7f72fd0fcacf8a856b0357261f2f521d90d1bb25)
and 2.13 (???).


Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.

Highlighted new features
========================

- conditional includes are now supported in policy
- multiple cache directories: allowing for a policy cache per kernel feature set. Meaning caches can be retained when booting between different kernel versions.
- overlay cache locations: this allows for distros to ship pre-compiled readonly caches and for a local cache that takes priority when a local change is made.
- updates to build, and documentation to gain the core infrastructure initiative's passing badge https://bestpractices.coreinfrastructure.org/en/projects/1699
    


Detailed changelog
==================

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------

-   add support for multiple policy cache directories
-   add support for overlay cache locations
-   add support for conditional includes
-   separate features used to compile policy and kernel cache features
-   add option to print the cache directory/directories
-   fix error when arg parsing fails
-   drop display_usage() calls after printing an error message
-   fix regression in network mediation when using feature pinning
-   disable write cache if filesystem is read-only and don't abort
-   fix parser so that cache creation failure doesn't cause load failure



Init
----

-   ???

Library
-------

-   ???

Utils
-----

-   ???
    -   ???

Policy
------

-   ???

Documentation
-------------

-   ???

Translations
------------

-   ????

Tests
-----

-   libapparmor
    -   ???
-   regression tests
    -   ???
