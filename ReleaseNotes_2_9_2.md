AppArmor 2.9.2 Release Notes
============================

[AppArmor 2.9.2](https://launchpad.net/apparmor/+milestone/2.9.2)
is an incremental bug fix release over AppArmor 2.9.1 that is focused
on fixing issues in the userspace code.

Please note: this is the first AppArmor
release signed with the [new AppArmor signing
key](https://lists.ubuntu.com/archives/apparmor/2015-April/007757.html)
(fingerprint 3ECD CBA5 FB34 D254 961C C53F 6689 E64E 3D36 64BB)

Improvements and Bugs Fixed
---------------------------

### Policy Compiler (a.k.a. apparmor\_parser)

-   Fix incorrect compilation of audit modifiers for exec and pivot\_root (, )
-   Fix compilation failure of deny link rules ()
-   Fix organization of network families data structure (thanks to Philip Withnall and Simon McVittie)
-   Correct handling of error result from readdir\_r(3)
-   Fix compilation failures when building with gcc 5.
-   Ensure debugging and error reporting goes to stderr
-   Added test cases and improvements to test infrastructure.

### Utils

-   Support additional syslog format ()
-   Fix minitools to work with multiple profiles at once ()
-   aa-unconfined: work with profile names that don't start with /
-   aa-status: don't crash when mountpoints contain UTF-8 chars (, thanks to Alain BENEDETTI)
-   aa-easyprof: add --include-templates-dir and --include-policy-groups-dir options
-   aa-complain: don't require strict names for profiles ()
-   Ignore disconnected path events rather than crashing ()
-   Clean up profile preamble and flag handling and add support for profile attachment
-   Clean up network rule writing
-   Fix doubled '-&gt;' when writing out exec rules ()
-   Fix crashes when reading 'send' and 'trace' log events (, )
-   Fix problems handling additional variable assignments
-   Fix crash when path rules are separated by non-path rules.
-   Sync the utils and the parsers notion of which files and directories to ignore
-   Other minor improvements
-   Numerous added test cases and improvements to test infrastructure

### Policy

Updates to the following profiles:

-   mysqld
-   dovecot
-   dnsmasq (thanks to Cédric Bosdonnat and Cameron Norman)

Updates to the following abstractions:

-   base - Allow writes to the systemd journal socket ()
-   aspell - allow access to /usr/share/aspell/ (thanks to Felix Geyer)
-   ssl\_certs - Add support for /etc/pki (thanks to Gregor Dschung)
-   ubuntu\_email - Add geary email client (thanks to Cameron Normon)
-   ubuntu-helpers - allow generation of texlive fonts ()
-   X - add new gdm path ()
-   mir - new abstraction ()

### Documentation

-   Fix network rule description and remove obsolete references to
    program-chunks in apparmor.d(5)

Complete Changelog
------------------

AppArmor 2.9.2 includes revisions 2911 through 2832 from the [2.9
branch](http://bazaar.launchpad.net/~apparmor-dev/apparmor/2.9/changes/2911?start_revid=2832).
