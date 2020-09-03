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

These release notes cover all changes between 2.13.4 [df0ac742f7a1146181d8734d03334494f2015134](https://gitlab.com/apparmor/apparmor/-/commitdf0ac742f7a1146181d8734d03334494f2015134) and 2.13.5 [???dab520aae9ee59c93307490a88fbde7883029dda](https://gitlab.com/apparmor/apparmor/-/commitdf????) on the [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).


## Build Infrastructure

- Fix build when using Make 4.3  ([MR:465](https://gitlab.com/apparmor/apparmor/merge_requests/465),[BOO:1167953](https://bugzilla.opensuse.org/show_bug.cgi?id=1167953),[AABUG:74](https://gitlab.com/apparmor/apparmor/-/issues/74))
- Fix build for cross compiles ([MR:485](https://gitlab.com/apparmor/apparmor/merge_requests/485))

- profiles
  - fix local dependency on parser ([MR:586](https://gitlab.com/apparmor/apparmor/merge_requests/586),[AABUG:98](https://gitlab.com/apparmor/apparmor/-/issues/98)) 
  - fix dependency check against libapparmor ([MR:586](https://gitlab.com/apparmor/apparmor/merge_requests/586),[AABUG:98](https://gitlab.com/apparmor/apparmor/-/issues/98))

- apparmor.vim
  - better error handling during creation ([MR:472](https://gitlab.com/apparmor/apparmor/merge_requests/472))
  - support "if exists" syntax highlighting ([MR:500](https://gitlab.com/apparmor/apparmor/merge_requests/500))
  - fix syntax highlighting for alias rules ([MR:527](https://gitlab.com/apparmor/apparmor/merge_requests/527))

## Init
- Fix profile load when using busybox ([AABUG:80](https://gitlab.com/apparmor/apparmor/-/issues/80))

## Policy Compiler (a.k.a apparmor\_parser)

- improve error handling in the lexer ([MR:572](https://gitlab.com/apparmor/apparmor/merge_requests/572))
- fix parsing around Whitespace and newline for some rules ([MR:572](https://gitlab.com/apparmor/apparmor/merge_requests/572))
- fix hashing of the feature set ([MR:583](https://gitlab.com/apparmor/apparmor/merge_requests/583))
- support capability BPF and capability perfmon regardless of kernel ([MR:594](https://gitlab.com/apparmor/apparmor/-/merge_requests/594))

## Utils
- genprof/logprof:
  - Fix json mode when displaying local inactive profile ([MR:516](https://gitlab.com/apparmor/apparmor/merge_requests/516))
  - Make call out to less use lists to properly handle odd filenames ([MR:520](https://gitlab.com/apparmor/apparmor/merge_requests/520))
  - Improve error handling to not fail silently when reading profiles ([MR:530](https://gitlab.com/apparmor/apparmor/merge_requests/530))
  - Correctly handle missing rules and children profiles ([MR:539](https://gitlab.com/apparmor/apparmor/merge_requests/539))
  - Fix handling of degenerate quoted strings ([MR:545](https://gitlab.com/apparmor/apparmor/merge_requests/545))
  - handle symlink log events ([MR:587](https://gitlab.com/apparmor/apparmor/merge_requests/587),[AABUG:107](https://gitlab.com/apparmor/apparmor/-/issues/107))
  - add CAPABILITIES BPF and PERFMON to severity.db ([MR:589](https://gitlab.com/apparmor/apparmor/merge_requests/589),[MR:591](https://gitlab.com/apparmor/apparmor/merge_requests/591),[LP1890547](https://bugs.launchpad.net/bugs/1890547))
## Policy
- Tunables
  - Add @{run} variable to support out of tree policies. ([MR:466](https://gitlab.com/apparmor/apparmor/merge_requests/466),[AABUG:88](https://gitlab.com/apparmor/apparmor/-/issues/88))

- Abstractions
  - hosts_access: add to support profiles that are using it ([MR:612](https://gitlab.com/apparmor/apparmor/merge_requests/612))
  - dbus-network-manager-strict: support new abstraction ([MR:409](https://gitlab.com/apparmor/apparmor/merge_requests/409))
  - enchant: support libenchant-2-2 ([MR:572](https://gitlab.com/apparmor/apparmor/merge_requests/572))
  - fonts: update for Debian ([MR:575](https://gitlab.com/apparmor/apparmor/merge_requests/575),[AABUG:94](https://gitlab.com/apparmor/apparmor/-/issues/94))
  - gnome:
    - allow read of /run/mount/utab ([MR:412](https://gitlab.com/apparmor/apparmor/merge_requests/412))
    - gnome: allow read of global settings file /usr/share/gtk-3.0/settings.ini ([MR:592](https://gitlab.com/apparmor/apparmor/merge_requests/592))
  - mdns: allow /etc/mdns.allow ([LP1869629](https://bugs.launchpad.net/ubuntu/+source/apparmor/+bug/1869629))
  - mesa: support i915 perf interface ([MR:464](https://gitlab.com/apparmor/apparmor/merge_requests/464))
  - nameservice: nss-systemd, allow access to /run/systemd/userdb/ ([AABUG:82](https://gitlab.com/apparmor/apparmor/-/issues/82))
  - nvidia_modprobe:
    - allow reading driver parameters ([MR:603](https://gitlab.com/apparmor/apparmor/-/merge_requests/603))
    - allow creating /dev/nvidia-modeset ([MR:603](https://gitlab.com/apparmor/apparmor/-/merge_requests/614))
  - xdg-open and friends: support new abstraction ([MR:404](https://gitlab.com/apparmor/apparmor/merge_requests/404))
  - vulkan: fix device and driver enumeration ([MR:543](https://gitlab.com/apparmor/apparmor/merge_requests/543))
  - X: support X auth path $XDG_RUNTIME_DIR/xauth_XXXXXX ([MR:581](https://gitlab.com/apparmor/apparmor/merge_requests/581))

- Profiles
  - dnsmasq:
    - support dnsmasq 2.81 ([MR:475](https://gitlab.com/apparmor/apparmor/merge_requests/475))
    - allow reading DNSSEC trust anchors ([MR:547](https://gitlab.com/apparmor/apparmor/merge_requests/547))
  - dovecot:
    - allow reading my.cnf ([MR:566](https://gitlab.com/apparmor/apparmor/merge_requests/566))
    - allow switching to user specific hats ([MR:566](https://gitlab.com/apparmor/apparmor/merge_requests/566))
    - fix postfix binary paths ([MR:602](https://gitlab.com/apparmor/apparmor/-/merge_requests/602))
  - postfix:
    - fix postfix binary paths ([MR:602](https://gitlab.com/apparmor/apparmor/-/merge_requests/602))
  - winbindd: allow krb5 rcache files locking  ([MR:460](https://gitlab.com/apparmor/apparmor/merge_requests/460))

## Tests

- fix regression tests regex separator ([MR:599](https://gitlab.com/apparmor/apparmor/-/merge_requests/599))
- update tests to use assertEqual instead of the deprecated assertEquals ([MR:347](https://gitlab.com/apparmor/apparmor/merge_requests/347))
- fix test to run in cross-compiled environment without forcing parser rebuild ([MR:465](https://gitlab.com/apparmor/apparmor/merge_requests/465))
- fix python test dir for select tests ([MR:574](https://gitlab.com/apparmor/apparmor/merge_requests/574))
- profiles
  - fix make check to select tools based on USE_SYSTEM ([MR:580](https://gitlab.com/apparmor/apparmor/merge_requests/580))
  - fix logprof invocation of profiles make check ([MR:586](https://gitlab.com/apparmor/apparmor/merge_requests/586),[AABUG:98](https://gitlab.com/apparmor/apparmor/-/issues/98)) 
- python utils
  - fix libapparmor swig python path ([MR:586](https://gitlab.com/apparmor/apparmor/merge_requests/586),[AABUG:98](https://gitlab.com/apparmor/apparmor/-/issues/98))

# Note

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.


