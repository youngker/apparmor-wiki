A quick guide to AppArmor profile Language
==========================================

This is a quick guide to the AppArmor profile language, for an in
depth guide see the [Profile Language Reference](ProfileLanguage)

AppArmor policy is created using an administrator friendly profile
language that is then compiled into a binary policy for loaded
into the kernel. AppArmor policy is stored in a set of files in
**/etc/apparmor.d/**. AppArmor policy is split into profiles which
usually are designed to confine a specific application. Profiles
will declare access rules to allow access to resources, and access
is implicitly denied with logging when there is no matching rule in
the profile.

Profiles
--------

Profiles begin with the name of the profile followed by an optional
flags field, then an opening **{** followed by the rules for the
profile and is finished by a closing **}** If the profile name does not
begin with a **/** then the keyword profile should be prepended. Eg:

```
 /usr/bin/firefox {
    # profile contents
 }

 /usr/bin/firefox flags=(complain) {
    # profile contents
 }

 profile /usr/bin/ {
    # profile contents
 }

 profile user1 {
    # profile contents
 }
```

Profile names can contain file rule globbing characters to allow
them to apply to multiple executables, see File Globbing (below)
for details.

Comments
--------

Comments are line oriented and begin with a **\#**. Text following
a *'\#* to the end of the line is ignored (with the exception of the
\#include rule). Eg:

```
 #  Comment 1
 #  Comment 2

 profile example {  # comment 3
    # comment 4
    /home/foo rw,  # comment at the end of a file rule
 }
```

Include Rules
-------------

Policy files can include other files to share text segments. AppArmor
includes follow the C include convention. They start with **\#include**
followed by either **&lt;** **&gt;** for text chucks relative to
a set include directory or **"** **"** for files relative to the
current file. The **\#** is optional and shouldn't be used in newer
profiles. Eg:

```
 #include <file>
 #include “a/relative/path/file”
 include <file>
```

**\#include** conflicts with commenting rules and takes precedence. The
**\#** and include must not be separated from the **include** with
white space otherwise it is considered a comment

```
 # include is a comment
 #include <file>
```

### Child profiles

A profile can contain children profiles. Child profiles can be used
to confine an application in a special way, or when you want the
child to be unconfined on the system, but confined when called from
the parent. Eg:

```
 /parent/profile {
     /path/to/child1 cx -> child1,
     /path/to/child2 cx,
     /path/to/* cx,           # for * matching child3 will transition to child3,
                              # child4, child5, ... will transition to /path/to/child*
                              # if matching child profile does not exist will fail
     /another/path/to/* cx -> child1,        # send all matching execs to child1
     profile child1 {
     }
     profile /path/to/child2 {
     }
     profile /path/to/child3 {
     }
     # generic fall back profile
     profile /path/to/child* {
     }
  }
```

### Hats

Hats are a special child profile that can be used with the change\_hat
API call. To denote a hat, prepend **^** before the hat name with no
spaces. Eg:

```
 /parent/profile {
     ^hat {
     }
 }
```

Capability Rules
----------------

AppArmor supports course grained access to Linux' POSIX-style
capabilities (see 'man capabilities) and capabilities rules are
used to allow access to these capabilities. For example, a setuid
application which drops privileges might need:

```
 /profile {
    capability setuid,
    capability setgid,
 }
```

Network Rules
-------------

AppArmor currently supports course grained access to networking via
network rules. For example, a network daemon might need:

```
 /profile {
   network inet dgram,
   network inet stream,
 }
```

Or a packet analyzer might need:

```
 /profile {
   network raw,
   network packet,
 }
```

Rlimit Rules
------------

TODO

File rules
----------

File rules control how files are accessed and only occur within
a profile. They consist of a pathname, a permission set and are
terminated by a comma. They can be written with either the permission
first or the pathname first, though the convention it to list the
path first. A valid pathname always begins with a **/**. Eg:

```
 /profile {
    /path/to/file  rw,   # file rule beginning with a pathname (convention)
    rw /path/to/file2,   # file rule beginning with permissions
    /path/to/file3       # file rule split over multiple lines
         rw,
 }
```

File rules can contain special globbing characters that allow matching
to multiple files (see File Globbing, below)

See AppArmor **File Permissions** for more detail about file permissions.

### File Globbing

AppArmor uses a file globbing syntax similar to that used by the
bash shell. Globbing is not standard full regular expression syntax,
but instead uses a few characters known as wild cards. The AppArmor
wildcards have slightly different semantics than that of bash.

-   <b>\*</b> - match zero or more characters at the directory level. When looking at the path as a string this will match every character except <b>/</b>
    -   This will match dot files (file names starting with <b>.</b>), excepting the special dot files <b>.</b> and <b>..</b>, if it is placed immediately after the directory /
        eg. /dir/\*
    -   This will not match an empty directory string eg. /dir//
    -   pcre equivalent is (\[^/\\000\]\*)
-   <b>\*\*</b> - match 0 or more characters over multiple directory levels.
    -   This will match dot files (file names starting with <b>.</b>), excepting the special dot files <b>.</b> and <b>..</b>, if it is placed immediately after the directory /
        eg. /dir/\*\*
    -   pcre equivalent is (\[^\\000\]\*)
-   <b>?</b> - match a single character that is not <b>/</b>
    -   pcre equivalent is \[^/\]
-   <b>{}</b> - alternation - a comma separated list of alternate strings that can be matched. An empty string is allowed and means the empty string is a viable alternative
    -   pcre equivalent is (|)
-   <b>\[\]</b> - character class
    -   same as pcre syntax
-   <b>\[^\]</b> - inverted character class
    -   same as pcre syntax
-   Nesting expressions in alternations (as of AppArmor 2.3):
    -   escaping characters \\\*
    -   expressing characters as \# \\001

#### Proposed additions

The following are proposed additions to the current file globbing and are not currently implemented:

-   <b>{\*^}</b> - a glob similar to \* with an alternation style list of things it is not allowed to match.
    eg. /etc/{\*^shadow} is the same as allowing everything /etc/\* matches except /etc/shadow
    eg. /etc/{\*^shadow,passwd} is the same as /etc/\* - /etc/{shadow,passwd}
    eg. /etc/{\*^\*shadow} is the same as /etc/\* - /etc/\*shadow
    \* note that an empty alternation entry is not allowed ie. {\*^shadow,}
-   <b>{\*\*^}</b> - a glob similar to \*\* with an alternation style list of things is is not allowed to match eg. /etc/{\*\*^shadow} is the same as /etc/\*\* matches - /etc/shadow
    eg. /etc/{\*\*^shadow,passwd} is the same as /etc/\*\* - /etc/{shadow,passwd}
    eg. /etc/{\*\*^\*shadow} is the same as /etc/\*\* - /etc/\*shadow
    \* note that an empty alternation entry is not allowed ie. {\*\*^shadow,}

#### File Globbing examples

```
/dir/file     - match a specific file
/dir/*        - match any files in a directory (including dot files)
/dir/a*       - match any file in a directory starting with 'a'
/dir/*.png    - match any file in a directory ending with '.png'
/dir/[^.]*    - match any file in a directory except dot files

/dir/         - match a directory
/dir/*/       - match any directory within /dir/
/dir/a*/      - match any directory within /dir/ starting with a
/dir/*a/      - match any directory within /dir/ ending with a

/dir/**       - match any file or directory in or below /dir/
/dir/**/      - match any directory in or below /dir/
/dir/**[^/]   - match any file in or below /dir/

/dir{,1,2}/** - match any file or directory in or below /dir/, /dir1/, and /dir2/
```

### File permissions

The following file permissions are supported:

-   r - read
-   w - write
-   a - append (implied by w)
-   x - execute
    -   ux - Execute unconfined (preserve environment) -- WARNING: should only be used in very special cases
    -   Ux - Execute unconfined (scrub the environment)
    -   px - Execute under a specific profile (preserve the environment) -- WARNING: should only be used in special cases
    -   Px - Execute under a specific profile (scrub the environment)
    -   pix - as px but fallback to inheriting the current profile if the target profile is not found
    -   Pix - as Px but fallback to inheriting the current profile if the target profile is not found
    -   pux - as px but fallback to executing unconfined if the target profile is not found
    -   Pux - as Px but fallback to executing unconfined if the target profile is not found
    -   ix - Execute and inherit the current profile
    -   cx - Execute and transition to a child profile (preserve the environment)
    -   Cx - Execute and transition to a child profile (scrub the environment)
    -   cix - as cx but fallback to inheriting the current profile if the target profile is not found
    -   Cix - as Cx but fallback to inheriting the current profile if the target profile is not found
    -   cux - as cx but fallback to executing unconfined if the target profile is not found
    -   Cux - as Cx but fallback to executing unconfined if the target profile is not found
-   m - memory map executable
-   k - lock (requires r or w, AppArmor 2.1 and later)
-   l - link

The owner keyword can be used as a qualifier making permission
conditional on owning the file (process fsuid == file's uid).

` owner /foo rw,`

The following are in development:

-   create (implied by w)
-   delete (implied by w)
-   chown - change ownership (implied by w)
-   chmod - change mode (implied by w)

#### Execute permissions

AppArmor distinguishes between the different ways a file may be
executed. Because a new process is created when executing a file,
the process is said to transition to another (possibly same) profile
across execute.

The base execute permission are:

-   **ix** - the new process should run under the current profile
-   **cx** - the new process should run under a child profile that matches the name of the executable
-   **px** - the new process should run under another profile that matches the name of the executable
-   **ux** - the new process should run unconfined

A process that runs unconfined is actually in the built-in unconfined
profile, which allows everything with no logging.

The px, cx, and ux permission when written using a capitalized
leading character (**Px**, **Cx**, **Ux**) will trigger libc's Secure
Execution. When developing profiles, the Secure Execution variants
should in general be used so that the executed program starts in a
clean environment.

The px and cx rules (and their Secure Execution variants) may also have
an ix, or ux fallback, expressed as pix, pux, cix, or cux. Using the
fallback indicates that the process should run under the profile if
it exists, otherwise the profile transition should use the specified
ix or ux transition. It is often a good idea to use 'PUx' instead of
'Ux' so you don't have to update your profile for when the executed
program has an AppArmor profile added later. For example, if a confined
program should be allowed to run 'evince', then the profile might have:

` /usr/bin/evince PUx,`

The px and cx rules (and all their variants) can also be modified to
specify a profile by name instead of using the profile that matches
the name of the executable. This is done by providing a **-&gt;**
transition arrow and the name of the profile.

` /foo px -> profile1,`

For directories, the UNIX execute permission maps to search access
and AppArmor does not further control directory search access. In
other words, traversing directories is granted if DAC permits it.

#### How AppArmor file permissions differ from DAC

On traditional UNIX systems using DAC, when files are looked up by
name, the lookup starts either at the root or the current working
directory of a process. From there, each directory reached is checked
for search permission (x). The permissions on the directories leading
to the current working directory are not checked. When a file is being
created or deleted, the parent directory of that file is checked
for write and search access (wx). When a file is being accessed,
the permissions of that file are checked for r, w, or x access, or
a combination thereof. Each check can result in a failure with errno
set to EACCES (Permission denied).

AppArmor provides an additional permission check to DAC. DAC is
always checked in addition to the AppArmor permission checks. As such,
AppArmor cannot override DAC to provide more access than what would
be normally allowed.

Path permission checking is calculated differently for AppArmor than
in DAC. AppArmor first computes the pathname to the file. If the
file is being created, the name being looked up is the name of the
new file and not the name of the parent directory. If the file being
looked up is a directory, AppArmor appends a slash to the pathname
(this means a directory pathname in a profile must always end in
a slash). As a convenience, AppArmor also allows pathnames to be
rewritten via aliases.

After determining the pathname to evaluate, AppArmor then checks for
file access rules in the process' profile that match that pathname,
and performs permission checks based on these rules. With some
exceptions for execute modes and deny mode, the permissions granted
are the union of permissions of all matching rules.

More simply, creation of files requires the create permission (implied
by w) on the path to be created. Permissions to write to the directory
the file is created or accessed in is not required. Deletion works
like creation but requires the delete permission (implied by w). Copy
requires 'r' of the source with create and write at the destination
(implied by w). Move is like copy, but also requires delete at
source. For example:

The permissions to create and/or delete a file are:


` /foo/bar      w,`

The permissions to copy a file are:

```
 /foo/src      r,
 /foo/dst      w,
```

The permissions to move a file are:

```
 /foo/src     rw,
 /foo/dst      w,
```

Notice in the above that AppArmor does not require additional rules
in the policy to access or write to the / or /foo/ directories (DAC
rules still apply though).

For directories, read access (r) allows reading of directory meta
data (eg, owner) and reading of directory contents (ie listing)
but is not needed for traversal. Write access (w) allows creation,
deletion, and updating of directory meta data.

#### AppArmor file labeling

AppArmor will assign a default label to a file (as opposed to storing
that label in the file's inode). When a process opens a file, the file
object is assigned a label, which can be thought of as the profile
name. A file can have multiple labels for when different processes
want to access the same file to allow for different processes having
different access controls. In practice, when developing policy one
just refers to files by name, and the kernel handles all the necessary
labeling behind the scenes. As such, AppArmor is often referred to as
'pathname-based'.

Since AppArmor labels files by pathname (rather than using on disk
labeling), the administrator does not need to perform a relabelling
step after a file is overwritten or moved. For example, if a process
is granted read access to /etc/shadow and the system administrator
renames /etc/shadow to /etc/shadow.old and replaces it with a copy
(that may have an additional user in it, for example), the process
will have access to the new /etc/shadow, and not to /etc/shadow.old.

Rule Modifiers
--------------

When there is no corresponding rule for a resource, AppArmor will
block access to the resource and log it. When there is a rule in
the policy, access is allowed to the resource without logging. The
following modifiers can be prepended to a rule to change this behavior:

-   audit: force logging
-   deny: explicitly deny, without logging
-   audit deny: combination to explicitly deny, but log

Eg:

```
 /profile {
    /path/to/file*            r,  # allow read to /path/to/file*
    /path/to/file1            w,  # allow write to /path/to/file1
    deny /path/to/file2,      w,  # deny write to /path/to/file2, without logging
    audit /path/to/file3      w,  # allow write to /path/to/file3, with logging
    audit deny /path/to/file4 r,  # deny read to /path/to/file4, with logging
 }
```

**IMPORTANT:** deny rules are evaluated before allow rules and cannot
be overridden by an allow rule. They are often used to override file
globbing rules. For example, in the above policy the 'audit deny
/path/to/file4 r' rule above overrides the '/path/to/file\* r' rule.
