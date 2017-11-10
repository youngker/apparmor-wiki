AppArmor 2.8.4 Release Notes
============================

[AppArmor 2.8.4](https://launchpad.net/apparmor/+milestone/2.8.4)
is an incremental bug fix release over AppArmor 2.8.3 that focused
on fixing bugs in the userspace code and updating policy.

AppArmor 2.8.4 includes revisions 2123 through 2148 from the [2.8
branch](http://bazaar.launchpad.net/~apparmor-dev/apparmor/2.8/changes/2148?start_revid=2123).

**Important behavioral change announcement for apache mod\_apparmor users**
---------------------------------------------------------------------------

[ AppArmor 2.8.2](ReleaseNotes_2_8_2) and before contained a bug in
mod\_apparmor that as a side effect, caused a AADefaultHatName defined
hat to be attempted before the raw URI if no AAHatName had been set,
contrary to the documented ordering of attempted hats. [ AppArmor
2.8.3](ReleaseNotes_2_8_3) fixed this bug and thus implemented the
documented ordering. However, a number of users had come to depend
on the AADefaultHatName being attempted before the URI hat (); also,
each failed URI hat attempt would cause a kernel log event, creating
a lot of noise. The AppArmor 2.8.4 release reverts this behavioral
ordering change, and documents the original ordering as such:

1.  try to aa\_change\_hat(2) into a matching AAHatName hat if it exists and applies, otherwise it will
2.  try to aa\_change\_hat(2) into an AADefaultHatName hat if it has been defined, otherwise it will
3.  try to aa\_change\_hat(2) into the URI itself, otherwise it will
4.  try to aa\_change\_hat(2) into the DEFAULT\_URI hat, if it exists, otherwise it will
5.  fall back to the global Apache policy

Impacts:

-   People using only the AAHatName keyword, URI hats, or the default
    hat should not see any changes in behavior.

-   People using only the AADefaultHatName should see the same behavior
    as from 2.8.2 and prior releases

-   People using a mix of any of AAHatName, AADefaultHatName, and
    URI based hats will want to re-examine their policy and/or apache
    configuration, based on the ordering above.

Profile tools
-------------

-   properly handle bare 'file' keyword
-   handle bare 'capability' keyword
-   fix bare 'network' keyword handling
-   add glob for perl5 multiarch to logprof.conf
-   add entries for perl and CAP\_AUDIT\_READ to severity.db

parser
------

-   Remove \<sys/sysctl.h\> from parser\_main.c to fix FTBFS on x32 platform [Debian \#760378](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=760378)
-   fix FTBFS when building with clang [Debian \#756807](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=756807)
-   Fix profile loads from cache files that contain multiple profiles

libapparmor
-----------

-   fix log parsing memory leaks

Policy updates
--------------

#### Abstractions

-   added abstractions/dconf for querying dconf settings
-   abstractions/audio - add /etc/wildmidi/wildmidi.cfg
-   abstractions/freedesktop.org - ~/.local/share/applications/
-   abstractions/gnome - /usr/share/themes/, /usr/share/gnome/applications/
-   abstractions/kde - /etc/xdg/Trolltech.conf
-   abstractions/mysql - more mysql.sock locations
-   abstractions/nameservice:
    -   /var/lib/extrausers/{group,password}
    -   read access for avahi socket
    -   support nscd locations under /run
-   abstractions/nvidia - additional /proc and glcache locations
-   abstractions/openssl -
-   abstractions/perl - /usr/lib/@{multiarch}/perl{,5}/\*\*
-   abstractions/python - support more python versions
-   abstractions/web-data - add default debian/ubuntu document location

#### Reference Policies

-   usr.sbin.ntpd - alternate pid file location
-   usr.sbin.dnsmasq:
    -   NetworkManager integration
    -   allow ipv6 mtu discovery
-   usr.sbin.smbd -

