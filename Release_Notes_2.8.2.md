2.8.2 includes revisions 2070 through 2087 from the [2.8
branch](http://bazaar.launchpad.net/~apparmor-dev/apparmor/2.8/changes/2087?start_revid=2087).

Bug fixes
=========

-   Kshitij Gupta fixed a display bug in aa-logprof, aa-genprof,
    with the Glob and Glob with Ext putting duplicate entries in the
    list. The fix introduced a Perl 5.10.1 or higher dependency.
-   Gernot Vormayr fixed a potential NULL-write in aa\_getprocattr() error path
-   Michael Palimaka fixed hu translations
-   Fix for cache failures when the feature file is larger than internal buffer
-   Fix apparmor\_parser cache tempfile location to use passed arg

Improvements
============

-   Dmitrijs Ledkovs fixed configure to use python-config if it exists
-   Dmitrijs Ledkovs provided python3 compability changes

Reference Profile updates
=========================

-   Intrigeri provided abstractions/fonts improvements
-   Felix Geyer added Dolphin (default Kubuntu file manager) to the list of file managers in abstractions/ubuntu-browsers.d/ubuntu-integration.
-   Move poppler's cMaps from gnome to fonts; gnome includes fonts
-   Deny writes to upstart user sessions jobs in abstractions/private-files
-   Deny @{HOME}/.gnome2/keyrings/\*\* to abstractions/private-files-strict
-   Add read access to @{PROC}/sys/vm/overcommit\_memory to abstractions/base
-   Update pulseaudio directory and cookie file paths
-   Add missing permissions to the nscd profile.
-   Deny capability block\_suspend to nscd
-   MariaDB compatability in abstractions/mysql

