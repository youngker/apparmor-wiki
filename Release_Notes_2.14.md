Introduction
============

AppArmor 2.14 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.16
kernel and ubuntu 16.10 kernel with the apparmor 3 development patches.

Note: These release notes cover all changes between 2.13 (f97782b100733770eebc7cf2839ba43683a74f46)
and 2.14 (???).


Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46 fixing CVE-2019-11190) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.

Highlighted new features
========================

- conditional includes are now supported in policy
- multiple cache directories: allowing for a policy cache per kernel feature set. Meaning caches can be retained when booting between different kernel versions.
- overlay cache locations: this allows for distros to ship pre-compiled readonly caches and for a local cache that takes priority when a local change is made.
- updates to build, and documentation to gain the core infrastructure initiative's passing badge https://bestpractices.coreinfrastructure.org/en/projects/1699
    

# Obtaining the Release
These release notes cover all changes between 2.13.2 (af4808b5f6b58946f5c5a4de4b77df5e0eae6ca0) and 2.13.3 (2f9d9ea7e01a115b29858455d3b1b5c6a0bab75c) [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).

Tarball
-   <https://launchpad.net/apparmor/2.13/2.13.3/+download/apparmor-2.13.3.tar.gz>
-   sha256sum: 267053234c68cdb122c5294d7c276b6e2f5fa7e75c6c2d23e3ce69f95d9a7639
-   signature: <https://launchpad.net/apparmor/2.13/2.13.3/+download/apparmor-2.13.3.tar.gz.asc>

# Changes in this Release

# Build Infrastructure
-


# Policy Compiler (a.k.a apparmor\_parser)
-


# Init

-


# Library

-

# pam_apparmor

-


# Utils

-   genprof/logprof

-   aa-status: split profile from exec name
-   aa-nofify



# Policy

-   abstractions
    - 


# Documentation
-

# Translations

-   merge in .de translation updates from launchpad


# Tests

-   parser
    -
-   libapparmor
    -   
-   utils
    -  
-   regression tests
    -   
