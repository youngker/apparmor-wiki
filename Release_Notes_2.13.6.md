AppArmor 2.13.6 was released 2020-12-02.

# Introduction

AppArmor 2.13.6 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

# Important Note

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

These release notes cover all changes between 2.13.5 [ab0f4ab2ed7e734827b143cd32dace4444875e9b](https://gitlab.com/apparmor/apparmor/-/commitab0f4ab2ed7e734827b143cd32dace4444875e9b) and 2.13.6 [???](https://gitlab.com/apparmor/apparmor/-/commit???)on the [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).


## Build Infrastructure

## Init

## Libapparmor

## Policy Compiler (a.k.a apparmor\_parser)

## Utils

## Policy
- Tunables

- Abstractions

- Profiles

## Tests


# Note

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.



