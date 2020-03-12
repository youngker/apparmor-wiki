Introduction
============

AppArmor 2.13.4 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

AppArmor 2.13.4 was released 2020-03-12.


# Obtaining the Release
These release notes cover all changes between 2.13.3 (2f9d9ea7e01a115b29858455d3b1b5c6a0bab75c) and 2.13.4 (XXXXX) [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).

Tarball
-   <https://launchpad.net/apparmor/2.13/2.13.4/+download/apparmor-2.13.4.tar.gz>
-   sha256sum: 
-   signature: <https://launchpad.net/apparmor/2.13/2.13.4/+download/apparmor-2.13.4.tar.gz.asc>

# Changes in this Release

Translations
--------------
???
- sync to most up to date language translations available

Build Infrastructure
--------------------
- Enable continuous integration testing on the 2.13 branch ([MR436])
- Fix autoconf check for Python 3.8 ([MR430])
- Fix vim syntax highlighting for capability ([MR421])
- Fix cross compile by allowing overriding of ar ([issue41])

libapparmor
-----------
- Fix log parsing for logs with an embedded \n ([MR395])

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- Fix typo in usage message ([MR406])

Init
----
???

- apparmor.systemd: fix minor issues detected by shellcheck ([MR293][MR293])
- ensure error value is returned correctly ([MR352][MR352])

Utils
-----
- aa-status
  - handle profile names containing '(' ([415])
- genprof/logprof
  - Fix crash due to local includes ([lp1848227], [MR427])
  - Fix crash when log message contains a filename with unbalanced parenthesis ([406])



Policy
------
???

- Profiles


- Tunables

- Abstractions
  - authentication
    - add support for /usr/etc used by some RO root images ([boo1153162], [MR426]) 
  - base
    - allow access to /run/uuidd/request ([MR445])
    - allow access to top level ecryptfs directories ([LP1848919], [MR443])
    - allow reading per-user themes from $XDG_DATA_HOME ([MR442])
  - fonts
    - don't allow writing to fontconfig cache ([MR420])
  - gnome
    - allow access to xdg mimeapps.list ([LP1792027], [MR444])
  - kerberosclient
    - allow reading /etc/krb5.conf.d/ ([MR425])
  - X
    - allow reading the Xauth file that mutter passes to Xwayland ([dbug935058], [MR419])





??ex    - grant read access to the system-wide asound.conf ([dbug920669][dbug920669], [MR320][MR320])

Tests
-----

????- fix mount test to use next available loop device ([MR379][MR379])


Documentation
-------------

- apparmor.d (7)
  - fix typos in documentation for dbus and unix rules ([LP1838991], [MR410])
- README.md
  - Document how to use PYFLAKES during testing ([MR429])


???
- apparmor.d (7):
  - update list of network domain keywords ([MR349][MR349])
  - drop unsupported 'to' option for link rules from manpage ([MR368][MR368])

Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.
