Introduction
============

AppArmor 2.11.2 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel.


# Obtaining this Release
These release notes cover all changes between 2.11.1 (8b81fe065f2d8ad93767ebf32afbd3740c6f3818)
and 2.11.2 (???) on the [apparmor-2.11 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.11).

Tarball
-   <https://launchpad.net/apparmor/2.11/2.11.2/+download/apparmor-2.11.2.tar.gz>
-   sha256sum: 
-   signature: <https://launchpad.net/apparmor/2.11/2.11.1/+download/apparmor-2.11.1,tar.gz.asc>


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
- fix cache write message when stdin is used
- fix parser so that cache creation failure doesn't cause load failure
- fix includes to allow white space
- add support for conditional includes
- fix Makefile hardcoded paths to flex and bison
- ignore abi rules

# Library
- fix: remove empty LD_RUN_PATH from libapparmor-perl

Utils
-----
-  genprof/logprof
   - ignore .git in is_skippable_dir()
   - ignore cache.d in is_skippable_dir()
   - fix writing "link subset" rules
   - fix writing alias rules
   - error out on nested child profiles which are not currently supported
   - don't crash if setting printk_ratelimit fails
   - add python3.6 line to utils/logprof.conf
   - add python3.7 to logprof.conf
   - add support for zsh in logprof.conf
   - handle_children(): automatically add m permissions on ix rules
   - FileRule: detect that 'a' is covered by 'w'
   - gracefully handles a missing parser when running the utils tests
   - fix conflicting profile error
   - fix selection order of profiles when saving
   - add basic support for abi rules to the tools

-   aa-notify
    - make message about notify-send package cross-distro compatible
    - set DBUS_SESSION_BUS_ADDRESS, needed by notify-send
    - remove group restrictions
    - Read user's configuration file from XDG_CONFIG_HOME

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
  - support distributions which merge sbin into bin
  - ping: support void linux binary location
  - traceroute: support void linux binary location
  - ntpd
    - allow access to ntp clockstat
    - add openntpd drift and socket files
    - support void linux binary location
  - netstat: allow capability sys_ptrace
  - syslog-ng: fix startup for some configurations
  - dnsmasq
    - add paths for NetworkManager connection sharing
    - add permission to open log files
  - mlmmj-sub fix moderated subscription
  - wireshark update for modern releases
  - allow running Thunderbird wrapper script
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
    - nmbd add missing files
    - smbd add missing pid lock file

- Tunables
  - global: accept seven digit pids
  - add uid and uids kernel var placeholders

- Abstractions
  - add qt5 abstraction
  - add qt5-compose-cache-write abstraction
  - ubuntu-email: add new Thunderbird executable path
  - ubuntu-browsers.d/user-files: disallow access to the dirs of private files
  - private-files: disallow writes to thumbnailer dir (LP: #1788929)
  - private-files-strict: disallow access to the dirs of private files
  - user-files: disallow access to the dirs of private files
  - remove antiquated abstractions/launchpad-integration
  - kde: use qt5 abstration
  - samba: add missing log files
  - gnupg: allow pubring.kbx
  - add mesa abstraction
  - ld.so.conf: Update base abstraction
  - nvidia: allow to create .nv directory
  - ssl
    - add dehydrated certificate location
    - support new location for ssl-params file
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
