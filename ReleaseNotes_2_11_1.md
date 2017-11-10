Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------

-   Fix af\_unix downgrade of network rules
-   Fix delete after new\[\]
-   Set parser executable path according to USE\_SYSTEM make variable

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

-   aa-notify - update to use normal urgency notifications to obtain intended behavior across DEs
-   Add network 'smc' keyword in NetworkRule
-   Prevent 'wa' conflicts for file rules
-   Carry over all autodep-generated rules in handle\_children()
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
-   YaST
    -   Fix save\_profiles() for YaST <https://bugzilla.opensuse.org/show_bug.cgi?id=1062667>
-   Add aa-remove-unknown utility to unload unknown profiles

Policy
------

-   Abstractions
    -   freedesktop.org - support /usr/local/applications; support subdirs of applications folder
    -   fix for non-latin file/directory names
    -   gnome - allow reading GLib schemas.
    -   wayland - allow wayland-cursor-shared-\*
    -   python - Adjust for python3.6
    -   perl-base - adjust the multiarch alternation rule in the perl abstraction for modern Debian and Ubuntu systems
    -   base - Allow sysconf(\_SC\_NPROCESSORS\_CONF)
    -   nvidia - Update nvidia for newer nvidia drivers
    -   Rename global variable “pid” to “log\_pid”
    -   glibc uses /proc/\*/auxv and /proc/\*/status files
    -   Apache2 - profile updates for proper signal handling, optional saslauth, and OCSP stapling

-   sshd - drop local/ include
-   /etc/cron.daily/logrotate update

-   dovecot
    -   Allow /var/run/dovecot/login-master-notify\* in dovecot imap-login profiles
    -   add the attach\_disconnected flag
    -   change Px to mrPx for /usr/lib/dovecot/\*
    -   dovecot-lda update
        -   the attach\_disconnected flags
        -   read access to /usr/share/dovecot/protocols.d/
        -   rw for /run/dovecot/auth-userdb

-   Postfix
    -   change abstractions/postfix-common to allow /etc/postfix/\*.db k
    -   add several permissions to postfix/error, postfix/lmtp and postfix/pipe
    -   remove superfluous abstractions/kerberosclient from all postfix profiles - it's included via abstractions/nameservice

-   Samba profile updates for ActiveDirectory / Kerberos
-   traceroute - support TCP SYN for probes, quite net\_admin request

Documentation
-------------

-   Add network 'smc' keyword to apparmor.d manpage
-   aa-status - update manpage for updated podchecker

Tests
-----

-   libapparmor: fix ptrace regression test failure
-   Add --no-reload to various utils manpages
-   Ignore test failures about duplicated conditionals in dbus rules
-   readdir - test both getdents() and getdents64() if available
-   where necessary use getdents64 to fix arm64 build failure
-   No longer skip testing generated\_perms\_leading profiles
-   regression tests
    -   fix environ fail case

