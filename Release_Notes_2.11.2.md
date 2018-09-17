Introduction
============

AppArmor 2.11.2 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel.

Note: These release notes cover all changes between 2.11.1 (8b81fe065f2d8ad93767ebf32afbd3740c6f3818)
and 2.11.2 (???).
    


Build Infrastructure
--------------------
- Update release infrastructure to use gitlab
- Fix $(PWD) when using "make -C profiles"
- exit rather than returning from shell snippets in Makefiles
- binutils: honor ${CFLAGS} and ${CPPFLAGS}
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
   - don't crash if setting printk_ratelimit fails
   - add python3.6 line to utils/logprof.conf
   - add python3.7 to logprof.conf
   - handle_children(): automatically add m permissions on ix rules
   - FileRule: detect that 'a' is covered by 'w'
   - gracefully handles a missing parser when running the utils tests
   - fix conflicting profile error
   - fix selection order of profiles when saving

-   aa-notify
    - make message about notify-send package cross-distro compatible
    - set DBUS_SESSION_BUS_ADDRESS, needed by notify-send
    - remove group restrictions

-   sandbox.py
    - remove unused exception binding

-  aa-status
   - split profile from exec name

- aa-decode
  - add the ability to support PROCTITLE string

- aa-remove-unknown
  - Don't print a literal '\n'

Policy
------
- Profiles
  - netstat: allow capability sys_ptrace
  - syslog-ng: fix startup for some configurations
  - ntpd allow access to ntp clockstat
  - mlmmj-sub fix moderated subscription
  - wireshark update for modern releases
  - dovecot
    - add dovecot/stats profile, and allow dovecot to run it
    - allow write to /run/dovecot/old-stats-user
    - allow dac_read_search and reading ssl-parameters.dat
    - config: allow dac_read_search and reading ssl-parameters.dat
    - auth: allow writing /run/dovecot/old-stats-user
    - add stats profile, and allow dovecot to run it
    - lda: allow reading anything under /usr/share/dovecot/protocols.d/
    - fix signal sending for usr.sbin.dovecot
  - samba
    - allow smbd to load new shared libraries
    - allow winbindd to read and write new kerberos cache location

- Tunables
  - global: accept seven digit pids

- Abstractions
  - gnupg: allow pubring.kbx
  - add mesa abstraction
  - ld.so.conf: Update base abstraction
  - nvidia: allow to create .nv directory
  - ssl: add dehydrated certificate location
  - php: allow ICU (unicode support) data tables
  - nameservice: allow reading /etc/netconfig
  - audio:
    - Fix local pulseaudio config file access
    - Allow to read pulseaudio config subdirectories
    - add OpenAL HRTF support
  - Python:
    - add support for python 3.7
    - allow /usr/local/lib/python3/dist-packages
  - X
    - add another location for .Xauthority


Tests
-----
- mount regression test: convert mount test to use MS_NODE
- fix regression tests to pass on 4.14 upstream kernel
- allow shell helper test read the locale
- mark profiles with multiple rules in one line as known-failing
- add tests for relative path includes
- ignore tests for 'include if exists'

Documentation
-------------
- apparmor(7): clarify the effect of reloading a profile.
- README: Document that the parser should be built before the utils
- update documentation to references gitlab and updated bug reporting procedures
