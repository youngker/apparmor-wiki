AppArmor 2.9.3 Release Notes
============================

[AppArmor 2.9.3](https://launchpad.net/apparmor/+milestone/2.9.3)
is an incremental bug fix release over AppArmor 2.9.2 that is focused
on fixing issues in the userspace code.

Improvements and Bugs Fixed
---------------------------

### Policy Compiler (a.k.a. apparmor\_parser)

-   set cache file timestamp to the mtime of most recent policy file tstamp ()
-   respect $CPPFLAGS
-   fix ptrace tests for arm64 and s390 (, )
-   allow “unspec” (AF\_UNSPEC) family in network rules ()

### pam\_apparmor

-   respect $CPPFLAGS

### Utils

-   aa-logprof, aa-genprof, aa-mergeprof
    -   ignore (no longer supported) hat declarations (“^foo,”)
    -   don't forget to write unix rules when saving a profile (, )
    -   fix handling link events
    -   fix crash with \#include <directory> when hitting a capability or network log event ()
    -   ignore log events that look like file events (based on operation=), but aren't (, , , )
    -   fix crash on change\_hat log events ()
    -   accept more log formats
    -   fix crash with hats in included files
    -   make sure pux, cux and CUx modes are accepted as valid
    -   fix handling capability rules in aa-mergeprof
    -   add python to the “no Px rule” list in logprof.conf
    -   map 'c' (create) log events to 'w' instead of 'a' - if a program opens a file without O\_APPEND, 'a' permissions aren't enough
    -   several fixes on parsing and writing variable definitions
    -   fix aa-mergeprof crash on files containing more than one profile
    -   several small bugfixes

-   aa-complain, aa-enforce, aa-audit, aa-disable
    -   set the flags for all hats, not only in the main profile
    -   add a --no-reload parameter (
    -   work for non-existing binaries ()
    -   let aa-audit print a warning on disabled profiles ()
    -   let aa-complain delete the disable symlink

-   aa-status
    -   make check if a file is readable more robust ()

-   aa-notify:
    -   also display notification for complain mode events

-   tests
    -   use in-tree libapparmor by default and add support for USE\_SYSTEM variable
    -   change minitools\_test.py to use --no-reload so that they can run as non-root

### Policy

Updates to the following profiles:

-   change /bin/ paths in all profiles to also match /usr/bin/
-   sbin.dhclient: allow executing nm-dhcp-helper and access to some files in /var/lib/dhcp6/ and /var/lib/NetworkManager/
-   sbin.syslog-ng: add several permissions (abstractions/openssl, reading the journal etc.) which are needed by the latest syslog-ng.
-   usr.bin.skype: allow reading @{PROC}/@{pid}/net/dev ()
-   usr.lib.dovecot.auth: allow writing to /var/run/dovecot/user-stats (needed by dovecot &gt;= 2.2.22)
-   usr.lib.dovecot.lmtp: add openssl and ssl\_keys abstractions
-   usr.lib.dovecot.imap: allow reading /run/dovecot/mounts
-   usr.lib.dovecot.dovecot-lda:
    -   allow to write tempfiles ()
    -   allow to execute sendmail ()
-   usr.lib.postfix.master: add 'k' permissions for master.pid
-   usr.sbin.avahi-daemon: allow write access to /run/systemd/notify (needed on systems with systemd)
-   usr.sbin.dnsmasq:
    -   allow /bin/sh and /bin/dash in addition to /bin/bash (, non-public)
    -   allow /dev/tty rw which is needed by the --dhcp-script's shell (, non-public)
    -   add attach\_disconnected flag ()
-   usr.sbin.nscd: allow reading /proc/self/cmdline, needed for paranoia mode ()
-   usr.sbin.ntpd:
    -   add attach\_disconnected flag (needed for using nscd)
    -   allow reading the directory listing of $PATH ()
-   usr.sbin.smbd: allow capability sys\_admin which is needed because smbd stores ACLs in the security.NTACL namespace ([Discussion on the Samba mailinglist](http://samba-technical.samba.narkive.com/eHtOW8DE/nt-acls-using-the-security-namespace-for-ntacl-considered-improper))
-   usr.sbin.winbindd:
    -   update for Samba 4.2 (, )
    -   allow k for /etc/samba/smbd.tmp/msg/\* ()

Updates to the following abstractions:

-   base: allow reading /usr/share/locale-bundle/ (contains translations in openSUSE)
-   nameservice: allow reading /run/systemd/resolve/resolv.conf
-   python: update for python3
-   samba: update for Samba 4.2 (, )
-   ssl\_certs, ssl\_keys: allow reading acmetool-generated certificates in /var/lib/acme/
-   X: allow unix connections to @/tmp/.ICE-unix/\[0-9\]\*, needed by (at least) firefox and thunderbird

### Documentation

-   apparmor.d manpage:
    -   document @{pids} and @{apparmorfs}
    -   document realtime signals
-   fix incorrect statement in aa\_change\_profile manpage
-   aa\_change\_profile manpage:
    -   fix description of EPERM
    -   clarify handling of open file descriptors

### initscripts

-   fix error handling in rc.apparmor.suse for “rcapparmor kill” (non-public bug report)

Complete Changelog
------------------

AppArmor 2.9.3 includes revisions 3005 through 2911 from the [2.9
branch](http://bazaar.launchpad.net/~apparmor-dev/apparmor/2.9/changes/3004?start_revid=2911).
