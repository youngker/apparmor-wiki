# Warning WIP release notes for apparmor 3.1 userspace that has not been finalized nor released yet.

Introduction
============

AppArmor 3.1 is a major new release of the AppArmor user space components. It
introduces several new features that are ???
The kernel portion of the project is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.20
kernel.

Note: These release notes cover all changes between 2.13 (f97782b100733770eebc7cf2839ba43683a74f46)
and 3.0 (wip covers up to f2de2952da73bc3fb0190c2cba70365c43a14865) [apparmor-3.0 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-3.0).




Highlighted new features
========================
??? Add links to extended descriptions of new features

- Policy now can use multiple feature abis. This allows for easier support and development of policy

- Multiple policy locations are now supported with support for new locations.

- Read-only policy and cache images are now supported with local modification allowed via overlay locations.

- Policy cache validation now uses fast hashing in addition to time stamps.

- New Init system for loading policy
  - aa-load - a stand alone policy cache loader that can be used by any init system. Also serves as an example for using the library function to allow init systems to directly load apparmor policy.
  - two stage load - early load stage for loading policy cache, second stage for ensuring policy cache is up to date and reloading if necessary.
- support profile modes
  - enforce (default)
  - kill
  - unconfined
  - prompt?
  - audit flags - normal, all, quiet, noquiet, quiet_denied, quiet_allow

- support new rule mode ```access```  quiet access, audit access

- support new rule qualifiers
  - quiet
  - prompt
  - complain

- new keyword ```all``` has place holder for mediation class
  - allow all,
  - deny all,

- Support for new kernel features (requires appropriate features abi tagging in policy)
  - upstream network rules support
  - IMA integration??
  - nnp

- APIs
  - fd based interfaces for querying task confinement and label ...
  - policy revision notification
  -

pam-apparmor
- support for setting up user policy namespace?




Build Infrastructure
--------------------


Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- fix bugs in --config-file handling
- make --config-file so it is no longer required to be the first option passed to the parser
- move default cache location to /var/cache
- ??? fix transition profile name mess
- profile name validation???

Init
----


Library
-------
- APIs


pam_apparmor
------------


Utils
-----


Policy
------

- profiles:
  - support distributions which merge sbin into bin

  - ping: support void linux binary location
  - traceroute: support void linux binary location
  - ntpd
    - allow access to ntp clockstat
    - add openntpd drift and socket files
    - support void linux binary location
  - dnsmasq: add paths for NetworkManager connection sharing

Documentation
-------------


Translations
------------


Tests
-----
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
