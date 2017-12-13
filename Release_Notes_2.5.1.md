AppArmor 2.5.1 Release Notes
============================

The 2.5.1 release of the AppArmor user space tools fixes
bugs and includes minor enhancements over the [AppArmor 2.5
release](Release_Notes_2.5). Official kernel patch updates will be
published at a later date.

-   For information on other versions please see [AppArmor versions](AppArmor_versions)

Bug Fixes and Enhancements
--------------------------

### AppArmor Profiles

-   (LP: \#611248) Fix gnome abstraction for gdk pixbuf loaders
-   (LP: \#538661) Adjust cgi path for php5 abstraction
-   Add 'k' to /var/lib/samba/\*\*.tdb in the samba abstraction
-   abstractions/user-tmp: require 'owner' matching
-   profiles/apparmor.d/abstractions/base: statvfs allowed by default
-   Add dbus-session abstraction (and use Pix rather than Uix)

### AppArmor Parser

-   (LP: \#599450) Change the table resizing so that there is always sufficient high entries in the table, preventing bounds violations from occurring.
-   (LP: \#626984) Prevent the parser from crashing when run against 2.6.36 upstream version of AppArmor which doesn't present information parser expects.
-   Move expression tree node labeling into expr node themselves to reduce memory usage and make node labeling per dfa rather than global.
-   Clean up the sets firstpos, lastpos, and followpos early to reduce peak memory usage.
-   Add the ability for the apparmor\_parser to dump flattened profiles. Passing the -p flag to the apparmor\_parser causes it to dump a flattened profile that includes all the text for all includes to stdout.
-   Fix memory leak during dfa minimization.
-   (LP: \#588012) Fix leaking file descriptors on included files.
-   (LP: \#588014) Report correct filename/line number on errors in the parser.
-   Detect when abstractions have been modified, and invalidate profile cache file when reloading.
-   Fix compilation/build warnings.

### AppArmor Library (libapparmor)

-   Fix perl swig bindings so that libapparmor can be built when configured without perl.
-   Add support for LSM\_AUDIT format messages
-   Update support for minor message changes that occurred as part of upstreaming effort

### AppArmor Desktop Notifier (apparmor\_notify)

-   Fix memory leak
-   (LP: \#582075) apparmor\_notify group like entries together when using -v with -s
-   Setting in notify.conf now defaults to on (apparmor\_notify is not usually installed by default)
-   Add long options
-   Cleanup output
-   Better handle auditd
-   Handle logfile rotation
-   Use seteuid() to drop privileges so we can raise/drop after log file rotation. Add -u USER option for dropping privileges when not using sudo
-   Update man page

### AppArmor Utils (genprof/logprof)

-   (LP: \#623467) SubDomain.pm: add support for distinct reported
    truncate, rename\_src, rename\_dest, and mkdir operations

### AppArmor PAM Library (pam\_apparmor)

-   (LP: \#619521) Teach pam\_apparmor about the current errno returned
    by the kernel when the hat that was passed does not exist in the
    profile (but other hats exist).

### AppArmor Kernel Patches

There are several kernel options available with the 2.5.1 release. The
2.5.1 userspace should work with kernel patches from version 2.3 and
later. Due to the timing of the 2.5.1 release the official kernel
patches come in two flavors a patched 2.5 kernel and variants of the
newer kernel patches the were accepted into Linux 2.6.36

-   Patched 2.5 kernel
    -   available for kernel versions: 2.6.31, 2.6.32, 2.6.33
-   Upstream Linux 2.6.36 kernel - AppArmor is available in

-   Upstream 2.6.36 kernel + compatibility patches
-   2.6.36 version of AppArmor on other kernel versions
    -   2.6.34
    -   2.6.35
