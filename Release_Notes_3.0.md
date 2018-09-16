Introduction
============

AppArmor 3.0 is a major new release of the AppArmor user space components. It
introduces several new features that are ???
The kernel portion of the project is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.20
kernel.

Note: These release notes cover all changes between 2.13 (????)
and 3.0 (???).


Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.


Highlighted new features
========================

- Policy now must declare the feature abi it was developed for if it is to use any new features. 

- Policy in multiple locations is now supported with a way to easily add new locations.

- Overlay locations are supported for policy and cache.

- New Init system for loading policy
  aa-load

- upstream network rules support

- IMA integration??




Detailed changelog
==================

Build Infrastructure
--------------------


Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------


Init
----


Library
-------
- APIs


pam_apparmor
------------


Utils
-----


Policy
------


Documentation
-------------


Translations
------------


Tests
-----
-   parser
-   libapparmor
-   utils
-   regression tests


