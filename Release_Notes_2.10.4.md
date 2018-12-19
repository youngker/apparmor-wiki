Introduction
============

AppArmor 2.10.4 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). 

# Obtaining the Release
These release notes cover all changes between 2.10.3 (3004390a6c3e2efe1c2bcfa51e3b0ceef5b39041)
and 2.10.4 (???) on the [apparmor-2.10 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.10).

Tarball
-   <https://launchpad.net/apparmor/2.10/2.10.4/+download/apparmor-2.10.4.tar.gz>
-   sha256sum: 
-   signature: <https://launchpad.net/apparmor/2.10/2.10.4/+download/apparmor-2.10.4.tar.gz.asc>

Build Infrastructure
--------------------
- Update release infrastructure to use gitlab
- Fix $(PWD) when using "make -C profiles"
- exit rather than returning from shell snippets in Makefiles


Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
- disable write cache if filesystem is read-only
- fix cache write message when stdin is used
- fix parser so that cache creation failure doesn't cause load failure
- fix includes to allow white space
- add support for conditional includes
- fix includes to allow white space
- fix Makefile hardcoded paths to flex and bison
- ignore abi rules


# Library
- fix: remove empty LD_RUN_PATH from libapparmor-perl

Utils
-----
- genprof/logprof
  - ignore .git in is_skippable_dir()
  - ignore cache.d in is_skippable_dir()
  - fix writing "link subset" rules
  - fix writing alias rules
  - don't crash if setting printk_ratelimit fails
  - add python3.6 line to utils/logprof.conf
  - add python3.7 to logprof.conf
  - add support for zsh in logprof.conf
  - error out on nested child profiles which are not currently supported
  - fix conflicting profile error
  - add basic support for abi rules to the tools
  - fix crash when hats are uninitialized
  
- aa-notify
  - make message about notify-send package cross-distro compatible
  - set DBUS_SESSION_BUS_ADDRESS, needed by notify-send
  - remove group restrictions
  - Read user's configuration file from XDG_CONFIG_HOME

- sandox.py
  - remove unused exception binding

- aa-decode
  - add the ability to support PROCTITLE string

- aa-remove-unknown
  - Don't print a literal '\n'


Policy
------
- Profiles
  - netstat: allow capability sys_ptrace
  - ntpd allow access to ntp clockstat
  - syslog-ng: fix startup for some configurations
  - mlmmj-sub: fix moderated subscription
  - allow running Thunderbird wrapper script
  - dovecot
    - add dovecot/stats profile, and allow dovecot to run it
    - allow dac_read_search and reading ssl-parameters.dat
    - config: allow dac_read_search and reading ssl-parameters.dat
    - add abstractions/openssl needed by dict
    - add write tempfiles needed by imap
    - add access to the login-master-notify socket needed by managesieve-login
    - fix signal sending for usr.sbin.dovecot
    - auth
      - allow dac_read_search and dac_override
      - allow writing /run/dovecot/old-stats-user
  - samba
    - allow smbd to load new shared libraries
    - allow winbindd to read and write new kerberos cache location
    - nmbd
      - add missing files
      - support writing to /run/systemd/notify
    - smbd add missing pid lock file

- Tunables
  - Add uid and uids kernel var placeholders

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
  - ld.so.conf: Update base abstraction
  - nvidia: allow to create .nv directory
  - php: allow ICU (unicode support) data tables
  - X: add another location for .Xauthority
  - nameservice: allow reading /etc/netconfig
  - ssl
    - add dehydrated certificate location
    - support new location for ssl-params file
  - audio
    - Fix local pulseaudio config file access
    - Allow to read pulseaudio config subdirectories
    - add OpenAL HRTF support
  - Python:
    - add support for python 3.6
    - add support for python 3.7
    - allow /usr/local/lib/python3/dist-packages

Tests
-----
- mount regression test: convert mount test to use MS_NODE
- fix regression tests to pass on 4.14 upstream kernel
- Allow shell helper test read the locale
- mark profiles with multiple rules in one line as known-failing
- add tests for relative path includes
- utils ignore tests for 'include if exists'


Documentation
-------------
- apparmor(7): clarify the effect of reloading a profile.
- Move README to README.md and sync with master branch to support gitlab
- update documentation to references gitlab and updated bug reporting procedures
