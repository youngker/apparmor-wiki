AppArmor 2.5.2 Release Notes
============================

The 2.5.2 release of the AppArmor user space tools fixes
bugs and includes minor enhancements over the [AppArmor 2.5.1
release](Release_Notes_2.5.1).

-   For information on other versions please see [AppArmor versions](AppArmor_versions)

Bug Fixes and Enhancements
--------------------------

### AppArmor Profiles

-   update Ubuntu email abstraction for ever-changing path of Thunderbird
-   fix proc usage in firefox profile (bnc \#436262)
-   fix incorrect /proc/\*/sys usage in usr.sbin.ntpd (bnc \#634801)
-   updated dnsmasq profile ([LP: \#697239](https://launchpad.net/bugs/697239))
-   updated freedesktop.org abstraction
-   updated X abstraction for dri modules ([LP: \#658135](https://launchpad.net/bugs/658135))
-   other various abstraction updates

### AppArmor Parser

-   add workaround for kernel bug in handling AF\_MAX network entries ([LP: \#727478](https://bugs.launchpad.net/bugs/727478))
-   skip build time cache tests when in environments where securityfs is not available
-   break build targets apart to support distributors who don't wish to build/include everything.
-   fix a couple of situations where invalid characters were being passed through to stdout
-   fix use after free in regexp parser
-   fix two x (execute) transition conflict bugs ([LP: \#693082](https://bugs.launchpad.net/apparmor/+bug/693082)) and add testcases

### AppArmor Utils (aa-genprof, aa-logprof)

-   fix to set complain mode on subprofiles ([LP: \#707092](https://bugs.launchpad.net/apparmor/+bug/707092))
-   fix handling of log entries containing a null path (bnc \#407959)
-   fix handling of audit events for unconfined processes
-   cleanup process state correct after abort in genprof (bnc \#586072)
-   fix aa-logprof's behavior when writing out variable declarations to not emit incorrect syntax
-   fix network event handling due to change in log format (bnc \#665483)
-   abstract out the perl vendor location for distros to override if necessary at installation time
-   adjust strings for easier translations (bnc \#586072)
-   updates to manpages
-   other minor bugfixes

### AppArmor Library (libapparmor)

-   make change\_hatv(), change\_hat\_varargs() available via swig interfaces
-   fix python swig bindings to be functional

### Regression Test Suite

-   fix auto-generation of profiles on x86\_64 systems.
-   fix “deleted” test case to match the documentation for the expected outcome and add additional positive test.
-   fix xattr tests to use its own loopback mounted fs to ensure that user\_xattrs are supported.
-   fix minimalist tcp test and re-enable
-   fix several testsuite warnings and typos.

Features removed
----------------

Some deprecated and obsolete features were removed with this release:

-   utils/severity.pl - due to incorrect license/copyright statement
