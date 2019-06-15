Introduction
============

AppArmor 2.13.3 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

AppArmor 2.13.3 was released 2019-06-15.


# Obtaining the Release
These release notes cover all changes between 2.13.2 (af4808b5f6b58946f5c5a4de4b77df5e0eae6ca0) and 2.13.3 (????) [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).

Tarball
-   <https://launchpad.net/apparmor/2.13/2.13.3/+download/apparmor-2.13.3.tar.gz>
-   sha256sum: ???
-   signature: <https://launchpad.net/apparmor/2.13/2.13.3/+download/apparmor-2.13.3.tar.gz.asc>

# Changes in this Release

Translations
--------------
- sync to most up to date language translations available

Build Infrastructure
--------------------
- add files to .gitignore
  - swig auto generated files for ruby ([MR366][MR366])
- fix libapparmor swig 4 failure 'aa\_log\_record' object has no attribute '\_\_getattr\_\_' ([BUG33][AABUG33])


libapparmor
-----------
- fix segfault in overlaydirat_for_each causing overlayed cache directory failures
- fix segfault when loading policy cache files
- fix failure to merge overlay directories in some situations

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- clean up error handling ([dbug921866][dbug921866], [LP1815294][LP1815294])
- fix parsing of target profile NAME in directed transitions “px -> NAME"
- improve runtime attachment by determine xmatch priority based on smallest DFA match
- don't skip cache loads just because optimizations flags are specified


Init
----
- apparmor.systemd: fix minor issues detected by shellcheck
- ensure error value is returned correctly ([MR352][MR352])


Utils
-----

-  genprof/logprof
   - Ensure there is always a fallback falue for the logfile location
   - fix handling of log stream when the suggestion of creating a new hat is rejected

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
    - align {pop3,managesieve}-login to imap-login
  - identd
    - allow network netlink dgram
  - syslog-ng
    - add abstractions/python for python-parser
  - lsb_release profile: new abstraction
  - dnsmasq:
    - allow peer=libvirtd to support named profile
    - Work around breakage caused by {bin,sbin} alternation
    - Revert /usr/{bin,sbin}/ alternation in dnsmasq profile name
  - msqld:
    - add mmap permission for mysqld (4.8 semantic change)
    - allow mysql to determine which cpus are online
    - allow locking of mysql files


- Tunables
  - share:
    - make it play well with aliases
    - fix buggy syntax that broke the ~/.local/share part of the @{user_share_dirs} tunable

- Abstractions
  - move dirc.d access from mesa to dir-common
  - base: allow mr permission on all *.so* common library paths
  - dri-common: allow reading /dev/dri/
  - ssl_certs,keys - add support for libdehydrated in /var/lib/
  - qt5: allow reading user configuration
  - qt5-settings-write: fix anonymous shared memory access
  - qt5-compose-cache-write: fix anonymous shared memory access
  - nameservice: allow access to /run/netconfig/resolv.conf
  - mesa: allow reading drirc.d
  - vulcan:  allow reading /etc/vulkan/icd.d/
  - nvidia: allow reading nvidia application profiles
  - postfix-common: make compatible with updated postfix profiles naming
  - python: allow reading /usr/local/lib/python3
  - ldapclient: allow rw access to the nslcd socket
  - ubuntu-browsers.d/multimedia: allow creating/writing config dirs
  - audio:
    - fix alsa settings access
    - grant read access to the system-wide asound.conf
    - grant read access to the libao configuration files
  - fonts:
    - Allow to read conf-avail dir itself.
    - Add various openSUSE-specific font config directories
    - allow creating/writing config dirs
  - kde:
    - allow access to common KDE-specific settings
    - allow access to global KDE settings
  - gnome:
    - allow reading gtk-3.0 cache files
    - allow creating config dirs


Tests
-----
- fix mount test to use next available loop device
- update tests to support distros with user-merge where /bin and /sbin are symlinks
- fix regression test failures around new binary cache layout
- update tests for new network domain keywords
- update tests for base abstraction changes


Documentation
-------------
- apparmor.d (7):
  - update list of network domain keywords
  - drop unsupported 'to' option for link rules from manpage

Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.