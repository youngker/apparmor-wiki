AppArmor 2.9.4 Release Notes
----------------------------

AppArmor 2.9.3 is an incremental bug fix release over AppArmor 2.9.2
that is focused on fixing issues in the userspace code.

It includes the changes in the [2.9
branch](http://bazaar.launchpad.net/~apparmor-dev/apparmor/2.9/changes/3004?start_revid=3005)
between r3005 (AppArmor 2.9.3) and r3043.

Library
-------

-   accept hostname with dots when parsing syslog
-   force libtoolize to replace existing files

Utils
-----

-   aa-genprof: ask about profiles in extra dir (again)
-   honor 'chown' file events in logparser.py
-   fix aa-logprof “add hat” endless looping
-   ignore exec events for non-existing profiles
-   ignore file events with a request mask of 'send' or 'receive' (which are actually network events)
-   handle ldd $? == 1 in get\_reqs() (instead of crashing)

-   aa-unconfined: fix netstat usage to include IPv6

Policy
------

abstractions:

-   update php abstraction for PHP7, and rename it (abstractions/php5 -&gt; abstractions/php, abstractions/php5 still available as compability wrapper)
-   abstractions/dbus-session-strict: allow access to the user bus socket
-   abstractions/gnome: add versioned gtk paths References:
-   abstractions/X: allow reading /tmp/.X11-unix/\*
-   abstractions/X: yet another location for Xauthority (/{,var/}run/user/\*/X11/Xauthority)

Samba profiles:

-   abstractions/samba: Allow /var/cache/samba/lck/\*
-   allow mr for /usr/lib\*/ldb/\*.so in samba abstractions
-   winbindd: allow dac\_override (needed to delete kerberos ccache files)
-   add several /var/cache/samba/ permissions to nmbd profile and abstractions/samba

Dovecot profiles:

-   dovecot: allow capability sys\_resource
-   dovecot/auth: allow to read /run/dovecot/stats-user
-   dovecot/auth: allow access to /run/dovecot/anvil-auth-penalty and /var/spool/postfix/private/auth
-   dovecot/config: allow to read /usr/share/dovecot/\*\*
-   dovecot/imap: allow to ix doveconf, read /etc/dovecot/ and /usr/share/dovecot/\*\*
-   dovecot/lmtp: allow reading ~/.dovecot.svbin
-   dovecot/log: add attach\_disconnected flag

other profile changes:

-   allow inet6 in ping profile
-   traceroute: allow both paths (to match the alternation in the profile name)
-   syslog-ng profile: allow writing \*.qf files
-   update mlmmj profiles
-   ntpd: allow “network unspec dgram,”
-   nscd profile: allow reading libvirt/dnsmasq/\*.status

Documentation
-------------

-   apparmor.d.pod: Document empty quotes ("") as empty value of a variable
-   add a note about still enforcing deny rules to aa-complain manpage

