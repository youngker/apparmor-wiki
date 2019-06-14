Introduction
============

AppArmor 2.12.3 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

AppArmor 2.12.3 was released 2019-06-15.


# Obtaining the Release
These release notes cover all changes between 2.12.2 (50aa7293ddbba5bde10065761e9b83764f3d2449) and 2.12.3 (????) [apparmor-2.12 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.12).

Tarball
-   <https://launchpad.net/apparmor/2.12/2.12.3/+download/apparmor-2.12.3.tar.gz>
-   sha256sum: ???
-   signature: <https://launchpad.net/apparmor/2.12/2.12.3/+download/apparmor-2.12.3.tar.gz.asc>



Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- ???

Init
----
- ???

Utils
-----

-  genprof/logprof
   - ???

Policy
------
- ???

- Profiles
  - ???

- Abstractions
  - ???


Tests
-----
- ???

Documentation
-------------
- ???

Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.