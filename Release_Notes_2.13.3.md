Introduction
============

AppArmor 2.13.3 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

AppArmor 2.13.3 was released 2019-06-18.


# Obtaining the Release
These release notes cover all changes between 2.13.2 (af4808b5f6b58946f5c5a4de4b77df5e0eae6ca0) and 2.13.3 (2f9d9ea7e01a115b29858455d3b1b5c6a0bab75c) [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).

Tarball
-   <https://launchpad.net/apparmor/2.13/2.13.3/+download/apparmor-2.13.3.tar.gz>
-   sha256sum: 267053234c68cdb122c5294d7c276b6e2f5fa7e75c6c2d23e3ce69f95d9a7639
-   signature: <https://launchpad.net/apparmor/2.13/2.13.3/+download/apparmor-2.13.3.tar.gz.asc>

# Changes in this Release

Translations
--------------
- sync to most up to date language translations available

Build Infrastructure
--------------------
- add files to .gitignore: swig auto generated files for ruby ([MR366][MR366])
- fix libapparmor swig 4 failure 'aa\_log\_record' object has no attribute '\_\_getattr\_\_' ([BUG33][AABUG33])

libapparmor
-----------
- fix segfault in `overlaydirat_for_each()` causing overlaid cache directory failures
- fix segfault when loading policy cache files ([MR348][MR348])
- fix failure to merge overlay directories in some situations ([MR348][MR348])

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- clean up error handling ([dbug921866][dbug921866], [LP1815294][LP1815294])
- fix parsing of target profile NAME in directed transitions “px -> NAME" ([MR334][MR334])
- improve runtime attachment by determine xmatch priority based on smallest DFA match ([MR326][MR326])
- don't skip cache loads just because optimizations flags are specified
  ([MR385][MR385], [LP1820068][LP1820068])

Init
----
- apparmor.systemd: fix minor issues detected by shellcheck ([MR293][MR293])
- ensure error value is returned correctly ([MR352][MR352])

Utils
-----
-  genprof/logprof
  - drop failing corner-case check in logparser.py ([bso1120472][bso1120472], [MR297][MR297])
  - drop unused `get_profile_filename()` from logparser.py ([MR297][MR297])
  - fix error `KeyError: 'logfiles'` when no logprof.conf exists ([MR365][MR365])
  - don't drop later events when user selects to deny a hat ([MR378][MR378])
- update network keyword list and add corresponding tests ([MR350][MR350])


Policy
------
- Profiles
  - dnsmasq:
    - allow peer=libvirtd to support named profile ([MR304][MR304])
    - work around breakage caused by {bin,sbin} alternation ([bso1127073][bso1127073], [MR346][MR346])
    - revert /usr/{bin,sbin}/ alternation in dnsmasq profile name ([bso1127073][bso1127073], [MR346][MR346])
  - dovecot
    - allow FD passing between dovecot and dovecot's anvil ([MR336][MR336])
    - allow chroot'ing the auth processes ([MR336][MR336])
    - let dovecot/anvil rw the auth-penalty socket ([MR336][MR336])
    - auth processes need to read from postfix auth socket ([MR336][MR336])
    - add abstractions/ssl\_certs to lmtp ([MR336][MR336])
    - allow master to use SIGTERM on children that are slow to die ([MR357][MR357])
    - align {pop3,managesieve}-login to imap-login ([MR389][MR389])
  - identd: allow network netlink dgram ([MR353][MR353])
  - lsb\_release profile: new abstraction ([MR154][MR154])
  - mysqld ([MR310][MR310]):
    - add mmap permission for mysqld (4.8 semantic change)
    - allow mysql to determine which cpus are online
    - allow locking of mysql files
  - syslog-ng: add abstractions/python for python-parser ([MR361][MR361])


- Tunables
  - share:
    - make it play well with aliases ([MR300][MR300])
    - fix buggy syntax that broke the ~/.local/share part of the @{user\_share\_dirs} tunable ([LP1816470][LP1816470], [MR344][MR344])

- Abstractions
  - audio:
    - fix alsa settings access
    - grant read access to the system-wide asound.conf ([dbug920669][dbug920669], [MR320][MR320])
    - grant read access to the libao configuration files ([dbug920670][dbug920670], [MR320][MR320])
  - base: allow mr permission on all *.so* common library paths ([MR345][MR345])
  - dri-common: allow reading /dev/dri/ ([AABUG29][AABUG29], [MR382][MR382])
  - fonts:
    - allow to read conf-avail dir itself ([MR165][MR165])
    - allow creating/writing config dirs ([MR165][MR165])
    - add various openSUSE-specific font config directories ([MR309][MR309])
  - gnome:
    - allow reading gtk-3.0 cache files ([MR342][MR342])
    - allow creating config dirs ([MR165][MR165])
  - kde:
    - allow access to common KDE-specific settings ([MR327][MR327])
    - allow access to global KDE settings ([MR327][MR327])
  - ldapclient: allow rw access to the nslcd socket ([LP1575438][LP1575438])
  - mesa:
    - allow reading drirc.d ([MR308][MR308])
    - move dirc.d access to dir-common ([MR314][MR314])
  - nameservice: allow access to /run/netconfig/resolv.conf ([bso1097370][bso1097370])
  - nvidia: allow reading nvidia application profiles ([MR125][MR125])
  - postfix-common: make compatible with updated postfix profiles naming ([MR387][MR387])
  - python: allow reading /usr/local/lib/python3 ([MR171][MR171])
  - qt5: allow reading user configuration ([MR335][MR335])
  - qt5-compose-cache-write: fix anonymous shared memory access ([MR301][MR301])
  - qt5-settings-write: fix anonymous shared memory access ([MR302][MR302])
  - ssl\_certs,keys - add support for libdehydrated in /var/lib/ ([MR299][MR299])
  - ubuntu-browsers.d/multimedia: allow creating/writing config dirs ([MR165][MR165])
  - vulcan: allow reading /etc/vulkan/icd.d/ ([MR329][MR329])


Tests
-----
- fix mount test to use next available loop device ([MR379][MR379])
- update tests to support distros with user-merge where /bin and /sbin are symlinks ([MR331][MR331])
- fix regression test failures around new binary cache layout ([MR348][MR348])
- update tests for new network domain keywords ([MR349][MR349])
- update tests for base abstraction changes ([MR358][MR358])


Documentation
-------------
- apparmor.d (7):
  - update list of network domain keywords ([MR349][MR349])
  - drop unsupported 'to' option for link rules from manpage ([MR368][MR368])

Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.

[AABUG29]: https://gitlab.com/apparmor/apparmor/issues/29
[AABUG33]: https://gitlab.com/apparmor/apparmor/issues/33
[bso1097370]: https://bugzilla.opensuse.org/show_bug.cgi?id=1097370
[bso1120472]: https://bugzilla.opensuse.org/show_bug.cgi?id=1120472
[bso1127073]: https://bugzilla.opensuse.org/show_bug.cgi?id=1127073
[dbug920669]: https://bugs.debian.org/920669
[dbug920670]: https://bugs.debian.org/920670
[dbug921866]: https://bugs.debian.org/921866
[LP1575438]: https://bugs.launchpad.net/bugs/1575438
[LP1815294]: https://bugs.launchpad.net/bugs/1815294
[LP1815294]: https://bugs.launchpad.net/bugs/1815294
[LP1816470]: https://bugs.launchpad.net/bugs/1816470
[LP1820068]: https://bugs.launchpad.net/bugs/1820068
[MR125]: https://gitlab.com/apparmor/apparmor/merge_requests/125
[MR154]: https://gitlab.com/apparmor/apparmor/merge_requests/154
[MR165]: https://gitlab.com/apparmor/apparmor/merge_requests/165
[MR171]: https://gitlab.com/apparmor/apparmor/merge_requests/171
[MR293]: https://gitlab.com/apparmor/apparmor/merge_requests/293
[MR297]: https://gitlab.com/apparmor/apparmor/merge_requests/297
[MR299]: https://gitlab.com/apparmor/apparmor/merge_requests/299
[MR300]: https://gitlab.com/apparmor/apparmor/merge_requests/300
[MR301]: https://gitlab.com/apparmor/apparmor/merge_requests/301
[MR302]: https://gitlab.com/apparmor/apparmor/merge_requests/302
[MR304]: https://gitlab.com/apparmor/apparmor/merge_requests/304
[MR308]: https://gitlab.com/apparmor/apparmor/merge_requests/308
[MR309]: https://gitlab.com/apparmor/apparmor/merge_requests/309
[MR310]: https://gitlab.com/apparmor/apparmor/merge_requests/310
[MR314]: https://gitlab.com/apparmor/apparmor/merge_requests/314
[MR320]: https://gitlab.com/apparmor/apparmor/merge_requests/320
[MR326]: https://gitlab.com/apparmor/apparmor/merge_requests/326
[MR327]: https://gitlab.com/apparmor/apparmor/merge_requests/327
[MR329]: https://gitlab.com/apparmor/apparmor/merge_requests/329
[MR331]: https://gitlab.com/apparmor/apparmor/merge_requests/331
[MR334]: https://gitlab.com/apparmor/apparmor/merge_requests/334
[MR335]: https://gitlab.com/apparmor/apparmor/merge_requests/335
[MR336]: https://gitlab.com/apparmor/apparmor/merge_requests/336
[MR344]: https://gitlab.com/apparmor/apparmor/merge_requests/344
[MR345]: https://gitlab.com/apparmor/apparmor/merge_requests/345
[MR346]: https://gitlab.com/apparmor/apparmor/merge_requests/346
[MR348]: https://gitlab.com/apparmor/apparmor/merge_requests/348
[MR349]: https://gitlab.com/apparmor/apparmor/merge_requests/349
[MR349]: https://gitlab.com/apparmor/apparmor/merge_requests/349
[MR350]: https://gitlab.com/apparmor/apparmor/merge_requests/350
[MR352]: https://gitlab.com/apparmor/apparmor/merge_requests/352
[MR353]: https://gitlab.com/apparmor/apparmor/merge_requests/353
[MR357]: https://gitlab.com/apparmor/apparmor/merge_requests/357
[MR358]: https://gitlab.com/apparmor/apparmor/merge_requests/358
[MR361]: https://gitlab.com/apparmor/apparmor/merge_requests/361
[MR365]: https://gitlab.com/apparmor/apparmor/merge_requests/365
[MR366]: https://gitlab.com/apparmor/apparmor/merge_requests/366
[MR368]: https://gitlab.com/apparmor/apparmor/merge_requests/368
[MR378]: https://gitlab.com/apparmor/apparmor/merge_requests/378
[MR379]: https://gitlab.com/apparmor/apparmor/merge_requests/379
[MR382]: https://gitlab.com/apparmor/apparmor/merge_requests/382
[MR385]: https://gitlab.com/apparmor/apparmor/merge_requests/385
[MR387]: https://gitlab.com/apparmor/apparmor/merge_requests/387
[MR389]: https://gitlab.com/apparmor/apparmor/merge_requests/389
