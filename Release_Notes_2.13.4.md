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
These release notes cover all changes between 2.13.3 (2f9d9ea7e01a115b29858455d3b1b5c6a0bab75c) and 2.13.4 (df0ac742f7a1146181d8734d03334494f2015134) on the [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).

Tarball
-   <https://launchpad.net/apparmor/2.13/2.13.4/+download/apparmor-2.13.4.tar.gz>
-   sha256sum: 90bf86c07ffbe2c22be46d75c7345fad12d5911653c59750a37d59c63ad5d10e
-   signature: <https://launchpad.net/apparmor/2.13/2.13.4/+download/apparmor-2.13.4.tar.gz.asc>

# Changes in this Release


Build Infrastructure
--------------------
- Enable continuous integration testing on the 2.13 branch ([MR436])
- Fix autoconf check for Python 3.8 ([MR430])
- Fix vim syntax highlighting for capability ([MR421])
- Fix cross compile by allowing overriding of ar ([issue41])
- Allow installing apparmor_parser to a custom sbin location ([MR111])


libapparmor
-----------
- Fix log parsing for logs with an embedded newline ([MR395])


Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- Fix typo in usage message ([MR406])


Utils
-----
- aa-status
  - handle profile names containing '(' ([MR415])
- genprof/logprof
  - Fix crash due to local includes ([lp1848227], [MR427])
  - Fix crash when log message contains a filename with unbalanced parenthesis ([MR406])



Policy
------
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
  - ssl_certs/ssl_keys
    - add support for cert bot ([MR397])
  - X
    - allow reading the Xauth file that mutter passes to Xwayland ([dbug935058], [MR419])


Tests
-----
- Fix mult_mount test for cases where the block size is greater than 1K ([LP1834192], [MR396])


Documentation
-------------

- apparmor.d (7)
  - fix typos in documentation for dbus and unix rules ([LP1838991], [MR410])
- README.md
  - Document how to use PYFLAKES during testing ([MR429])


Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.
