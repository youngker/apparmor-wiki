New features
============

-   Basic mount rules are now supported (requires mount kernel patch)
-   New introspection interface - the features file has been converted to a directory if new kernel patches or linux 3.4 is used
-   new aa-exec utility to launch programs under a specific profile
-   new aa-easyprof templated profile generation tool

Language changes/improvements
=============================

-   Files
    -   addition of optional 'file' keyword, that can be used with file rules
        file /example/rule r,
    -   bare file keyword can be used to represent all file perms
        file, \#Grant access to all file perms
-   Capabilities
    -   capability rules can now list multiple capabilities
        capability dac\_override sys\_admin,
    -   bare capability keyword now allows representing all capability permissions
        capability, \# Grant all capabilities
-   Mount
    -   see mount rules in the core policy reference manual

Improvements
============

-   improvements to state minimization has been made in the compiler
-   improvements in the compilers run time memory foot print, especially for large policy
-   improvements in build autogeneration of network, rlimit, and capability info
-   change\_profile rules grant access to the necessary kernel interface
-   New regression tests and test suite improvements
-   restructured apparmor.vim creation script
-   use of autodep when creating child profiles
-   improvments to compilation test, verification and dumping

Bug fixes
=========

-   Fix infinite loop bug in compiler normialization
-   Fix conversion bug for pcre hex digits
-   several fixes around genprof/logprof missing log entries
-   Fix several genprof/logprof handling x rules
-   Fix autodep profile creation
-   Fix aa\_getprocattr error paths
-   Fix handling of ip addresses and ports in logparsing
-   added missing capabilities to the severity.db
-   Change\_profile now works correctly from confined processes
-   change\_onexec has been fixed for profiles that don't have an attachment specification
-   and numerous other bug fixes

Reference Profile updates
=========================

-   dnsmasq
-   usr.lib.dovecot.imap-login allow inet6
-   webbrowser profile updates
-   new abstractions:
    -   ubuntu-helpers
    -   xdg-desktop
-   X abstractions
-   KDE abstractions
-   and many more

