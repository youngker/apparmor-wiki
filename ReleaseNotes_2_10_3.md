AppArmor 2.10.3 Release Notes
-----------------------------

AppArmor 2.10.3 is an incremental bug fix release over AppArmor 2.10.2
that is focused on fixing issues in the userspace code.

This release includes the 2.10 branch changes between r3379 (= 2.10.2)
and r3407.

Policy Compiler (a.k.a. apparmor\_parser)
-----------------------------------------

-   Fix af\_unix downgrade of network rules
-   Fix delete after new\[\]

Init
----

-   Preserve unknown profiles when restarting apparmor init/job/unit CVE-2017-6507

Library
-------

-   libapparmor: fix swig test\_apparmor.py for zero length ptrace records

Utils
-----

-   aa-unconfined - fix netstat invocation regression
-   aa-logprof - Ignore change\_hat events with error=-1 and “unconfined can not change\_hat”
-   Add aa-remove-unknown utility to unload unknown profiles
-   Remove re.LOCALE flag

Policy
------

-   Abstractions
    -   freedesktop.org - support /usr/local/applications; support subdirs of applications folder
    -   python - update for python3.6
    -   perl - adjust the multiarch alternation rule for modern Debian and Ubuntu systems
    -   base - glibc uses /proc/\*/auxv and /proc/\*/status files, too
    -   apache2 - updates for proper signal handling, optional saslauth, and OCSP stapling
-   dovecot
    -   Allow /var/run/dovecot/login-master-notify\* in dovecot imap-login profiles
    -   add the attach\_disconnected flag
    -   change Px to mrPx for /usr/lib/dovecot/\*
    -   dovecot-lda needs
    -   Add several permissions to the dovecot profiles that are needed on ubuntu
        -   the attach\_disconnected flags
        -   read access to /usr/share/dovecot/protocols.d/
        -   rw for /run/dovecot/auth-userdb
-   traceroute - support TCP SYN for probes, quite net\_admin request
-   Samba - updates for ActiveDirectory / Kerberos
-   postfix
    -   change abstractions/postfix-common to allow /etc/postfix/\*.db k
    -   add several permissions to postfix/error, postfix/lmtp and postfix/pipe
    -   remove superfluous abstractions/kerberosclient from all postfix profiles - it's included via abstractions/nameservice

Documentation
-------------

-   apparmor.d manpage - Add network 'smc' keyword in NetworkRule
-   aa-status manpage updated for updated podchecker
-   Add --no-reload to various utils manpages

Tests
-----

-   libapparmor - remove test\_multi unconfined-change\_hat.profile
-   regression tests: fix environ fail case

