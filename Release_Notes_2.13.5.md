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

- ???


## libapparmor

- ???


## Policy Compiler (a.k.a apparmor\_parser)

- ???

## Utils

- ???



## Policy

- Abstractions
  - ???

- Profiles
  - ???

## Tests

- ???

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