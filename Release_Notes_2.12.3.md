
Introduction
============

AppArmor 2.12.3 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

AppArmor 2.12.3 was released 2019-06-18.


# Obtaining the Release
These release notes cover all changes between 2.12.2 (50aa7293ddbba5bde10065761e9b83764f3d2449) and 2.12.3 (????) [apparmor-2.12 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.12).

Tarball
-   <https://launchpad.net/apparmor/2.12/2.12.3/+download/apparmor-2.12.3.tar.gz>
-   sha256sum: ???
-   signature: <https://launchpad.net/apparmor/2.12/2.12.3/+download/apparmor-2.12.3.tar.gz.asc>

# Changes in This Release

Build Infrastructure
--------------------
- add files to .gitignore:
  - swig auto generated files for ruby ([MR366][MR366])
- fix libapparmor swig 4 failure 'aa\_log\_record' object has no attribute '\_\_getattr\_\_' ([BUG33][AABUG33])

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- clean up error handling ([dbug921866][dbug921866], [LP1815294][LP1815294])
- fix parsing of target profile NAME in directed transitions “px -> NAME"
- improve runtime attachment by determine xmatch priority based on smallest DFA match
- don't skip cache just because parser optimizations are specified

Init
----
- ensure error value is returned correctly ([MR352][MR352])
- ???

Utils
-----
- logprof/genprof:
  - drop failing corner-case check in logparser.py ([bso1120472][bso1120472], [MR297][MR297])
  - drop unused `get_profile_filename()` from logparser.py ([MR297][MR297])
  - fix error `KeyError: 'logfiles'` when no logprof.conf exists ([MR365][MR365])
  - don't drop later events when user selects to deny a hat ([MR378][MR378])
- update network keyword list and add corresponding tests ([MR350][MR350])

Policy
------
- ???

- Profiles
  - dnsmasq: Work around breakage caused by {bin,sbin} alternation ([bso1127073][bso1127073], [MR346][MR346])
  - dovecot:
    - allow FD passing between dovecot and dovecot's anvil ([MR336][MR336])
    - allow chroot'ing the auth processes ([MR336][MR336])
    - let dovecot/anvil rw the auth-penalty socket ([MR336][MR336])
    - auth processes need to read from postfix auth socket ([MR336][MR336])
    - add abstractions/ssl\_certs to lmtp ([MR336][MR336])
    - align {pop3,managesieve}-login to imap-login
    - allow dovecot-lda to read anything under /usr/share/dovecot/protocols.d/
    - allow lmtp the dac_read_search capability
    - allow master to use SIGTERM on children that are slow to die
  - identd: allow network netlink dgram ([MR353][MR353])
  - syslog-ng: add abstractions/python for python-parser

- Abstractions
  - audio: 
    - grant read access to the libao configuration files ([dbug920670][dbug920670], [MR320][MR320])
    - grant read access to the system-wide asound.conf ([dbug920669][dbug920669], [MR320][MR320])
  - fonts: 
    - allow writing to owned fontconfig directories
    - allow creating owned fontconfig directories
  - gnome: 
    - allow creating gtk-2, gtk-3 config directories
    - allow read/write access to gtk-3 config directory
  - kde:
    - update kde abstraction for common settings ([MR327][MR327])
    - fix global settings access for Kubuntu and openSUSE ([MR327][MR327])
  - ldapclient: allow read/write access to the nslcd socket
  - nameservice: allow /run/netconfig/resolv.conf ([bso1097370][bso1097370])
  - nvidia: allow reading nvidia application profiles
  - postfix-common: make compatible with latest postfix profiles
  - python: allow /usr/local/lib/python3
  - qt5: read user configuration
  - ubuntu-browsers.d/multimedia: allow creating and writing to owned .adobe directory
  - vulkan: allow reading /etc/vulkan/icd.d/ ([MR329][MR329])


Tests
-----
- fix various tests to cope with usr-merge ([MR331][MR331])
- fix mount test to use next available loop device ([MR379][MR379])

Documentation
-------------
- update list of network domain keywords in the `apparmor.d` manpage ([MR349][MR349])
- drop `to` option for link rules from the `apparmor.d` manpage ([MR368][MR368])
- ???

Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.

[AABUG33]: https://gitlab.com/apparmor/apparmor/issues/33
[bso1097370]: https://bugzilla.opensuse.org/show_bug.cgi?id=1097370
[bso1120472]: https://bugzilla.opensuse.org/show_bug.cgi?id=1120472
[bso1127073]: https://bugzilla.opensuse.org/show_bug.cgi?id=1127073
[dbug920669]: https://bugs.debian.org/920669
[dbug920670]: https://bugs.debian.org/920670
[dbug921866]: https://bugs.debian.org/921866
[LP1815294]: https://bugs.launchpad.net/bugs/1815294
[MR297]: https://gitlab.com/apparmor/apparmor/merge_requests/297
[MR320]: https://gitlab.com/apparmor/apparmor/merge_requests/320
[MR327]: https://gitlab.com/apparmor/apparmor/merge_requests/327
[MR329]: https://gitlab.com/apparmor/apparmor/merge_requests/329
[MR331]: https://gitlab.com/apparmor/apparmor/merge_requests/331
[MR336]: https://gitlab.com/apparmor/apparmor/merge_requests/336
[MR346]: https://gitlab.com/apparmor/apparmor/merge_requests/346
[MR349]: https://gitlab.com/apparmor/apparmor/merge_requests/349
[MR350]: https://gitlab.com/apparmor/apparmor/merge_requests/350
[MR352]: https://gitlab.com/apparmor/apparmor/merge_requests/352
[MR353]: https://gitlab.com/apparmor/apparmor/merge_requests/353
[MR365]: https://gitlab.com/apparmor/apparmor/merge_requests/365
[MR366]: https://gitlab.com/apparmor/apparmor/merge_requests/366
[MR368]: https://gitlab.com/apparmor/apparmor/merge_requests/368
[MR378]: https://gitlab.com/apparmor/apparmor/merge_requests/378
[MR379]: https://gitlab.com/apparmor/apparmor/merge_requests/379