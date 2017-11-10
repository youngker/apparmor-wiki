AppArmor 2.6.0 Release Notes
============================

AppArmor 2.6 is incremental improvement over AppArmor 2.5, that has
focused on bug fixing and internal cleanups of the userspace code
over new features. AppArmor 2.6 is purely a userspace release, it
leverages the same kernel code as AppArmor 2.5.

-   For information on other versions please see [AppArmor versions](AppArmor_versions "wikilink")

New Features and Improvements
-----------------------------

### AppArmor Parser

-   add support for profile names that are independent of attachment specification
-   faster policy compilation, with less peak memory use
-   add a safe exec transition keyword
-   make leading x permissions consistent with trailing x permissions
-   new policy compilation information dump flags
-   write\_cache is no longer a privileged operation (DAC permissions still apply)
-   use file timestamps to determine if cache is stale on load
-   fix dfa graph dumping
-   add -o option to dump compiled policy to a file
-   reintroduce -p (preprocess) flag
-   fix two x (execute) transition conflict bugs ([LP: \#693082](https://bugs.launchpad.net/apparmor/+bug/693082)) and add testcases
-   enable initscripts to work with upstream kernel that is missing compatibility patches
-   skip cache tests during build when securityfs is not mounted
-   break out make targets so that distributors that don't want full documentation can pick the targets they want

### AppArmor Utils (aa-genprof/aa-logprof)

-   standardize on all utils using the “aa-” prefix
-   add aa-disable, a utility to disable profiles
-   updated apparmor.vim to more accurately parse current policy language syntax
-   abstract out the perl vendor location for distros to override if necessary at installation time
-   fix to set complain mode on subprofiles ([LP: \#707092](https://bugs.launchpad.net/apparmor/+bug/707092))
-   other minor bugfixes

### AppArmor Library (libapparmor)

-   add support for newer auditd formatted messages.
-   make change\_hatv(), change\_hat\_varargs() available via swig interfaces
-   fix python swig bindings to be functional

### AppArmor release wide changes

-   new/updated regression tests
-   new and updated profile abstractions
-   new and updated reference profiles
-   refreshed kernel compatibility patches for most recent versions of the kernel
-   updated documentation and translation files
-   Fix up tomcat build
-   make setup target work independently
-   replace SubDomain with AppArmor in most instances
-   build, code, and comment cleanups

AppArmor Policy Language Changes
--------------------------------

There have been a few changes to the profile language

### Profile Naming and Attachment Specification

Profiles can now have a name and an attachment specification. This
allows for profiles with a logical name that can be more meaningful to
users/administrators than a profile name that contains globbing. For
example, the default profile:

```
  /** { .. }    # a default profile
```

can be named

```
  profile default /** { .. }
```

Also, a profile using globbing/regular expressions can be named; for example:

```
  /usr/lib/firefox-3.*/firefox-*bin { }
```

can be named

```
  profile firefox /usr/lib/firefox-3.*/firefox-*bin { }
```

### Safe/unsafe Keyword

The safe and unsafe keywords have been added, this allows rules with
x transitions to have environment scrubbing specified by keyword
instead of using the case of the x modifier.

```
 /example1 Px,
```

is the same as

```
 safe /example1 px,
 safe /example1 Px,
 safe px /example1,
 safe Px /example1,
```

and

```
 /example2 px,
 ```

is the same as

```
 unsafe /example2 px,
 unsafe /example2 Px,
 unsafe px /example2,
 unsafe Px /example2,
```

The safe/unsafe keywords are mutually exclusive and can be used in
a file rule after the owner keyword, i.e.:

```
 [audit] [deny] [owner] [safe|unsafe] filerule,
```

### Consistent x (execute) Permissions

Leading and trailing x (execute) permissions have been made
consistent. Previously when x permissions where used in the leading
position

```
 px /example
```

The case of the x modifier was ignored and the unsafe keyword was
required to indicate environment variable scrubbing was not to be done
on exec. The case on the leading x permissions is now consistent with
the use when the x permissions are specified after the path in a rule.

The safe and unsafe keywords retain the same function but can be used
with all file rules that have x transitions.

This behavior change may affect profiles written using leading x
permission that used lower case exec modifiers.

Features removed
----------------

Some deprecated and obsolete features were removed with this release:

-   utils/severity.pl - due to incorrect license/copyright statement
-   rc.sd-event-dispatch.suse - replaced by aa-eventd, which is not nearly but almost as obsolete
-   gnome apparmor applet - replaced by aa-notify
-   old v2.0 log parsing code from libapparmor
-   remove subversion deritrus
-   slackware tarball build support
-   embedded pcre code from AppArmor 2.0.x

