# Introduction

AppArmor 2.14 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.16
kernel and ubuntu 16.10 kernel with the apparmor 3 development patches.

Note: These release notes cover all changes between 2.13 (f97782b100733770eebc7cf2839ba43683a74f46)
and 2.14 (???).


## Note

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46 fixing CVE-2019-11190) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.

# Highlighted new features


- 


## Obtaining the Release
These release notes cover all changes between 2.13 (f97782b100733770eebc7cf2839ba43683a74f46) and 2.14 (???) [apparmor-2.14 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.14).

Tarball
-   <https://launchpad.net/apparmor/2.14/2.14.0/+download/apparmor-2.14.tar.gz>
-   sha256sum: 
-   signature: <https://launchpad.net/apparmor/2.14/2.14.0/+download/apparmor-2.14.tar.gz.asc>

## Changes in this Release

Includes all the bug fixes and improvements in
- [2.13.3](Release_Notes_2.13.3)
- [2.13.2](Release_Notes_2.13.2)
- [2.13.1](Release_Notes_2.13.1)

And the following improvements

## Build Infrastructure
-


## Policy Compiler (a.k.a apparmor\_parser)
-


## Init

-


## Library

-

## pam_apparmor

-


## Utils

-   genprof/logprof

-   aa-status: split profile from exec name
-   aa-nofify



## Policy

-   abstractions
    - 


## Documentation
-

## Translations

-   merge in .de translation updates from launchpad


## Tests

-   parser
    -
-   libapparmor
    -   
-   utils
    -  
-   regression tests
    -   
