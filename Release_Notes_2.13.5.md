AppArmor 2.13.5 was released 2020-??-???.

# Introduction

AppArmor 2.13.5 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.


# Obtaining the Release

### Tarball
-   <https://launchpad.net/apparmor/2.13/2.13.5/+download/apparmor-2.13.5.tar.gz>
-   sha256sum: ???
-   signature: <https://launchpad.net/apparmor/2.13/2.13.5/+download/apparmor-2.13.5.tar.gz.asc>

# Changes in this Release

These release notes cover all changes between 2.13.4 [df0ac742f7a1146181d8734d03334494f2015134](https://gitlab.com/apparmor/apparmor/-/commitdf0ac742f7a1146181d8734d03334494f2015134) and 2.13.5 [???](https://gitlab.com/apparmor/apparmor/-/commitdf????) on the [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).


## Build Infrastructure

- Fix build when using Make 4.3  Fixes: https://gitlab.com/apparmor/apparmor/-/issues/74
Fixes: https://bugzilla.opensuse.org/show_bug.cgi?id=1167953 MR:!465
- Fix build for cross compiles MR:!485


- apparmor.vim
  - better handling during creation MR:!472
  - support "if exists" syntax highlighting MR:!500
  - fix syntax highlighting for alias rules MR:!527

## ???
- Fix profile load when using busybox Fixes: https://gitlab.com/apparmor/apparmor/-/issues/80

## libapparmor

- ???


## Policy Compiler (a.k.a apparmor\_parser)

- ???

## Utils

- genprof:
  - Fix json mode when displaying local inactive profile MR:!516
  - Make call out to less use lists to properly handle odd input MR:!520
  - Improve error handling to not fail silently when reading profiles MR:!530
  - Correctly handle missing rules and children profiles MR:!539
  - Fix handling of degenerate quoted strings MR:!545


## Policy
- Tunables
  - Add @{run} variable to support out of tree policies. Fixes:https://gitlab.com/apparmor/apparmor/-/issues/88 MR:!466

- Abstractions
  - dbus-network-manager-strict: support new abstraction MR:!409
  - gnome: allow read of /run/mount/utab MR:!412
  - mdns: allow /etc/mdns.allow Fixes: https://bugs.launchpad.net/ubuntu/+source/apparmor/+bug/1869629
  - mesa: support i915 perf interface MR:464
  - nameservice: nss-systemd, allow access to /run/systemd/userdb/ Fixes:https://gitlab.com/apparmor/apparmor/-/issues/82
  - xdg-open and friends: support new abstraction MR:!404
  - vulkan: fix device and driver enumeration MR:!543



- Profiles
  - dnsmasq:
    - support dnsmasq 2.81 MR:!475
    - allow reading DNSSEC trust anchors MR:!547
  - winbindd: allow krb5 rcache files locking  MR:!460

## Tests

- update tests to use assertEqual instead of the deprecated assertEquals MR:!347
- fix test to run in cross-compiled environment without forcing parser rebuild MR:!465

## Documentation

- ???

# Note

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.


# References

[AABUG???: ???](???)<br>
[dbug???: https://bugs.debian.org/????](https://bugs.debian.org/???)<br>
[lp???: https://bugs.launchpad.net/bugs/???](https://bugs.launchpad.net/bugs/???)<br>
[MR???: https://gitlab.com/apparmor/apparmor/merge_requests/???](https://gitlab.com/apparmor/apparmor/merge_requests/???)<br>
