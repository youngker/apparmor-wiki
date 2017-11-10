AppArmor 2.9.1 Release Notes
============================

[AppArmor 2.9.1](https://launchpad.net/apparmor/+milestone/2.9.1)
is an incremental bug fix release over AppArmor 2.9.0 that is focused
on fixing issues in the userspace code.

Improvements and Bugs Fixed
---------------------------

### libapparmor

-   fix log parsing for 3.16 kernels + syslog-ng, that was preventing utils from working (, )
-   allow skipping build of man pages via configure option

### Policy Parser

-   parsing of mount option fixups:
    -   fix incorrect mount options ()
    -   fail compilation if unknown mount options are found ()
    -   don't treat recursive mount options as normal options
    -   fix error typo
    -   add language parsing test cases
-   clean up some minor file descriptor handling issues

### Utils

Numerous improvements and bug fixes were made to the python tools,
including:

-   proposing abstractions for missing network rules ()
-   don't ask for existing existing network rules ()
-   performance improvements when parsing log files
-   other miscellaneous bug fixes

### Policy

Updates to the following profiles

-   dnsmasq
-   nscd
-   useradd
-   sendmail
-   man
-   passwd

### Documentation

-   document ability to load profiles from a directory
-   sync documentation on mount rules with parser's implementation

### Translations

-   updated German, Italian translations

Complete Changelog
------------------

AppArmor 2.9.1 includes revisions 2760 through 2831 from the [2.9
branch](http://bazaar.launchpad.net/~apparmor-dev/apparmor/2.9/changes/2831?start_revid=2760).
