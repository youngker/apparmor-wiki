Introduction
============

AppArmor 2.12 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.8
kernel and ubuntu 16.10 kernel with the apparmor 3 development patches.

Note: These release notes cover all changes between 2.11 (bzr r3613)
and 2.12 (7f72fd0fcacf8a856b0357261f2f521d90d1bb25, 2017-12-24).
There aren't any release notes for 2.11.95 aka 2.12 beta1.

Also note that this is the first release after switching from launchpad/bzr
to gitlab.

Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 have been updated to support running on pre 4.8 and 4.8+ kernels.

Highlighted new features
========================

-   Reworked Yast interface (aa-logprof --json and aa-genprof --json)
-   Add support for 'owner' events to aa-logprof and aa-genprof <https://bugs.launchpad.net/apparmor/+bug/1538340>
-   Add support for includes with absolute paths to the python tools <https://bugs.launchpad.net//apparmor/+bug/1733700>

Detailed changelog
==================

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------

-   Fix af\_unix downgrade of network rules
-   Fix delete after new\[\]
-   Set parser executable path according to USE\_SYSTEM make variable
-   update cache handling to make it consistent
-   update ignored files
-   partially address issues building with musl

Init
----

-   Preserve unknown profiles when restarting apparmor init/job/unit CVE-2017-6507

Library
-------

-   fix swig test\_apparmor.py for zero length ptrace records
-   Don't print shell commands that check for test failures
-   Fix parallel make dependency issue in testsuite
-   Preserve errno across aa_*_unref() functions

Utils
-----

-   Add aa-remove-unknown utility to unload unknown profiles
-   Add support for 'owner' events to aa-logprof and aa-genprof (actually re-add this - it was supported with the years-old previous audit.log format) <https://bugs.launchpad.net/apparmor/+bug/1538340>
-   aa-decode: add the ability to support PROCTITLE string <https://bugs.launchpad.net/apparmor/+bug/1736841>
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
-   FileRule: detect that 'a' is covered by 'w' <https://bugs.launchpad.net/apparmor/+bug/1385474>
-   Carry over all autodep-generated rules in handle\_children()
-   Python 3.6 support
-   Add network 'smc' keyword in NetworkRule
-   rework ruletypes code
-   rework profile storage code
-   automatically add mr when creating ix rules
-   YaST
    -   Add a new JSON interface for interacting with YaST (aa-logprof --json, aa-genprof --json)
    -   Remove old YaST communication code
    -   Fix save\_profiles() for YaST
-   Bugfixes
    -   Fix crash in serialize\_profile\_from\_old\_profile()
    -   Fix save\_profiles() for YaST
    -   Fix sorted() regression in save\_profiles() to get the displayed and
        the internal numbering in sync again
    -   Remember selected profile in save\_profiles()
    -   Let read\_inactive\_profiles() do nothing when calling it the second time

Policy
------

-   Abstractions
    -   Apache2
        - profile updates for proper signal handling, optional saslauth, and OCSP stapling
        - add attach\_disconnected flag <https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=875892>
    -   audio:
		-   allow openAL HRTF support <https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=874665>
		-	allow reading \*.conf files in /etc/pulse/ and ~/.config/pulse/ including subdirs
    -   base
        -   Allow sysconf(\_SC\_NPROCESSORS\_CONF)
        -   Allow additional journald sockets
    -   fix for non-latin file/directory names
    -   fonts: allow reading ~/.local/share/fonts
    -   freedesktop.org - support /usr/local/applications; support subdirs of applications folder
    -   glibc uses /proc/\*/auxv and /proc/\*/status files
    -   gnome: allow reading GLib schemas.
    -   java: update for Java 8 and 9
    -   nvidia - Update nvidia for newer nvidia drivers
    -   perl-base - adjust the multiarch alternation rule in the perl abstraction for modern Debian and Ubuntu systems
    -   python - Adjust for python3.6
    -   Rename global variable “pid” to “log\_pid”
    -   Enable camera access in browser apparmor profile for WebRTC
    -   fix permissions matching for seven digit pids
    -   nameservice
        -   allow access to /etc/netconfig
    -   ubuntu-browsers - support Debian's Firefox non-ESR path.
    -   ubuntu-browsers, ubuntu-helpers: add support for Google Chrome beta <https://bugs.debian.org/880923>, <https://bugs.launchpad.net/apparmor/+bug/1730536>
    -   ubuntu-email: update for new thunderbird path
    -   support both the the old-style /usr/lib/firefox/firefox.sh wrapper and the current /usr/lib/firefox{,-esr}/firefox{,-esr} paths.
    -   wayland - allow wayland-cursor-shared-\* <https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=870807>
    -   X - allow new sddm location for .Xauthority
-   dovecot
    - dovecot main binary:
        -   add the attach\_disconnected flag
        -   change Px to mrPx for /usr/lib/dovecot/\*
        -   allow sending signals to reload/restart child daemons
        -   allow capability dac\_read\_search <https://bugzilla.opensuse.org/show_bug.cgi?id=1069470#c9>
    - auth: allow capability dac\override and dac\_read\_search <https://bugzilla.opensuse.org/show_bug.cgi?id=1069470>
    -   imap-login: Allow /var/run/dovecot/login-master-notify\*
    -   dovecot-lda update
        -   the attach\_disconnected flags
        -   read access to /usr/share/dovecot/protocols.d/
        -   rw for /run/dovecot/auth-userdb
    -   dict: add abstractions/openssl (needed with openssl 1.1)
    -   imap: allow writing tempfiles
    -   imap-login: allow /var/run/dovecot/login-master-notify\*
    -   managesieve-login: grant access to the login-master-notify socket
    -   pop3-login: grant access to the anvil socket
-   update netstat profile
    -   allow reading @{PROC}/@{pid}/net/netstat and @{PROC}/@{pid}/net/snmp
    -   drop owner conditional - /proc/\*/net/\* is always owned by root, and the owner conditional means breaking netstat for non-root users
    -   drop “@{PROC}/@{pids}/fd r,” - /proc/\*/fd is a directory, so this rule would never apply
	-   allow capability sys\_ptrace and ptrace (read)
-   Postfix
    -   change abstractions/postfix-common to allow /etc/postfix/\*.db k
    -   add several permissions to postfix/error, postfix/lmtp and postfix/pipe
    -   remove superfluous abstractions/kerberosclient from all postfix profiles - it's included via abstractions/nameservice
-   Samba profile updates for ActiveDirectory / Kerberos
-   sshd - drop local/ include
-   syslog-ng - allow reading /proc/kmsg <https://bugs.launchpad.net/bugs/1739909>
-   traceroute - support TCP SYN for probes, quite net\_admin request
-   /etc/cron.daily/logrotate update
-   wireshark profile update
-   useradd: allow audit\_write and running pam\_telly2

Documentation
-------------

-   Add network 'smc' keyword to apparmor.d manpage
-   Add --no-reload to various utils manpages
-   aa-status - update manpage for updated podchecker
-   aa-enabled - update manpage
-   improve build instructions in README

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

