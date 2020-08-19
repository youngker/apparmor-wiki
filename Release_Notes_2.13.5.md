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

These release notes cover all changes between 2.13.4 [df0ac742f7a1146181d8734d03334494f2015134](https://gitlab.com/apparmor/apparmor/-/commitdf0ac742f7a1146181d8734d03334494f2015134) and 2.13.5 [???cd4a161350a97a42a07dc7f8acb3f4251c4ba11b](https://gitlab.com/apparmor/apparmor/-/commitdf????) on the [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).


## Build Infrastructure

- Fix build when using Make 4.3  Fixes: https://gitlab.com/apparmor/apparmor/-/issues/74
Fixes: https://bugzilla.opensuse.org/show_bug.cgi?id=1167953 ([MR:465](https://gitlab.com/apparmor/apparmor/merge_requests/465))
- Fix build for cross compiles MR:!485

- profiles
  - fix local dependency on parser MR:!586 Fixes:https://gitlab.com/apparmor/apparmor/-/issues/98
  - fix dependency check against libapparmor MR:!586 Fixes:https://gitlab.com/apparmor/apparmor/-/issues/98

- apparmor.vim
  - better handling during creation MR:!472
  - support "if exists" syntax highlighting MR:!500
  - fix syntax highlighting for alias rules MR:!527
  - add CAPABILITIES BPF and PERFMON to severity.db MR:!589 LP:https://bugs.launchpad.net/bugs/1890547

## Init
- Fix profile load when using busybox Fixes: https://gitlab.com/apparmor/apparmor/-/issues/80

## Policy Compiler (a.k.a apparmor\_parser)

- improve error handling in the lexer MR:!572
- fix parsing around Whitespace and newline for some rules MR:!572
- fix hashing of the feature set MR:!583

## Utils
- genprof/logprof:
  - Fix json mode when displaying local inactive profile MR:!516
  - Make call out to less use lists to properly handle odd input MR:!520
  - Improve error handling to not fail silently when reading profiles MR:!530
  - Correctly handle missing rules and children profiles MR:!539
  - Fix handling of degenerate quoted strings MR:!545
  - handle symlink log events MR:!587 Fixes https://gitlab.com/apparmor/apparmor/-/issues/107

## Policy
- Tunables
  - Add @{run} variable to support out of tree policies. Fixes:https://gitlab.com/apparmor/apparmor/-/issues/88 MR:!466

- Abstractions
  - dbus-network-manager-strict: support new abstraction MR:!409
  - enchant: support libenchant-2-2 MR:!572
  - fonts: update for Debian MR:!575 Fixes:#94
  - gnome: allow read of /run/mount/utab MR:!412
  - mdns: allow /etc/mdns.allow Fixes: https://bugs.launchpad.net/ubuntu/+source/apparmor/+bug/1869629
  - mesa: support i915 perf interface MR:464
  - nameservice: nss-systemd, allow access to /run/systemd/userdb/ Fixes:https://gitlab.com/apparmor/apparmor/-/issues/82
  - xdg-open and friends: support new abstraction MR:!404
  - vulkan: fix device and driver enumeration MR:!543
  - X: support X auth path $XDG_RUNTIME_DIR/xauth_XXXXXX MR:!581

- Profiles
  - dnsmasq:
    - support dnsmasq 2.81 MR:!475
    - allow reading DNSSEC trust anchors MR:!547
  - dovecot:
    - allow reading my.cnf MR:!566
    - allow switching to user specific hats MR:!566
  - winbindd: allow krb5 rcache files locking  MR:!460

## Tests

- update tests to use assertEqual instead of the deprecated assertEquals MR:!347
- fix test to run in cross-compiled environment without forcing parser rebuild MR:!465
- fix python test dir for select tests MR:!562
- profiles
  - fix make check to select tools based on USE_SYSTEM MR:!580
  - fix logprof invocation of profiles make check MR:!586 Fixes:https://gitlab.com/apparmor/apparmor/-/issues/98
- python utils
  - fix libapparmor swig python path MR:!586 Fixes:https://gitlab.com/apparmor/apparmor/-/issues/98

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
