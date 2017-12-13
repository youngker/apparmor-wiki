Introduction
============

AppArmor 2.10 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project is
maintained and pushed separately. This version of the userspace should
work with all kernel versions from 2.6.15 and later (some earlier
version of the kernel if they have the apparmor patches applied). This
version of the userspace also supports the apparmor 3, development
patches that are currently shipped as part of the Ubuntu kernel.

Highlighted New features
========================

-   libapparmor has gained new apis for managing and work with the
    kernel and caches, enabling external projects like **systemd** to
    be able interact with and manage apparmor cache and policy loads.

Highlighted Improvements
========================

-   policy compile times have been improved by up to 40%

-   variables can now be used in profile names

-   the apparmor tools/utils have stabilized and should reliably work

-   the apparmor tools/utils have added support for several rules,
    flag types and profile layouts that where previously unsupported

-   the documentation has been updated and improved to address many
    mis/un-documented features

Highlighted Bug Fixes
=====================

-   for a more comprehensive list see the change log or summary

-   The apparmor tools/utils have seen numerous bug fixes finally
    bringing them to a stable state

-   A cache time stamp issue was fixed

-   several memory leak/crash bugs were fixed in libapparmor

Profile updates
===============

-   abstractions
    -   aspell
    -   base
    -   mir
    -   postfix-common
    -   samba
    -   ssl\_certs
    -   ubuntu-email
    -   ubuntu-helpers
    -   X
-   Profiles
    -   sbin.klogd
    -   sbin.syslogd
    -   sbin.syslog-ng
    -   usr.lib.dovecot.auth
    -   usr.lib.dovecot.imap
    -   usr.lib.dovecot.imap-login
    -   usr.lib.dovecot.lmtp
    -   usr.sbin.dnsmasq
    -   usr.sbin.dovecot
    -   usr.sbin.ntpd
    -   usr.sbin.smbd
    -   usr.sbin.winbindd
-   Example/Reference profiles
    -   usr.lib.postfix.master
    -   usr.sbin.mysqld

Change log summary ([r2832-3205](http://bazaar.launchpad.net/~apparmor-dev/apparmor/master/changes/3205?start_revid=2832))
==========================================================================================================================

tools

-   fix crash when non-ASCII mount points are used
-   fix aa-audit, aa-complain, aa-enforce, aa-autodep to work with multiple profiles at once
-   fix only require log file for log\_prof and gen\_prof
-   fix aa-unconfined to work with profile names that don't begin '/' character
-   fix honor chmod logs
-   fix network rule handling/writing
-   fix support -&gt; in exec rules
-   fix handling of overlapping file and network logs
-   fix handling of variables
-   fix crash in profile serialization
-   fix aa\_complain for non-existin binaries and symlinks
-   fix ignore change\_hat declarations
-   fix don't use access to determine readablity of file due to race condition
-   fix parsing of bool assignments
-   fix ignore log entries without request mask
-   fix aa-complain to change the flags of all hats
-   major under the hood improvements, complete refactoring of handling capability, change\_profile, network and rlimit rules - which also brings lots of bugfixes
-   add support for pux, cux, CUx exec mode
-   add support for profile attachment specifications
-   add in profile de-duplication
-   add support for change\_profile rules
-   add --no-reload option
-   migrate to using private library fn \_aa\_is\_blacklist()
-   improve handling of disconnected path log parsing
-   improve severity coverage and handling
-   improve capability handling
-   improve error handling
-   unit tests and coverage improvements
-   testing against simple\_tests sample profiles
-   General code cleanup and refactoring
-   Makefile improvements and cleanup

apparmor\_parser

-   fix expansion of @{profile\_name} that contains regexs
-   fix bare include keyword
-   fix missing dependency build failure
-   fix variable expansion in link rule target
-   fix cache file time stamp issue
-   fix error checking in several places
-   fix several issues areound build flags
-   fix debug output
-   fix issues around address family value lookup
-   fix gcc-5 build issues
-   migrate cache, features, kernel interface to use new library apis
-   improve black\_listed file check
-   add support for variables in profile names and attachments
-   add support for onexec transitions in change\_profile rules
-   improve performance of policy compile
-   general refactoring and code imrpovements
-   Makefile cleanup
-   tests
    -   expand and improve test suite for new features and existing features
    -   add generated profile tests verifying permission equality

libapparmor

-   new apis for managing
    -   policy cache
    -   kernel policy interface
    -   kernel supported features
    -   query\_label helper functions for file path and link permissions
    -   aa\_split\_con for label parsing
-   fix memory leaks
-   fix several build warnings
-   fix FTBS when secure\_getenv is not available
-   fix parsing of some syslog variants
-   add support for parsing of direct dmesg logs

Man page documentation

-   library fns
    -   new man page for new aa\_policy\_cache
    -   new man page aa\_kernel\_interface
    -   new man page for aa\_query\_label
    -   general cleanup and improvements of documentation
-   parser
    -   add documenation of kernel features-file
-   apparmor.d
    -   fix network rules documentation
    -   add documentation of change\_profile rules
    -   add documentation of rules with leading permissions
    -   add documentation of link rules
    -   add documentation of alias rules
    -   update capability rules documentation
    -   update file rule patterns
    -   update exec transition documentation
    -   update rule qualifier information
    -   update rlimit rules documentation
    -   general cleanup and refactoring

Translations

-   Merge in the latest translation updates

Regression Tests

-   Fix define collisions
-   Fix query label regression
-   Fix ptrace test build failures on newer glibcs
-   Fix/improve query\_label tests
-   Fix pivot root mouting issues
-   Fix gcc-5 inline issues
-   verify aa\_getpeercon values
-   add kernel\_interface tests
-   add policy\_cache tests
-   improve kernel feature support detection and support of conditionally running tests based on supported feature sets

