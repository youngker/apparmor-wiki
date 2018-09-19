Introduction
============

AppArmor 2.12.1 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

Note: These release notes cover all changes between 2.12 (7f72fd0fcacf8a856b0357261f2f521d90d1bb25)
and 2.12.1 (???) [apparmor-2.12 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.12).


Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.

    


Build Infrastructure
--------------------
- Update release infrastructure to use gitlab
- Fix $(PWD) when using "make -C profiles"
- coverity
   - capture separate log files for each coverity invocatio
   - support python scan


Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- disable write cache if filesystem is read-only
- fix parser so that cache creation failure doesn't cause load failure
- fix includes to allow white space
- add support for conditional includes


Utils
-----
-  genprof/logprof
   - ignore .git in is_skippable_dir()
   - fix writing "link subset" rules
   - fix writing alias rules
   - error out on nested child profiles which are not currently supported
   - fix unsetting filename in get_profile()
   - don't crash if setting printk_ratelimit fails
   - fix overwriting of child profile flags if they differ from the main profile
   - add python3.7 to logprof.conf

-   aa-notify
    - make message about notify-send package cross-distro compatible
    - set DBUS_SESSION_BUS_ADDRESS, needed by notify-send
    - remove group restrictions

-   sandbox.py
    - remove unused exception binding

-  aa-status
   - split profile from exec name

Policy
------
- Profiles
  - support distributions which merge sbin into bin
  - ping: support void linux binary location
  - traceroute: support void linux binary location
  - ntpd
    - allow access to ntp clockstat
    - add openntpd drift and socket files
    - support void linux binary location
  - mlmmj-sub fix moderated subscription
  - dnsmasq
    - allow chown capability.
    - add paths for NetworkManager connection sharing
  - Fix ubuntu-browsers for 64bit openSUSE
  - dovecot
     - add dovecot/stats profile, and allow dovecot to run it
     - allow write to /run/dovecot/old-stats-user
     - allow dac_read_search and reading ssl-parameters.dat
  - samba
    - allow smbd to load new shared libraries
    - allow winbindd to read and write new kerberos cache location

- Abstractions
  - gnupg: allow pubring.kbx
  - ld.so.conf: Update base abstraction
  - nvidia: allow to create .nv directory
  - ssl: add dehydrated certificate support
  - php: allow ICU (unicode support) data tables
  - Python:
    - add support for python 3.7
    - allow /usr/local/lib/python3/dist-packages


Tests
-----
- mount regression test: convert mount test to use MS_NODE
- add tests for relative path includes
- fix regression tests to pass on 4.14 upstream kernel
- allow shell helper test read the locale
- mark profiles with multiple rules in one line as known-failing
- ignore tests for 'include if exists'

Documentation
-------------
- apparmor(7): clarify the effect of reloading a profile.
- fix typo in apparmor_parser.pod
- update documentation to references gitlab and updated bug reporting procedures.


