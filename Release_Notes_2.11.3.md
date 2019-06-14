Introduction
============

AppArmor 2.11.3 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel.

AppArmor 2.11.3 was released 2019-06-15.


# Obtaining the Release
These release notes cover all changes between 2.11.2 (5afe0f8889d5007b7ddb3b2aef287a19e01e656c) and 2.11.3 (????) [apparmor-2.11 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.11).

Tarball
-   <https://launchpad.net/apparmor/2.11/2.11.3/+download/apparmor-2.11.3.tar.gz>
-   sha256sum: ???
-   signature: <https://launchpad.net/apparmor/2.11/2.11.3/+download/apparmor-2.11.3.tar.gz.asc>

# Changes in This Release

Build Infrastructure
--------------------
- add files to .gitignore
  - swig auto generated files for ruby
- fix libapparmor swig 4 failure 'aa_log_record' object has no attribute '__getattr__'

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- fix parsing of target profile NAME in directed transitions “px -> NAME"
- clean up error handling 

Init
----
- ensure error value is returned correctly

Utils
-----
- logprof/genprof:
  - don't drop later events when user selects to deny a hat

Policy
------
- Profiles
  - dovecot
    - allow FD passing between dovecot and dovecot's anvil
    - allow chroot'ing the auth processes
    - let dovecot/anvil rw the auth-penalty socket
    - auth processes need to read from postfix auth socket
    - add abstractions/ssl_certs to lmtp
    - allow master to use SIGTERM on children that are slow to die
  - identd: allow network netlink dgram

- Abstractions
  - ssl_certs,keys: add support for libdehydrated in /var/lib/
  - qt5-settings-write: fix anonymous shared memory access
  -  qt5-compose-cache-write: fix anonymous shared memory access
  - fonts:
    - allow to read conf-avail dir itself
    - add various openSUSE-specific font config directories
  - nameservice: allow access to /run/netconfig/resolv.conf

Tests
-----
- fix mount test to use next available loop device

Documentation
-------------
- drop `to` option for link rules from the `apparmor.d` manpage


