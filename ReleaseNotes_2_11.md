Introduction
============

AppArmor 2.11 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.8
kernel and ubuntu 16.10 kernel with the apparmor 3 development patches.

Note: These release notes cover all changes between 2.10.1 and 2.11 (up
to r3612) There aren't any release notes for 2.10.95 aka 2.11 beta1.

Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.11 have been updated to support running on pre 4.8 and 4.8+ kernels.

Highlighted new features
========================

-   apparmor\_parser now supports parallel compiles and loads
-   add support for stacking in apparmor\_parser
-   add full support for dbus, ptrace and signal rules and events to the utils
-   full rewrite of the file rule handling in the utils

Detailed changelog
==================

parser
------

-   add support for parallel compiles and loads (-j/--jobs, --max-jobs)
    -   enable dynamically scaling max jobs if new resources are brought online
-   support stacking in exec and change\_profile rules
-   check kernel stacking support when handling stacked transitions
-   allow debugedit to work on apparmor\_parser
-   allow change\_profile rules to accept an exec mode modifier
-   fix: make sure overlapping safe and unsafe exec rules conflict
-   fix memory leaks in variable failure cases
-   fix memory leaks triggered by unit tests
-   merge latest launchpad translations

libapparmor
-----------

-   implement aa\_stack\_profile and aa\_stack\_onexec
-   accept hostname with dots when parsing syslog
-   force libtoolize to replace existing files
-   python bindings: use \_\_init\_\_.py to import from LibAppArmor.py (fixes import failure with swig &gt; 3.0.8)
-   be consistent with the type used for buffer sizes
-   fix overflowed return value in load\_features\_file() Coverity CID \#55992
-   don't close invalid fd Coverity CIDs \#55996 and \#55997
-   error out on failing libapparmor test\_multi tests

Utils
-----

-   switch utils to python3
    -   python2 is still supported, but deprecated

-   add aa-enabled program to check AppArmor status (replacement for aa-status --enabled, with minimum dependencies)
-   replace Perl aa-exec with C aa-exec
    -   the new C aa-exec does not implement the --file option because it encouraged running programs as root, since root privileges were required to load the specified profile

aa-status:

    -   add JSON output options (--json, --pretty-json)
    -   make aa-status(8) work without python3-apparmor

aa-unconfined:

    -   use ss(8) instead of netstat by default (can be overridden with --with-netstat / --with-ss)
    -   fix netstat usage to include IPv6

aa-logprof, aa-genprof, aa-mergeprof

-   add FileRule and FileRuleset (full rewrite of handling file rules, including some bugfixes along the way)
-   add DbusRule and DbusRuleset classes
-   add support for dbus events to aa-logprof
-   add PtraceRule and PtraceRuleset classes
-   add support for ptrace log events to aa-logprof
-   add SignalRule and SignalRuleset classes
-   add support for signal log events to aa-logprof
-   add support for rlimit and signal rules to aa-mergeprof
-   aa-genprof: ask about profiles in extra dir (again)
-   add AARE class
-   handle versioned ruby interpreters
-   fix handling of link events in aa-logprof
-   handle quoted peers when parsing ptrace rules
-   fix aa-logprof crash when creating a named profile with a name looking like a file if that file doesn't exist
-   fix aa-mergeprof crash with files containing multiple profiles
-   prevent crash in aa-logprof “(V)iew changes” if a file contains multiple profiles
-   handle\_binfmt: resolve symlinks in library paths
-   change log\_dict to use profile\_storage() and simplify log translation
-   add a test to check for hotkey conflicts
-   load variables in ask\_the\_questions()
-   honor 'chown' file events in logparser.py
-   Handle the safe/unsafe change\_profile exec modes
-   delete\_duplicates(): make sure all superfluous rules get deleted
-   fix aa-logprof “add hat” endless looping
-   set audit mode for all options
-   AARE: let match() handle plain path regexes as non-regexes
-   re-enable clear\_common() call in aa-mergeprof to avoid superfluous questions
-   cleanup superfluous rules when user adds a new rule
-   ignore exec events for non-existing profiles
-   Load all includes in aa-mergeprof ask\_the\_questions()
-   let aa-mergeprof ask about new hats and subprofiles
-   test log to profile “translation” and add lots of libapparmor/test\_multi/\*.profile reference files
-   logparser.py: improve file vs. network event recognition
-   Handle ldd $? == 1 in get\_reqs() (instead of crashing)
-   change SignalRule to use AARE instead of plain strings
-   merge latest launchpad translations

Policy
------

abstractions:

-   add abstractions/wayland and include it in abstractions/gnome
-   add abstractions for fcitx input method framework (abstractions/fcitx, abstractions/fcitx-strict)
-   add abstraction for mozc input method editor (abstractions/mozc)
-   update php abstraction for PHP7, and rename it (abstractions/php5 -&gt; abstractions/php, abstractions/php5 still available as compability wrapper)
-   abstractions/base: add ld.so.preload
-   abstractions/dbus-session-strict:
    -   allow access to the user bus socket
    -   allow dbus-user-session D-Bus path
-   abstractions/gnome:
    -   add versioned gtk paths References:
    -   grant read access to ~/.config/gtk-3.0/\*.
    -   allow reading file type associations from another place where it can live on Debian.
-   abstractions/nameservice:
    -   allow read on /run/systemd/resolve/resolv.conf for systems using networkd
    -   support systems with NetworkManager but no resolvconf where /etc/resolv.conf is a symlink to /var/run/NetworkManager/resolv.conf
    -   support systems that use libnl-3-200 via libnss-gw-name.
    -   also support ConnMan-managed resolv.conf
-   abstractions/ubuntu-bittorrent-clients: add deluge-{gtk,console}
-   abstractions/ubuntu-browsers: support Debian's firefox-esr
-   abstractions/ubuntu-unity7-base: update to use dbus abstractions instead of repeating the rules here
-   abstractions/user-mail: /var/mail/\* should only be accessible to their owners
-   abstractions/X:
    -   allow reading /tmp/.X11-unix/\*
    -   yet another location for Xauthority (/{,var/}run/user/\*/X11/Xauthority)

dnsmasq profile:

-   extract confinement of libvirt\_leaseshelper into a dedicated sub-profile
-   Allow reading conf snippets from /etc/dnsmasq.d-available
-   add lxd-bridge rules
-   Add attach\_disconnected flag
-   several dnsmasq.\* additions for lxd
-   allow libvirt\_leaseshelper “m” permission on itself.
-   extract confinement of libvirt\_leaseshelper into a dedicated sub-profile.

Dovecot profiles:

-   dovecot: allow capability sys\_resource
-   dovecot/auth: allow to read /run/dovecot/stats-user
-   dovecot/auth: allow access to /run/dovecot/anvil-auth-penalty and /var/spool/postfix/private/auth
-   dovecot/config: allow to read /usr/share/dovecot/\*\*
-   dovecot/imap: allow to ix doveconf, read /etc/dovecot/ and /usr/share/dovecot/\*\*
-   dovecot-lda: allow tempfiles and executing sendmail
-   dovecot/lmtp: allow reading ~/.dovecot.svbin
-   dovecot/log: add attach\_disconnected flag

Samba profiles:

-   allow mr for /usr/lib\*/ldb/\*.so in samba abstractions
-   abstractions/samba: allow /var/cache/samba/lck/\*
-   add several permissions to smbd profile
-   winbindd: allow dac\_override (needed to delete kerberos ccache files)
-   Add several /var/cache/samba/ permissions to nmbd profile and abstractions/samba

other profile changes:

-   update lots of profiles for usrMerge
-   allow inet6 in ping profile
-   traceroute: allow both paths (to match the alternation in the profile name)
-   syslog-ng profile: allow writing \*.qf files
-   update mlmmj profiles
-   ntpd: allow “network unspec dgram,”
-   nscd:
    -   allow paranoia mode
    -   allow reading libvirt/dnsmasq/\*.status
-   usr.lib.apache2.mpm-prefork.apache2: allow CAP\_CHOWN
-   usr.sbin.sshd: allow reading blacklisted host keys

Documentation
-------------

apparmor.d manpage:

-   document empty quotes ("") as empty value of a variable
-   add network 'kcm' keyword
-   fix 'alias' rule description in apparmor.d manpage
-   document change\_profile exec modes
-   document aliases for dbus send and receive

Utils:

-   add a note about still enforcing deny rules to aa-complain manpage
-   move aa-exec and aa-enabled manpages to section 1

Library manpages:

-   create man page for aa\_stack\_profile()/aa\_stack\_onexec()
-   correct meaning of EPERM in aa\_change\_profile man page
-   add funcs to the NAME section of aa\_query\_label(2) - (aa\_query\_file\_path, aa\_query\_file\_path\_len, aa\_query\_link\_path, and aa\_query\_link\_path\_len)
-   aa\_change\_profile: document that open fds may be revalidated after aa\_change\_profile()

Build:

-   build documentation at tarball creation time, not during build to avoid latex build dependency
-   generate pdfs from odt files

tests
-----

-   create a program for stacking tests to use
-   add stackprofile regression tests
-   add stackonexec regression tests
-   Add exec\_stack regression tests
-   regression tests to verify AT\_SECURE across exec transitions
-   add regression tests for aa-exec
-   add lots of tests covering the new code in the python utils
-   update tests to work with kernel semantic change introduced by 9f834ec18defc369d73ccf9e87a2790bfa05bf46

other changes
-------------

-   README: Document Coverity support

-   Update change\_profile highlighting in apparmor.vim

-   Add out of tree kernel patches for several 4.x kernel versions

-   pam\_apparmor: Don't leak /dev/urandom fd Coverity CID \#56012

