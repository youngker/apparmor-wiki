AppArmor 2.9.5 Release Notes
----------------------------

AppArmor 2.9.5 is an incremental bug fix release over AppArmor 2.9.4
that is focused on fixing issues in the userspace code.

It includes the changes in the [2.9
branch](http://bazaar.launchpad.net/~apparmor-dev/apparmor/2.9/changes/3045?start_revid=3068)
between r3045 (AppArmor 2.9.4) and r3068.

Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------

-   Fix af\_unix downgrade of network rules
-   parser Fix delete after new\[\]

Init
----

-   Preserve unknown profiles when restarting apparmor init/job/unit. CVE-2017-6507

Utils
-----

-   aa-logprof - Ignore change\_hat events with error=-1 and “unconfined can not change\_hat”
-   aa-unconfined - fix netstat invocation regression
-   Add aa-remove-unknown utility to unload unknown profiles
-   Remove re.LOCALE flag

Policy
------

-   abstractions
    -   base - update for glibc use of /proc/\*/auxv and /proc/\*/status
    -   apache2 - updates for proper signal handling, optional saslauth, and OCSP stapling
    -   freedesktop.org: support /usr/local/applications; support subdirs of applications folder
    -   Adjust python abstraction for python3.6
-   dovecot
    -   Allow /var/run/dovecot/login-master-notify\* in dovecot imap-login profiles
    -   add the attach\_disconnected flag
    -   change Px to mrPx for /usr/lib/dovecot/\*
    -   Add several permissions to the dovecot profiles that are needed on ubuntu
    -   dovecot-lda needs
-   traceroute updates [OpenSUSE \#1057900](https://bugzilla.opensuse.org/show_bug.cgi?id=1057900)
-   Samba profile updates for ActiveDirectory / Kerberos
-   Postfix
    -   change abstractions/postfix-common to allow /etc/postfix/\*.db k
    -   add several permissions to postfix/error, postfix/lmtp and postfix/pipe
    -   remove superfluous abstractions/kerberosclient from all postfix

Documentation
-------------

-   aa-status: update man page for updated podchecker
-   utils: Add --no-reload option to manpage

Tests
-----

-   libapparmor/tests
    -   remove test\_multi unconfined-change\_hat.profile
-   regression/tests
    -   fix environ fail case

