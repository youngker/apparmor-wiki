Introduction
============

AppArmor 2.12.2 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

AppArmor 2.12.2 was released 2018-12-????.


# Obtaining the Release
These release notes cover all changes between 2.12.1 (4633658232827cfeb8d301257d9112cd101c2f7c) and 2.12.2 (?????? ) [apparmor-2.12 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.12).

Tarball
-   <https://launchpad.net/apparmor/2.12/2.12.2/+download/apparmor-2.12.2.tar.gz>
-   sha256sum: ???
-   signature: <https://launchpad.net/apparmor/2.12/2.12.2/+download/apparmor-2.12.2,tar.gz.asc>


Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- Fix failures due to -M only setting compile-features
- Don't hard code the location of netinet/in.h.


Init
----
- Ignore *.orig and *.rej files when loading profiles
- Fix syntax error in rc.apparmor.functions which could cause policy load failures


Utils
-----

-  genprof/logprof
   - Fix viewing a local inactive profile in aa-genprof
   - Ensure last line in a profile is valid   
   - Fix handling of options when serializing profiles
   - Fix minitools for named profiles
   - Fix preview when viewing profile changes


Policy
------
- Use @{sys} tunable in profiles and abstractions

- Profiles
  - Add profile names to all profiles with {bin,sbin} attachment except for the dnsmasq profile

  - dovecot: allow reading /proc/sys/fs/suid_dumpable
  - postalias: allow locking /etc/aliases.db
  - dnsmasq:
    - Add pid file used by NetworkManager
    - Adjust pattern for log files to comply with SELinux


- Abstractions
  - private-files: deny ~/.mutt**
  - private-files-strict: audit deny ~/.aws
  - ssl_key: Add letsencrypt abstraction
  - Add vulkan abstraction

Tests
-----
- error out on superfluous TODOs
- disable abi/ok_10 and abi/ok_12 tests
- Remove TODO notes from no-longer-failing tests
- add utils/test/common_test.pyc to gitignore


Documentation
-------------
- apparmor(7): document various debugging options.
- aa-notify(8): update user's configuration file path


Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.
