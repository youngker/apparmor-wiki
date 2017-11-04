Standardized AppArmor Policy
============================

This documents the standardized AppArmor policy layout, and is what is
expected by the AppArmor tools. The enforcement of AppArmor policy does
not depend on this layout and it can be modified by setting config file
options or even completely replaced if the tools are modified/replaced.

Tool settings
=============

Configuration settings for the AppArmor tools are stored in two
locations:

-   Global and system settings are stored in `/etc/apparmor/`
-   User settings are stored in `${HOME}/.apparmor/`

Policy Layout
=============

System policy is stored in the **/etc/apparmor.d/** directory and,
when implemented user policy will be stored in **${HOME}/.apparmor.d/**
(**${APPARMOR.D}** is used to refer to both of these directories). The
${APPARMOR.D} directory contains a set of directories and profile
files. All files within the directory are treated as profiles unless
they match any of the following patterns in which case the files
are ignored.

-  \*~
-   \.bak
-   \.rpm\*
-   \.dpkg-{bak,dist,new,old}

Within ${APPARMOR.D} the following directories have special meaning:

-   `${APPARMOR.D}/abstractions/`
-   `${APPARMOR.D}/apache2.d/`
-   `${APPARMOR.D}/cache/`- precompiled binary policy files
-   `${APPARMOR.D}/disable/`- symlinks to profiles to disable
-   `${APPARMOR.D}/force-complain/`- symlinks to profiles to set to complain mode
-   `${APPARMOR.D}/libvirt/`
-   `${APPARMOR.D}/local/`
-   `${APPARMOR.D}/namespaces/`
-   `${APPARMOR.D}/pam/`
-   `${APPARMOR.D}/tunables/`

Your system may have some or all of the above directories, depending
on your OS vendor and version.

### ${APPARMOR.D}/abstractions/

This directory contains files and directories of common profile
'chunks' that can be included by profiles files. These files can
be thought of as libraries of access rules. Abstraction files may
include other abstractions (when appropriate -- doing too often can
make profile auditing less clear). By convention, sub-directories
are used to group abstractions that are logically related.

AppArmor integrators can provide their own set of abstractions
and changes in a subdirectory. For example, Ubuntu uses
abstractions/ubuntu-browsers.d/ to contain abstractions for profiling
web browsers in Ubuntu.

### ${APPARMOR.D}/apache2.d/ - DRAFT

Optional directory containing mod\_apparmor hat profiles for Apache. See [mod\_apparmor\_example](mod_apparmor_example) for more information.

### ${APPARMOR.D}/cache/

This directory contains a file for each file in the profiles directory
that has been precompiled and cached. The file name matches that of the
profile file that it is caching. A file with the same name prefixed
with '.' will contain the list of files used to build the cached
binary and can be used to verify that the cache files are valid. Eg:

```
 ${APPARMOR.D}/usr.bin.evince                # profile file
 ${APPARMOR.D}/cache/usr.bin.evince          # binary cache of usr.bin.evince profile
 ${APPARMOR.D}/cache/.usr.bin.evince         # list of files (one per line) used to create ${APPARMOR.D}/cache/usr.bin.evince
```

There is a special **.features** file which contains information about
the kernel features when the cached binary policy was built. This
allows for invalidating the cache when it doesn't match what is
supported by the current kernel.

### ${APPARMOR.D}/disable/

This directory may contain symlinks to profile files in the
${APPARMOR.D} directory. Profiles with symlinks in this directory
are ignored by the initscripts.

### ${APPARMOR.D}/force-complain/

This directory may contains symlink to profile files in the
${APPARMOR.D} directory. Profiles with symlinks in this directory
are loaded in complain mode by the initscripts.

### ${APPARMOR.D}/libvirt/

Systems using an AppArmor-enabled libvirt will also have the
${APPARMOR.D}/libvirt/ directory. This directory contains dynamically
generated profiles for individual virtual machines. The dynamic
profiles are template-based with names based on the UUID of the domain
XML of the virtual machine. Please see the libvirt documentation for
more information.

### ${APPARMOR.D}/local/

This directory is intended to contain profile additions and
overrides to distributed profiles to aid in packaging AppArmor
for distributions. The main profiles in ${APPARMOR.D} can still be
modified by an administrator and people should modify the main profile
when making large policy changes, rather than trying to make those
adjustments in the local include.

For simple access additions or the occasional deny override, adjusting
them here can prevent the package manager of the distribution from
interfering with local modifications. For example, if the main
${APPARMOR.D}/usr.sbin.smbd profile has:

```
 #include <local/usr.sbin.smbd>
```

then an administrator can adjust ${APPARMOR.D}/local/usr.sbin.smbd
to contain any additional paths to be allowed, such as:

```
 /var/exports/** lrw,
```

Keep in mind that 'deny' rules are evaluated after allow rules,
so you won't be able to allow access to files that are explicitly
denied by the shipped profile using this mechanism. In most cases,
the main profile should be adjusted when you want to deny specific
accesses to avoid confusing and inflated policy.

### ${APPARMOR.D}/namespaces/

This directory contains the policy for sub-namespaces. Sub-namespace
policy may be defined in other places as well, such as in the
${APPARMOR.D} directory in a chroot. The layout of the sub-namespaces
directory is the common set of directories described here, one per
namespace being defined with the name of the directory being the name
of the namespace.

Because the layout of the namespace directory is the same as that of
${APPARMOR.D}, sub-namespaces can have their own sub-namespace. The
abstractions and other directories can be unique to the namespace or
they can be shared with profile directories by using symlinks. Eg:

```
 /etc/apparmor.d/namespaces/foo/
 /etc/apparmor.d/namespaces/foo/abstractions
 /etc/apparmor.d/namespaces/foo/usr.bin.evince
 /etc/apparmor.d/namespaces/bar/
 /etc/apparmor.d/namespaces/bar/abstractions
 /etc/apparmor.d/namespaces/bar/usr.bin.evince
 ...
```

NOTE: users can not currently define their own namespaces.

### ${APPARMOR.D}/pam/ - DRAFT

Optional directory containing pam\_apparmor profile mappings and/or
includes. See [Pam\_apparmor\_example](Pam_apparmor_example) for
more information.

### ${APPARMOR.D}/tunables/

The tunables directory contains variable and alias definitions
files that can be included as part of the preamble of profile
files. Depending on your OS and installed profile set, this directory
might contain:

-   alias - used to define AppArmor aliases
-   global - used to include other tunables files, such as alias, home, and proc
-   home - used to define the @{HOME} and @{HOMEDIRS} variables
-   home.d/ - directory containing files used to define additional values for the @{HOMEDIRS} variable
-   ntpd - used to define the @{NTPD\_DEVICE} variable
-   proc - used to define the @{PROC} variable

Profile file naming conventions
===============================

The profile name used by AppArmor is defined by the executable being
confined, not the name of the file containing policy. The profile
files within ${APPARMOR.D} can be arbitrarily named, but the file name
should relate in some meaningful way to the profile that it contains.

By convention, AppArmor uses a few different naming conventions
depending on the type of profiles that are contained. The AppArmor
tools will use these conventions when generating profiles, but will
respect any file names that are manually specified.

If a file contains profiles that do not specify attachment (ie does
not start with a **/** character), then the file name should begin
with **profile\_**, **role\_**, or some other prefix that helps
describes the profiles purpose. Eg:

```
 ${APPARMOR.D}/profile_example1
 ${APPARMOR.D}/role_log_admin
 ${APPARMOR.D}/pam_roles
```

If a file contains a profile that specifies attachment (ie starts
with a **/** character), then the file is named after the profile
it contains with the **/** characters changed to **.** and with the
leading **.** omitted. Eg. a profile confining /usr/sbin/smbd will
have its policy saved in

```
 ${APPARMOR.D}/usr.sbin.smbd
```

If a file contains any of the following:

-   multiple profiles
-   a profile with multiple names
-   uses globbing to specify attachment
-   specifies hats
-   uses child profiles

then the profile file should be named after single name that bests
describes the intent of the profile(s). Eg. The Ubuntu firefox profile
uses both globbing (/usr/lib/firefox-3.6.13/firefox-\*bin) and contains
multiple child profiles for Java. While the profile actually confines
/usr/lib/firefox-3.6.13/firefox-\*bin, ${APPARMOR.D}/usr.bin.firefox
is used as the file name since this is the way most people will launch
the application.

It is acceptable to use the basename of the executable base name as a file name if:

-   there will only be one instance of the executable, or
-   all instances will be defined in the file, or
-   it is the primary instance, with secondary instances being named using the dot convention described above

For example, for the firefox example above, it would be acceptable to have the profile file name be ${APPARMOR.D}/firefox

The tools will not generate profiles with these shortened names because
they do not have the required knowledge to verify the above conditions,
but they will respect any file name the administrator chooses to use.
