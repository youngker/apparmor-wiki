AppArmor 2.10.1 Release Notes
=============================

[AppArmor 2.10.1](https://launchpad.net/apparmor/+milestone/2.10.1)
is an incremental bug fix release over AppArmor 2.10 that is focused
on fixing issues in the userspace code.

Improvements and Bugs Fixed
---------------------------

### Policy Compiler (a.ka. apaprmor\_parser)

-   Caching
    -   Fix caching timestamp mtime issues ()
    -   bump abi to force policy recompiles for bad caches
    -   Fix uninitialized variable and reference leak

<!-- -->

-   allow “unspec” (AF\_UNSPEC) family in network rules ()
-   properly parse named transition targets ()
-   allow profile keyword to be used with namespaces ()
-   Fix segfault when processing profile directories ()
-   Fix regression: Honor the --namespace-string commandline option ()
-   Fix remount with bind ()
-   Fix a missing comma in when dumping capability names for debugging
-   Fix incorrect output of child profile names (-N) which could cause policy reload to remove children profiles that should not have been. ()

### Library

-   Fix logparser.py crash on change\_hat events ()
-   fix log parsing memory leaks

### Utils

-   aa-status
    -   make aa-status work without python3-apparmor ()
-   aa-easyprof
    -   Use apparmor.fail for AppArmorException handling in aa-easyprof
-   aa-logprof, aa-genprof, aa-mergeprof
    -   Fix wrong usage of write\_prof\_data in serialize\_profile\_from\_old\_profile() ()
    -   Fix aa-mergeprof crash with files containing multiple profiles
    -   Add simple\_tests/profile/profile\_ns\_bad8.sd to utils test exception list
    -   Remove pname to bin\_name mapping in autodep()
    -   logparser.py: do sanity check for all file events ()
    -   handle versioned ruby interpreters
    -   print test filenames in 'make check' and 'make coverage'
    -   Better error message on unknown profile lines
    -   AARE: escape reserved exclamation mark symbol
    -   More useful logparser failure reports
    -   Fix handling of link events in aa-logprof
    -   Write unix rules when saving a profile (, )
    -   Adjust test-aa.py for python2
    -   Adjust type(x) == str checks in the rule classes for py2 ()
    -   Let the apparmor.fail error handler print to stderr ()
    -   ignore log event if request\_mask == '' ()
    -   Fix logparser.py crash on change\_hat events ()
    -   Several fixes for variable handling
    -   Change abstract methods in BaseRule to use NotImplementedError
    -   Map c (create) log events to w instead of a
    -   Also add python 3.5 to logprof.conf
    -   Add debug info to profile\_storage()
    -   Fix parsing/storing bare file rules
    -   update PYMODULES in tools/Makefile
    -   Add python to the “no Px rule” list in logprof.conf
    -   let logparser.py ignore file\_inherit events without request\_mask ()
    -   Let 'make check' work without logprof.conf ()
    -   Fix handling of interpreters with parameters ()
    -   merge script handling into get\_interpreter\_and\_abstraction()
    -   Add tests for create\_new\_profile()
    -   Change utils/test/Makefile to use the in-tree libapparmor
    -   Parse all parser simple\_tests with the utils code
    -   Get rid of global variable 'logger'
    -   make 'ldd' variable non-global
    -   Fix missing profile init in create\_new\_profile()
    -   Store filename for includes and hats
    -   Add AARE tests for \[chars\] and \[^chars\] style globbing to test-aare.py.
    -   load\_include(): use include\_dir\_filelist()
    -   remove unused code from load\_include()
    -   load\_include(): avoid loading directory includes multiple times
    -   Reset aa and original\_aa in read\_profiles()
    -   move tests for convert\_regexp() to (new) test-aare.py
    -   Accept more log formats in logparser.py
    -   Test libapparmor test\_multi tests against logparser.py
    -   utils/aa-logprof.pod: fix typo in manpage ()
    -   Add network mpls and ib to rule/network.py and the apparmor.d manpage
    -   map socket\_create events to 'net' events
    -   Check for duplicate profiles
    -   Fix name\_to\_prof\_filename() error behaviour
    -   Change RE\_PROFILE\_START to accept variables
    -   Split logparser.py add\_event\_to\_tree() into multiple functions
    -   drop shebang from apparmor/rule/\*.py

### Policy

-   Change /bin/ paths in profiles to also match on /usr/bin/

Updates to the following profiles:

-   sbin.dhclient: allow executing nm-dhcp-helper and access to some files in /var/lib/dhcp6/ and /var/lib/NetworkManager/
-   sbin.syslog-ng: add several permissions (abstractions/openssl, reading the journal etc.) which are needed by the latest syslog-ng (, )
-   usr.bin.skype: allow reading @{PROC}/@{pid}/net/dev ()
-   usr.lib.dovecot.auth: allow writing to /var/run/dovecot/user-stats (needed by dovecot &gt;= 2.2.22)
-   usr.lib.dovecot.lmtp: add openssl and ssl\_keys abstractions
-   usr.lib.dovecot.imap: allow reading /run/dovecot/mounts
-   usr.lib.dovecot.dovecot-lda:
    -   allow to write tempfiles ()
    -   allow to execute sendmail ()
-   usr.sbin.avahi-daemon: allow write access to /run/systemd/notify (needed on systems with systemd)
-   usr.sbin.dnsmasq:
    -   allow /bin/sh and /bin/dash in addition to /bin/bash (, non-public)
    -   allow /dev/tty rw which is needed by the --dhcp-script's shell (, non-public)
    -   add attach\_disconnected flag ()
-   usr.sbin.nscd: allow reading /proc/self/cmdline, needed for paranoia mode ()
-   usr.sbin.ntpd:
    -   add attach\_disconnected flag (needed for using nscd)
    -   allow reading the directory listing of $PATH ()
-   usr.sbin.smbd: allow capability sys\_admin which is needed because smbd stores ACLs in the security.NTACL namespace (, [Discussion on the Samba mailinglist](http://samba-technical.samba.narkive.com/eHtOW8DE/nt-acls-using-the-security-namespace-for-ntacl-considered-improper))
-   usr.sbin.winbindd:
    -   update for Samba 4.2 (, )
    -   allow k for /etc/samba/smbd.tmp/msg/\* ()

Updates to the following abstractions:

-   base: allow reading /usr/share/locale-bundle/ (contains translations in openSUSE)
-   nameservice: allow reading /run/systemd/resolve/resolv.conf ()
-   python: update for python3
-   samba: update for Samba 4.2 ()
-   ssl\_certs, ssl\_keys: allow reading acmetool-generated certificates in /var/lib/acme/
-   X: allow unix connections to @/tmp/.ICE-unix/\[0-9\]\*, needed by (at least) firefox and thunderbird
-   allow dconf abstraction to read /etc/dconf/\*\*

### Documentation

-   Correct meaning of EPERM in aa\_change\_profile man page
-   document open fds may be revalidated after aa\_change\_profile()
-   document exec deny rules don't allow tranisition quanifier ix, Px, Ux etc. - only 'deny /foo x,' is allowed.
-   Add realtime signals to SIGNALS list in apparmor.d manpage
-   Add realtime signal example to the apparmor.d manpage
-   Add missing variables @{pids} and @{apparmorfs} to the apparmor.d manpage
-   fix typo “sinlge” in apparmor\_parser manpage ()
-   Remove incorrect statement in aa\_change\_profile man page

### Init Scripts

-   Fix aa\_log\_end\_msg() in rc.apparmor.suse ()

### Regression and Unit Tests

-   fix ptrace tests for arm64 and s390 (, )
-   Add parser tests for various rules outside of a profile body
-   make caching tests not fail w/python &lt;= 3.2
-   Don't skip parser unit test cleanup when the test was skipped
-   Run caching tests even when apparmorfs is not mounted
-   Verify cache file mtime in caching tests
-   make sysctl(2) regression test a bit more resiliant
-   fix memory leaks in libapparmor's aalogmisc unit tests.
-   Add a new test that was posted on IRC to the test\_multi set

Complete Changelog
------------------

AppArmor 2.10.1 includes revisions 3326 through 3205 from the [2.10
branch](http://bazaar.launchpad.net/~apparmor-dev/apparmor/2.10/changes/3325?start_revid=3205).
