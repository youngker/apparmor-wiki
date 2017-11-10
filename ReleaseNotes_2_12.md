Introduction
============

AppArmor 2.12 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.8
kernel and ubuntu 16.10 kernel with the apparmor 3 development patches.

Note: These release notes cover all changes between 2.11 (r3613)
and 2.12 (up to r3724???currently) There aren't any release notes
for 2.11.95 aka 2.12 beta1.

Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 have been updated to support running on pre 4.8 and 4.8+ kernels.

Highlighted new features
========================

-   Reworked Yast interface
-   full unix rule support???

Detailed changelog
==================

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------

-   Fix af\_unix downgrade of network rules
-   Fix delete after new\[\]
-   Set parser executable path according to USE\_SYSTEM make variable
-   update cache handling to make it consistent
-   update ignored files

Init
----

-   Preserve unknown profiles when restarting apparmor init/job/unit CVE-2017-6507

Library
-------

-   fix swig test\_apparmor.py for zero length ptrace records
-   Don't print shell commands that check for test failures
-   Fix parallel make dependency issue in testsuite

Utils
-----

-   Add aa-remove-unknown utility to unload unknown profiles
-   aa-notify - update to use normal urgency notifications to obtain intended behavior across DEs
-   Ignore ptrace log events without denied\_mask
-   Fix aa-logprof crash on ptrace garbage log events
-   Fix regressions caused by init\_aa()
-   apparmor.easyprof update
    -   Fix import in test-aa-easyprof.py
    -   Add option to specify the apparmor\_parser path
-   Set parser base path according to USE\_SYSTEM make variable
-   Accept parser base and include options in aa-easyprof
-   Update the logprof.conf in the test dir to point to in-tree paths
-   Improve error messages when profiles/parser is not found
-   Don't enforce ordering of dbus rule attributes
-   Fix failing tests in test-aa.py
-   Ignore change\_hat events with error=-1 and “unconfined can not change\_hat”
-   Remove re.LOCALE flag
-   update how questions are asked in profile generation
-   honor ${CFLAGS} and ${CPPFLAGS}
-   stop rewriting shbang lines in setup script
-   Improve explanation messages
-   cleanup dead code and several bug fixes
-   Prevent 'wa' conflicts for file rules
-   Carry over all autodep-generated rules in handle\_children()
-   Python 3.6 support
-   Add network 'smc' keyword in NetworkRule
-   rework ruletypes code
-   rework profile storage code
-   YaST
    -   Add a new JSON interface for interacting with YaST
    -   Remove old YaST communication code
    -   Fix save\_profiles() for YaST
-   Bugfixes
    -   Fix crash in serialize\_profile\_from\_old\_profile()
    -   Fix save\_profiles() for YaST

Policy
------

-   Abstractions
    -   Apache2 - profile updates for proper signal handling, optional saslauth, and OCSP stapling
    -   base
        -   Allow sysconf(\_SC\_NPROCESSORS\_CONF)
        -   Allow additional journald sockets
    -   fix for non-latin file/directory names
    -   freedesktop.org - support /usr/local/applications; support subdirs of applications folder
    -   glibc uses /proc/\*/auxv and /proc/\*/status files
    -   gnome: allow reading GLib schemas.
    -   nvidia - Update nvidia for newer nvidia drivers
    -   perl-base - adjust the multiarch alternation rule in the perl abstraction for modern Debian and Ubuntu systems
    -   python - Adjust for python3.6
    -   Rename global variable “pid” to “log\_pid”
    -   Enable camera access in browser apparmor profile for WebRTC
    -   fix permissions matching for seven digit pids
    -   nameservice
        -   allow access to /etc/netconfig
    -   ubuntu-browsers - support Debian's Firefox non-ESR path.
    -   support both the the old-style /usr/lib/firefox/firefox.sh wrapper and the current /usr/lib/firefox{,-esr}/firefox{,-esr} paths.
    -   wayland - allow wayland-cursor-shared-\* <https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=870807>
-   dovecot
    -   Allow /var/run/dovecot/login-master-notify\* in dovecot imap-login profiles
    -   add the attach\_disconnected flag
    -   change Px to mrPx for /usr/lib/dovecot/\*
    -   dovecot-lda update
        -   the attach\_disconnected flags
        -   read access to /usr/share/dovecot/protocols.d/
        -   rw for /run/dovecot/auth-userdb
-   update netstat profile
    -   allow reading @{PROC}/@{pid}/net/netstat and @{PROC}/@{pid}/net/snmp
    -   drop owner conditional - /proc/\*/net/\* is always owned by root, and the owner conditional means breaking netstat for non-root users
    -   drop “@{PROC}/@{pids}/fd r,” - /proc/\*/fd is a directory, so this rule would never apply
-   Postfix
    -   change abstractions/postfix-common to allow /etc/postfix/\*.db k
    -   add several permissions to postfix/error, postfix/lmtp and postfix/pipe
    -   remove superfluous abstractions/kerberosclient from all postfix profiles - it's included via abstractions/nameservice
-   Samba profile updates for ActiveDirectory / Kerberos
-   sshd - drop local/ include
-   traceroute - support TCP SYN for probes, quite net\_admin request
-   /etc/cron.daily/logrotate update

Documentation
-------------

-   Add network 'smc' keyword to apparmor.d manpage
-   Add --no-reload to various utils manpages
-   aa-status - update manpage for updated podchecker
-   aa-enabled - update manpage

Translations
------------

-   update Indonesian translation
-   update german translation
-   update english translation
-   update spanish translation
-   update swedish translation

Tests
-----

-   libapparmor
    -   fix ptrace regression test failure
    -   extend ptrace log parsing tests
-   Ignore test failures about duplicated conditionals in dbus rules
-   readdir - test both getdents() and getdents64() if available
-   where necessary use getdents64 to fix arm64 build failure
-   No longer skip testing generated\_perms\_leading profiles
-   fix unix policy language tests
-   fix regression test failures on really slow machines <http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=699774>
-   regression tests
    -   document test infrastructure requires and features api
    -   improve test status information messages
    -   fix longpath regression failures on newer kernels
    -   fix query interface tests to reflect fixes in kernels that support mult-transaction
    -   fix pivot root domain transition tests to be conditional on kernel support
    -   fix environ fail case

