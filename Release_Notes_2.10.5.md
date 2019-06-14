Introduction
============

AppArmor 2.10.5 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied).

AppArmor 2.10.5 was released 2019-06-15.


# Obtaining the Release
These release notes cover all changes between 2.10.4 (758ec0cdbd1978ff7b82fb0b18abc92687f81f59) and 2.10.5 (????) [apparmor-2.10 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.10).

Tarball
-   <https://launchpad.net/apparmor/2.10/2.10.5/+download/apparmor-2.10.5.tar.gz>
-   sha256sum: ???
-   signature: <https://launchpad.net/apparmor/2.10/2.10.5/+download/apparmor-2.10.5.tar.gz.asc>

# Changes in This Release

Build Infrastructure
--------------------
- add files to .gitignore
  - parser/libapparmor\_re/parse.cc is autogenerated during build
  - parser/tst\_lib gets compiled during "make check"
  - swig auto generated files for ruby
- fix libapparmor swig 4 failure 'aa\_log\_record' object has no attribute '\_\_getattr\_\_'

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- fix abi rule skipping core dump on bad file names
- fix parsing of target profile NAME in directed transitions “px -> NAME"

Policy
------
- Profiles
  - dovecot:
    - allow FD passing between dovecot and dovecot's anvil
    - allow chroot'ing the auth processes
    - let dovecot/anvil rw the auth-penalty socket
    - auth processes need to read from postfix auth socket
    - add abstractions/ssl\_certs to lmtp
    - allow master to use SIGTERM on children that are slow to die
  - identd: allow network netlink dgram
  - syslog-ng: add abstractions/python for python-parser

- Abstractions
  - ssl\_certs,keys: add support for libdehydrated in /var/lib/
  - qt5-settings-write: fix anonymous shared memory access
  - qt5-compose-cache-write: fix anonymous shared memory access
  - fonts:
    - allow to read conf-avail dir itself.
    - add various openSUSE-specific font config directories.
    - nameservice: allow access to /run/netconfig/resolv.conf

Tests
-----
- fix mount test to use next available loop device

