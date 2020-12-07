AppArmor 2.13.6 was released 2020-12-07.

# Introduction

AppArmor 2.13.6 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

# Obtaining the Release

There are two ways to obtain this release either through gitlab or a tarball in launchpad. Important note: the gitlab release tarballs: Differ from the launchpad release tarballs. The launchpad release tarball has a couple processing steps already performed:
  - libapparmor ```autogen.sh``` is already done, meaning distros only need to use ./configure in their build setup
  - the docs for everything but libapparmor have already been built

### gitlab release
- https://gitlab.com/apparmor/apparmor/-/releases/v2.13.6

### Launchpad Tarball
-   <https://launchpad.net/apparmor/2.13/2.13.6/+download/apparmor-2.13.6.tar.gz>
-   sha256sum: 
-   signature: <https://launchpad.net/apparmor/2.13/2.13.6/+download/apparmor-2.13.6.tar.gz.asc>

# Changes in this Release

These release notes cover all changes between 2.13.5 [ab0f4ab2ed7e734827b143cd32dace4444875e9b](https://gitlab.com/apparmor/apparmor/-/commitab0f4ab2ed7e734827b143cd32dace4444875e9b) and 2.13.6 [c16fff8cb487cf150e3e5ad536b7ff2d4cb4f784](https://gitlab.com/apparmor/apparmor/-/commitc16fff8cb487cf150e3e5ad536b7ff2d4cb4f784)on the [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).


## Libapparmor
- Fix 2.13 libapparmor so version ([MR:658](https://gitlab.com/apparmor/apparmor/-/merge_requests/658))

## Policy Compiler (a.k.a apparmor\_parser)

## Utils
- Check hotkey conflicts case-insensitive ([MR:678](https://gitlab.com/apparmor/apparmor/-/merge_requests/678))
- fix make -C profiles check-logprof fails ([MR:663](https://gitlab.com/apparmor/apparmor/-/merge_requests/663), [AABUG:36](https://gitlab.com/apparmor/apparmor/-/issues/36))
-  aa-autodep: load abstractions on start ([MR:682](https://gitlab.com/apparmor/apparmor/-/merge_requests/682), [BOO:1178527](https://bugzilla.opensuse.org/show_bug.cgi?id=1178527)

## Policy

- Abstractions
  - X
    - Allow (only) reading X compose cache ([MR:685](https://gitlab.com/apparmor/apparmor/-/merge_requests/685))

- Profiles
  - dovecot
    - allow kill signal ([MR:671](https://gitlab.com/apparmor/apparmor/-/merge_requests/671))
    - allow reading dh.pem ([MR:671](https://gitlab.com/apparmor/apparmor/-/merge_requests/671), [debug:10](https://bugs.debian.org/10))
    - backport usr.lib.dovecot.script-login to 2.13 to fix denials in Debian Buster+Bullseye ([MR:672](https://gitlab.com/apparmor/apparmor/-/merge_requests/672))


# Note

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.



