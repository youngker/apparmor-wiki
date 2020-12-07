AppArmor 2.10.6 was released 2020-12-07.

# Introduction

AppArmor 2.10.6 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied).

The kernel portion of the project is maintained and pushed separately.


# Important Note
- This is the last release in the 2.10 series.

# Obtaining the Release

There are two ways to obtain this release either through gitlab or a tarball in launchpad. Important note: the gitlab release tarballs: Differ from the launchpad release tarballs. The launchpad release tarball has a couple processing steps already performed:

* libapparmor `autogen.sh` is already done, meaning distros only need to use ./configure in their build setup
* the docs for everything but libapparmor have already been built

### gitlab
- https://gitlab.com/apparmor/apparmor/-/releases/v2.10.6

### Launchpad

  -   <https://launchpad.net/apparmor/2.10/2.10.6/+download/apparmor-2.10.6.tar.gz>
  -   sha256sum: ???
  -   signature: <https://launchpad.net/apparmor/2.0/2.10.6/+download/apparmor-2.10.6.tar.gz.asc>

# Changes in this Release

These release notes cover all changes between 2.10.5 (6a871a5082e7b9997132d152b27783dfea109a36) and 2.10.6 (???) [apparmor-2.10 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.10).


## Library

## Policy Compiler (a.k.a apparmor\_parser)

## aa_status

## aa-notify

## Utils

## Policy

#### abstractions

#### profiles

## Documentation

## Tests

# Note

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.
