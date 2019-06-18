
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
These release notes cover all changes between 2.12.2 (50aa7293ddbba5bde10065761e9b83764f3d2449) and 2.12.3 (f2fb53c6c3752c5a816035b0561bb16e82f09dd9) [apparmor-2.12 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.12).

Tarball
-   <https://launchpad.net/apparmor/2.12/2.12.3/+download/apparmor-2.12.3.tar.gz>
-   sha256sum: 60e245abfc1b62424b88df1bf8e3aee5aa858c8c12c145882a11e306c6ef8de4
-   signature: <https://launchpad.net/apparmor/2.12/2.12.3/+download/apparmor-2.12.3.tar.gz.asc>

# Changes in This Release

Build Infrastructure
--------------------
- add files to .gitignore: swig auto generated files for ruby ([MR366][MR366])
- fix libapparmor swig 4 failure 'aa\_log\_record' object has no attribute '\_\_getattr\_\_' ([BUG33][AABUG33])

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- clean up error handling ([dbug921866][dbug921866], [LP1815294][LP1815294])
- fix parsing of target profile NAME in directed transitions “px -> NAME" ([MR334][MR334])
- improve runtime attachment by determine xmatch priority based on smallest DFA match ([MR326][MR326])
- don't skip cache just because parser optimizations are specified
  ([MR385][MR385], [LP1820068][LP1820068])

Init
----
- ensure error value is returned correctly ([MR352][MR352])

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

- Profiles
  - dnsmasq:
    - work around breakage caused by {bin,sbin} alternation ([bso1127073][bso1127073], [MR346][MR346])
    - allow peer=libvirtd to support named profile ([MR304][MR304])
  - dovecot:
    - allow FD passing between dovecot and dovecot's anvil ([MR336][MR336])
    - allow chroot'ing the auth processes ([MR336][MR336])
    - let dovecot/anvil rw the auth-penalty socket ([MR336][MR336])
    - auth processes need to read from postfix auth socket ([MR336][MR336])
    - add abstractions/ssl\_certs to lmtp ([MR336][MR336])
    - align {pop3,managesieve}-login to imap-login ([MR389][MR389])
    - allow dovecot-lda to read anything under /usr/share/dovecot/protocols.d/ ([MR386][MR[386])
    - allow lmtp the dac\_read\_search capability ([MR386][MR386])
    - allow master to use SIGTERM on children that are slow to die ([MR357][MR357])
  - mysqld ([MR310][MR310]):
    - add mmap permission for mysqld (4.8 semantic change)
    - allow mysql to determine which cpus are online
    - allow locking of mysql files
  - identd: allow network netlink dgram ([MR353][MR353])
  - syslog-ng: add abstractions/python for python-parser ([MR361][MR361])

- Abstractions
  - audio:
    - fix alsa settings access ([MR303][MR303])
    - grant read access to the libao configuration files ([dbug920670][dbug920670], [MR320][MR320])
    - grant read access to the system-wide asound.conf ([dbug920669][dbug920669], [MR320][MR320])
  - fonts:
    - allow writing to owned fontconfig directories ([MR165][MR165])
    - allow creating owned fontconfig directories ([MR165][MR165])
    - add various openSUSE-specific font config directories ([MR309][MR309])
  - gnome:
    - allow creating gtk-2, gtk-3 config directories ([MR165][MR165])
    - allow read access to gtk-3 cache directory ([MR342][MR342])
  - kde:
    - update kde abstraction for common settings ([MR327][MR327])
    - fix global settings access for Kubuntu and openSUSE ([MR327][MR327])
  - ldapclient: allow read/write access to the nslcd socket ([LP1575438][LP1575438])
  - nameservice: allow /run/netconfig/resolv.conf ([bso1097370][bso1097370])
  - nvidia: allow reading nvidia application profiles ([MR125][MR125])
  - postfix-common: make compatible with latest postfix profiles ([MR387][MR387])
  - python: allow /usr/local/lib/python3 ([MR171][MR171])
  - qt5: read user configuration ([MR335][MR335])
  - qt5-compose-cache-write: fix anonymous shared memory access ([MR301][MR301])
  - qt5-settings-write: fix anonymous shared memory access ([MR302][MR302])
  - ssl\_certs,keys: add support for libdehydrated in /var/lib/ ([MR299][MR299])
  - ubuntu-browsers.d/multimedia: allow creating and writing to owned .adobe directory ([MR165][MR165])
  - vulkan: allow reading /etc/vulkan/icd.d/ ([MR329][MR329])


Tests
-----
- fix various tests to cope with usr-merge ([MR331][MR331])
- fix mount test to use next available loop device ([MR379][MR379])

Documentation
-------------
- update list of network domain keywords in the `apparmor.d` manpage ([MR349][MR349])
- drop `to` option for link rules from the `apparmor.d` manpage ([MR368][MR368])

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
[LP1575438]: https://bugs.launchpad.net/bugs/1575438
[LP1815294]: https://bugs.launchpad.net/bugs/1815294
[LP1820068]: https://bugs.launchpad.net/bugs/1820068
[MR125]: https://gitlab.com/apparmor/apparmor/merge_requests/125
[MR165]: https://gitlab.com/apparmor/apparmor/merge_requests/165
[MR171]: https://gitlab.com/apparmor/apparmor/merge_requests/171
[MR297]: https://gitlab.com/apparmor/apparmor/merge_requests/297
[MR299]: https://gitlab.com/apparmor/apparmor/merge_requests/299
[MR301]: https://gitlab.com/apparmor/apparmor/merge_requests/301
[MR302]: https://gitlab.com/apparmor/apparmor/merge_requests/302
[MR303]: https://gitlab.com/apparmor/apparmor/merge_requests/303
[MR304]: https://gitlab.com/apparmor/apparmor/merge_requests/304
[MR309]: https://gitlab.com/apparmor/apparmor/merge_requests/309
[MR310]: https://gitlab.com/apparmor/apparmor/merge_requests/310
[MR320]: https://gitlab.com/apparmor/apparmor/merge_requests/320
[MR326]: https://gitlab.com/apparmor/apparmor/merge_requests/326
[MR327]: https://gitlab.com/apparmor/apparmor/merge_requests/327
[MR329]: https://gitlab.com/apparmor/apparmor/merge_requests/329
[MR331]: https://gitlab.com/apparmor/apparmor/merge_requests/331
[MR334]: https://gitlab.com/apparmor/apparmor/merge_requests/334
[MR335]: https://gitlab.com/apparmor/apparmor/merge_requests/335
[MR336]: https://gitlab.com/apparmor/apparmor/merge_requests/336
[MR342]: https://gitlab.com/apparmor/apparmor/merge_requests/342
[MR346]: https://gitlab.com/apparmor/apparmor/merge_requests/346
[MR349]: https://gitlab.com/apparmor/apparmor/merge_requests/349
[MR350]: https://gitlab.com/apparmor/apparmor/merge_requests/350
[MR352]: https://gitlab.com/apparmor/apparmor/merge_requests/352
[MR353]: https://gitlab.com/apparmor/apparmor/merge_requests/353
[MR357]: https://gitlab.com/apparmor/apparmor/merge_requests/357
[MR361]: https://gitlab.com/apparmor/apparmor/merge_requests/361
[MR365]: https://gitlab.com/apparmor/apparmor/merge_requests/365
[MR366]: https://gitlab.com/apparmor/apparmor/merge_requests/366
[MR368]: https://gitlab.com/apparmor/apparmor/merge_requests/368
[MR378]: https://gitlab.com/apparmor/apparmor/merge_requests/378
[MR379]: https://gitlab.com/apparmor/apparmor/merge_requests/379
[MR385]: https://gitlab.com/apparmor/apparmor/merge_requests/385
[MR386]: https://gitlab.com/apparmor/apparmor/merge_requests/386
[MR387]: https://gitlab.com/apparmor/apparmor/merge_requests/387
[MR389]: https://gitlab.com/apparmor/apparmor/merge_requests/389
