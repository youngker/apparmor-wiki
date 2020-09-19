# Warning populating the Changes in this release is a WIP will be fully updated in the next few days

AppArmor 3.0 was released 2020-06-07.

# Introduction

AppArmor 3.0 is a major new release of the AppArmor user space that makes an important change to policy development and support. Its focus is transitioning policy to the new features abi and as such other new features have been limited.

Apprmor 3.0 is a bridge release between older AppArmor 2.x policy and the newer AppArmor 3 style policy which requires the declaration of a features abi. As such AppArmor 3.0 will be a short lived release, and will not receive long term support. The following AppArmor 3.1 feature release is planned to be a regular release, please take this into account when including AppArmor 3.0 into a distro release.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.20
kernel.

The kernel portion of the project is maintained and pushed separately.


# Highlighted new features

- Policy now must declare the feature abi it was developed for if it is to use any new features. For further information please see the [wiki](AppArmorpolicyfeaturesabi).
- The use of profile names that are based on pathnames are deprecated. For further information please see the [wiki](DeprecateProfilePathName).
- Support for new kernel features (requires appropriate features abi tagging in policy)
  - upstream v8 network socket rules
  - xattr attachment conditionals
  - capabilities PERFMON and BPF
- compiler improvemetns
  - improved warnings and controls of warnings warnings
- rewritten aa-status
  - supports use in systems/images where python is not available
  - supports kill, unconfined and mixed profile modes
- rewritten aa-notify
  - move from perl to python 3
  - shared backend with other python tools
  - support use of aa.CONFDIR instead of hard coded /etc/apparmor
  - improved message layout
- improved support for kernels that support LSM stacking
- new utility aa-features-abi to extract and work with kernel abi features
- new utility aa-load to load binary policy without calling the apparmor_parser
- support profile modes
  - enforce (default when no mode flag is supplied)
  - kill (experimental)
  - unconfined (experimental)
- reference policy updated for 3.0 feature abi



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
- make --config-file so it is no longer required to be the first option passed to the parser
- move default location of cache to /var/cache/apparmor/ ([MR:156](https://gitlab.com/apparmor/apparmor/-/merge_requests/156),[debug904637](https://bugs.debian.org/904637))
- output deprecated warning when using filenames as a profile name ([MR:506](https://gitlab.com/apparmor/apparmor/-/merge_requests/506))
- Add the ability to turn off jobs to ease with debugging ([MR:105](https://gitlab.com/apparmor/apparmor/-/merge_requests/105))
- Improve warn flags and add support for -Werror ([MR:600](https://gitlab.com/apparmor/apparmor/-/merge_requests/600))
- improve capability handling so it is not variable to which kernel headers are installed ([MR:578](https://gitlab.com/apparmor/apparmor/-/merge_requests/578))
- support experimental enforce, kill and unconfined profile modes ([MR:440](https://gitlab.com/apparmor/apparmor/-/merge_requests/440),[AABUG:7](https://gitlab.com/apparmor/apparmor/-/issues/7))
- add support for out-of-band transitions ([MR:414](https://gitlab.com/apparmor/apparmor/-/merge_requests/414))
- Support xattr exec attachment conditional ([MR:270](https://gitlab.com/apparmor/apparmor/-/merge_requests/270))
- improve static attachment overlap analysis providing better resolution when profile attachments overlap during exec  ([MR:326](https://gitlab.com/apparmor/apparmor/-/merge_requests/326))
- improve cache handling when optimizations are used ([MR:385](https://gitlab.com/apparmor/apparmor/-/merge_requests/385))
- limit the number of passes expr tree simplification does to avoid degenerate cases.
- add missing permissions generation for using new kernel interfaces used by LSM stacking
- fix various build errors, warnings, and refactor code ([MR:196](https://gitlab.com/apparmor/apparmor/-/merge_requests/196),[MR:196](https://gitlab.com/apparmor/apparmor/-/merge_requests/196),[MR:196](https://gitlab.com/apparmor/apparmor/-/merge_requests/196),[MR:616](https://gitlab.com/apparmor/apparmor/-/merge_requests/616),[MR:608](https://gitlab.com/apparmor/apparmor/-/merge_requests/608),[MR:569](https://gitlab.com/apparmor/apparmor/-/merge_requests/569),[MR:590](https://gitlab.com/apparmor/apparmor/-/merge_requests/590),[MR:561](https://gitlab.com/apparmor/apparmor/-/merge_requests/561),[MR:146](https://gitlab.com/apparmor/apparmor/-/merge_requests/146))
- Fix debug builds [MR:609](https://gitlab.com/apparmor/apparmor/-/merge_requests/609)
- remove resource leaks ([MR:439](https://gitlab.com/apparmor/apparmor/-/merge_requests/439))
- fix cross compilation with libintl ([MR:485](https://gitlab.com/apparmor/apparmor/-/merge_requests/485))
- remove deprecated dynamic exception specifications ([MR:356](https://gitlab.com/apparmor/apparmor/-/merge_requests/356))
- Improve error message format ([MR:610](https://gitlab.com/apparmor/apparmor/-/merge_requests/610))



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
