# Warning WIP will be fully updated in the next few days

AppArmor 3.0 was released 2020-04-29.

# Introduction

AppArmor 3.0 is a major new release of the AppArmor user space that makes an important change to policy development and support. Its focus is transitioning policy to the new features abi and as such other new features have been limited.

Apprmor 3.0 is a bridge release between older AppArmor 2.x policy and the newer AppArmor 3.x style policy which requires the declaration of a features abi. As such AppArmor 3.0 will be a short lived release, and will not receive long term support. The following AppArmor 3.1 feature release is planned to be a regular release, please take this into account when including AppArmor 3.0 into a distro release.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.20
kernel.

The kernel portion of the project is maintained and pushed separately.


# Highlighted new features

- Policy now must declare the feature abi it was developed for if it is to use any new features. 

- support profile modes
  - enforce (default)
  - kill
  - unconfined
 
- Support for new kernel features (requires appropriate features abi tagging in policy)
  - upstream v8 network socket rules
  - xattr attachment conditionals

- rewritten aa-status
  - supports use in systems/images where python is not available
  - supports kill, unconfined and mixed profile modes

- improved support for kernels that support LSM stacking


# Obtaining the Release
These release notes cover all changes between 2.13 (f97782b100733770eebc7cf2839ba43683a74f46) and 3.0 (???) [apparmor-3.0 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-3.0).

Tarball
-   <https://launchpad.net/apparmor/3.0/3.0/+download/apparmor-3.0.tar.gz>
-   sha256sum: 
-   signature: <https://launchpad.net/apparmor/3.0/3.0/+download/apparmor-3.0.tar.gz.asc>

# Changes in this Release

Includes all the bug fixes and improvements in
- [2.13.1](Release_Notes_2.13.1)
- [2.13.2](Release_Notes_2.13.2)
- [2.13.3](Release_Notes_2.13.3)
- [2.13.4](Release_Notes_2.13.4)
- [2.13.5](Release_Notes_2.13.5)


And the following improvements

## Build Infrastructure



## Policy Compiler (a.k.a apparmor\_parser)

- fix bugs in --config-file handling
- make --config-file so it is no longer required to be the first option passed to the parser
- move default cache location to /var/cache
- ??? fix transition profile name mess
- profile name validation???

## Init



## Library

- APIs



## Utils



## Policy

- profiles:
  - support distributions which merge sbin into bin

  - ping: support void linux binary location
  - traceroute: support void linux binary location
  - ntpd
    - allow access to ntp clockstat
    - add openntpd drift and socket files
    - support void linux binary location
  - dnsmasq: add paths for NetworkManager connection sharing

## Documentation



## Translations



## Tests

- use --config-file in tests so they are unaffected by the system parser.conf file

-   parser
-   libapparmor
-   utils
-   regression tests


Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.
