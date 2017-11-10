AppArmor 2.9.0 Release Notes
============================

[AppArmor 2.9.0](https://launchpad.net/apparmor/+milestone/2.9.0)
introduces significant changes over the prior AppArmor 2.8 series.

-   For information on other versions please see [AppArmor versions](AppArmor_versions)

New Features and Improvements Highlights
----------------------------------------

-   Support for dbus mediation (requires dbus patch, work is currently underway to get this accepted in dbus upstream)
-   Support for mediating ptrace, signals, and unix abstract sockets (needs kernel patches)
-   User tools rewritten in Python, Perl versions deprecated
-   The mod\_apparmor apache module uses the vhost name as a default hat name, if one is not defined (see mod\_apparmor(8) for details)
-   Lots of bug fixes, added testcases, and reference policy improvements

Language Changes
----------------

-   New “allow” keyword
-   New dbus mediation type
-   New ptrace mediation type
-   New signal mediation type
-   New unix mediation type

Please see the [AppArmor Core Policy Reference](AppArmor_Core_Policy_Reference) for more information.
