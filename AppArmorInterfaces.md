Introduction
============

This page provides documentation of the low level interfaces to the
apparmor kernel module. It is not generally recommended that the low
level interfaces are accessed directly, the apparmor api??? provides
a higher level access that hides many of the details, can optimize
access as the interfaces evolve, and ensure your application continues
working when an interface abi break occurs (rare but possible if
necessary to address a serious problem).

Security contexts
=================

Many of the low level interfaces when read return what apparmor refers to as a security context which is an apparmor label or type followed by an optional mode. The mode is always separated from the type with a space and enclosed in parenthesis ().

```
 <label> (mode)
```

The label or type (??? reference) will always follow the label spec
???reference???. And the mode will be one of

-   *enforce* - enforce profile rules, returning with access denied
-   *kill* - enforce profile rules, when possible (not all mediation is
    done in task context) killing tasks that cause an access violation
    (not recommended, it is better to use the finer grained kill
    rule modifier)
-   *complain* - allow accesses that would normally result in a
    denial, but complain about them either to a specialized daemon
    or via auditing the access.
-   *unconfined* - the profile is in the unconfined state and does
    not enforce mediation
-   *mixed* - the label/type is a stack that has different modes
    being enforced by it constituent components

The special *unconfined* profile which is always in the *unconfined*
mode usually is the only profile or label that is reported without
a mode. Though it can be forced to be reported with its mode is
some cases.

Examples:

```
 unconfined
 unconfined (unconfined)
 firefox (enforce)
```

The apparmor api aa\_splitcon ???ref??? can be used to split a security
context into its label and mode components.

Trailing newline (\\n)
----------------------

Some security contexts may have a trailing newline appended. This is
done in some version of the proc interface files and for the *profiles*
file interface which contains 1 security context per line in the file.

The aa\_splitcon()???ref?? api is aware of trailing '\\n' characters
and will remove them.

/proc/\<pid\>/attr/
=================

reading /proc/<pid>/attr/ files
-------------------------------

The following apparmor interfaces can be read to obtain security context information about a task.

-   `/proc/<pid>/attr/current` - the current task context
-   `/proc/<pid>/attr/exec` - the task context to use at exec
-   `/proc/<pid>/attr/prev` - the previous task context when the task has switched hats

### Trailng newline (\\n)

The proc interface may return a security context with a trailing
newline, which was done up until the 4.14??? kernel. The trailing
newline was removed from the proc interface to ensure the output
was safe for common utilities (ps, top, pstree, ...) accessing the
proc/<pid>/attr/ interfaces, and to unify the output with that of
the socket op SO\_PEERSEC. The trailing \\n can be safely removed or
ignored if present.

The trailing newline (\\n) is still used, one per line to separate
profile context entries in the *profiles* file.

### Trailing \\000

AppArmor does not return a trailing \\000 characters on the context
strings it returns to userspace. It may write a trailing \\000 to
the buffer holding the context string in the kernel but it is not
included in the size of data returned to userspace.

writing the /proc/<pid>/attr/ files
-----------------------------------

As of 4.14 writes to the proc file interface has been deprecated in
favour of ????. This was done to better support interaction of the
shared resource when multiple LSMs are active at the same time. Write
to the /proc/<pid>/attr/ files are restricted to a task writing its
own own files. If a task tries to write to another tasks files the
write will fail.

Writing commands to the /proc/current/attr/ files must be done as a
single atomic write containing the command and all its arguments.

### multi-threaded applications

The following documentation uses the /proc/self/attr/XXX path to
describe the file write path while this works for many applications
it may not for multithreaded tasks. In the case of multithreaded
applications the thread id (tid) must be used. This is NOT the
applications process id (pid),

The write path string must be customized for each thread.

eg.

```
 fd = open(“/proc/1234/attr/current”, O_WRONLY);
```

where 1234 is the tid of the task writing to the interface.

### /proc/self/attr/current

The */proc/self/attr/current* file can be written to change the tasks
current confinement. It is used to perform both change\_profile and
change\_hat operations. The follow commands are supported. As such it
is recommended that the apparmor api??? is used in to write to the
proc interface (as note the apparmor api will use better interfaces
if they are available).

#### changehat:

```
 'changehat '<token: unsigned long>'^('<hat name>\000)+
```

eg. try hat *privsep* and if not present *privsep2*

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “changehat 1234^privsep\000privsep2”, 32); /* includes trailing \000 in size */
 if (res != 32)
     goto fail;
```

eg. return from hat

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “changehat 1234^”, 16); /* no name specifies a return */
 if (res != 16)
     goto fail;
```

#### permhat:

allows testing permissions for a change\_hat operation without actually
doing the change\_hat. It has the same format has *changehat* except
the *changehat* command word is replaced with *permhat*.

eg.

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “permhat 1234^privsep”, 21);
 if (res != 21)
     goto fail;
```

#### changeprofile:

```
 'changeprofile '<profile name>\000
```

eg.

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “changeprofile firefox”, 22);
 if (res != 22)
     goto fail;
```

Compound profile names can be specified using compound label names eg.

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “changeprofile firefox//&user_1”, 31);
 if (res != 31)
     goto fail;
```

Stacking against the current scope confinement can be achieved by
prepending the *&* character. eg.

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “changeprofile &firefox”, 23);
 if (res != 23)
     goto fail;
```

Namespace transitions can be achieved using the fully qualified
profile name eg.

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “changeprofile :ns1:unconfined”, 30);
 if (res != 30)
     goto fail;
```

#### permprofile:

Allows testing permissions for a change\_profile operation without actually doing the change\_profile

```
 'permprofile '<profile name>\000
```

eg.

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “permprofile firefox”, 20);
 if (res != 20)
     goto fail;
```

#### stack:

Allows stacking a profile or compound label without prepending the *&*
character to the name.

```
 'stack '<profile name>\000
```

eg.

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “stack firefox”, 14);
 if (res != 14)
     goto fail;
```

is equivalent to

```
 res = write(fd, “changeprofile &firefox”, 23);
```

### /proc/self/attr/exec

The */proc/self/attr/exec* file can be written to change the tasks
confinement at exec time. It is used to perform the change\_onexec
operation.

#### exec:

Allows changing the profile/label at exec time.

```
 'exec '<label name>\0
```

eg.

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “exec firefox”, 13);
 if (res != 13)
     goto fail;
```

eg. setting it to a stack

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “exec firefox//&user_1”, 22);
 if (res != 22)
     goto fail;
```

#### stack:

Convenience function allows stacking on top of the current task's
scope of confinement without specifying a leading *&* character. The
same affect can be achieved with *exec* but it may not always be
convenient to use.

eg.

```
 fd = open(“/proc/self/attr/current”, O_WRONLY);
 if (fd == -1)
     goto fail;
 res = write(fd, “stack firefox”, 14);
 if (res != 14)
     goto fail;
```

/sys/module/apparmor/parameters/
================================

The presence of the /sys/module/apparmor directory within /sys/
indicates the apparmor kernel module has been built-in to the
kernel. The files in the parameters directory can be used to obtain
and set certain configuration parameters.

The parameters directory files are NOT virtualized to the reading
task's namespace in kernel's before 4.14. In 4.14 and later the
parameter directory files have been virtualized to the viewing task's
namespace or view.

It is important to note that just because the /sys/module/apparmor
is present does not mean that apparmor is enabled. It only indicates
that apparmor support has been built into the kernel. The *enabled*
file covered below indicates whether apparmor is actually active on
the system.

CONFIG\_DEFAULT\_SECURITY
-------------------------

The kernel config DEFAULT\_SECURITY determines which LSM is chosen
at boot by default. If this config is set to apparmor then the kernel
will select the apparmor LSM by default on boot. If the config is set
to another value then apparmor can be enabled using the *security=*
kernel parameter.

*security= kernel* parameter
----------------------------

To have the kernel chose apparmor at boot when a different LSM is
chosen by default add the following to the grub kernel boot line

```
 security=apparmor
```

CONFIG\_APPARMOR\_BOOT\_PARAM\_VALUE
------------------------------------

Even if apparmor is selected as the LSM it can be disabled by default
by the APPARMOR\_BOOT\_PARAM\_VALUE which will set the default value
for the apparmor.enabled kernel parameter. If apparmor is selected as
the LSM and apparmor is disabled the kernel will default to booting
using standard unix capabilities.

enabled
-------

Reading this value determines whether apparmor is enabled in the
kernel. If the */sys/kernel/security/apparmor/policy/enabled* file is
available it can be disabled at the per namespace level to lie about
apparmor being available. Note the enabled value can not ever be set
to enabled if apparmor is not enabled in the kernel.

When read this file will return either

-   *Y* - for apparmor is enabled
-   *N* - for apparmor is disabled

This parameter can be set as part of the kernel parameters in grub
using

```
 apparmor=<value>
```

or

```
 apparmor.enabled=<value>
```

Where the value can be

Enable

-   *Y*
-   *1*

Disable

-   *N*
-   *0*

eg.

```
 apparmor=0
```

It is important to note that setting the enabled/disabled value as
a kernel parameter is not enough to enable apparmor. Apparmor must
be built into the kernel and it must be selected as the active LSM
module. ?????

lock\_policy
------------

Returns whether policy has been locked, or if it is modifiable. Can
be written to lock policy. It is a one way switch, once locked policy
can not be updated or unlocked.

mode
----

audit
-----

debug
-----

hash\_policy
------------

logsyscall
----------

unconfined
----------

audit\_header
-------------

path\_max
---------

paranoid\_load
--------------

(deprecated/removed) - if present allows turning off many of
the consistency checks the apparmor kernel module does on policy
loads. Turning off the consistency checks could speed up policy
loads a small amount, but is unsafe if policy loading can not be
trusted. Removed as policy loads opened up to user namespaces.

apparmorfs
==========

The apparmorfs sub file system provides the primary
interface for dealing with apparmor policy. It is accessed
through the securityfs file system which is usually mounted at
/sys/kernel/security. Technically some of the apparmor files are in
securityfs and others are in the special kernel mounted apparmorfs
file system which is used to virtualize the fs to the readers view.

/sys/kernel/security/apparmor/.access
-------------------------------------

The .access file provides a transactional query interface so that
userspace can make permissions queries to the kernel. It provides
the low level interface for the aa\_query\_XXX() functions. It is
recommended that the query api???ref??? is used as it does more than
just the raw query, providing caching, help building the queries,
optimizations based on supported features etc.

Transactional queries must follow the basic pattern of doing an *open,
write, read, close* for each query that is made.

```
   fd = open(aafs_access, O_RDWR);
   if (fd == -1) {
       if (errno == ENOENT)
           errno = EPROTONOSUPPORT;
       return -1;
   }
   memcpy(query, AA_QUERY_CMD_LABEL, AA_QUERY_CMD_LABEL_SIZE);
   errno = 0;
   ret = write(fd, query, size);
   if (ret != size) {
       if (ret >= 0)
           errno = EPROTO;
       /* IMPORTANT: This is the only valid error path that can have
        * errno set to ENOENT. It indicates that the subject label
        * could not be found by the kernel.
        */
       (void)close(fd);
       return -1;
   }
   ret = read(fd, buf, QUERY_LABEL_REPLY_LEN);
   saved = errno;
   (void)close(fd);
   errno = saved;
   if (ret != QUERY_LABEL_REPLY_LEN) {
       errno = EPROTO;
       return -1;
   }
```

If the apparmor kernel module supports multi-transaction queries which
is indicated by the *features/query/label/multi-transaction* file
then the pattern can be reduced to a single open and close to setup
the query interface followed by *write, read* access pairs for each
query on the opened file descriptor. Making the core access pattern

```
   memcpy(query, AA_QUERY_CMD_LABEL, AA_QUERY_CMD_LABEL_SIZE);
   errno = 0;
   ret = write(fd, query, size);
   if (ret != size) {
       if (ret >= 0)
           errno = EPROTO;
       /* IMPORTANT: This is the only valid error path that can have
        * errno set to ENOENT. It indicates that the subject label
        * could not be found by the kernel.
        */
       (void)close(fd);
       return -1;
   }
   ret = read(fd, buf, QUERY_LABEL_REPLY_LEN);
   saved = errno;
```

The *features/query/label/* directory files if available can be used
to determine which queries are supported. If the *.access* file exists
and the *features/query/label/* directory does not exist then the query
interface can be probed to find out if a given query is supported. If
a query type is not support the error

```
 EINVAL
```

will be returned.

The currently supported queries are

-   *profile* - query permissions of a profile
-   *label* - query the in scope permission of a label
-   *labelall* - query the full tree of permissions for a label. This will include permissions for child namespaces that are out of scope.
-   *data* - query name value pair data associated with profiles in a label.

### query format

????

/sys/kernel/security/apparmor/.load
-----------------------------------

This is a write only interface that allows loading of new policy. It
won't allow replacing any policy that is already loaded. The write
that does the policy load must be done as a single atomic write. It
uses the writing tasks view to determine which namespace to load to,
if the loaded policy specifies a namespace it is relative to the
tasks namespace view.

The data format of the policy is specified in ????.

/sys/kernel/security/apparmor/.replace
--------------------------------------

This is a write only interface that allows replacement of policy,
and loading of new policy. The write that does the policy load must
be done as a single atomic write.

The data format of the policy is specified in ????.

/sys/kernel/security/apparmor/.remove
-------------------------------------

This is a write only interface that allows removal of policy. The
write that does the policy load must be done as a single atomic write.

ns profile list of profiles

/sys/kernel/security/apparmor/.ns\_name
---------------------------------------

If present this file can be read to determine the name of the reading
task's policy namespace.

/sys/kernel/security/apparmor/.stacked
--------------------------------------

If present this file can be read to determine if the reading task
has a stacked &lt;???ref&gt; confinement.

The value returned will be either

-   *yes*
-   *no*.

/sys/kernel/security/apparmor/.ns\_stacked
------------------------------------------

If present this file can be read to determine if the reading task
has a stacked confinement that crosses a scope <ref ???> boundary of
a policy namespace.

The value returned will be either

-   *yes*
-   *no*.

/sys/kernel/security/apparmor/profiles
--------------------------------------

This file can be read to get a list of the currently loaded policy
from the reading task's policy view. Profiles not visible to the
current task will not be displayed nor will the current set of stacked
profile combinations. Each line lists a single profiles context (name
followed by mode). The special unconfined profile is not displayed
as its existence is implied by the namespace existing. If a child
namespace exists it will be listed along with its profiles using the
fully qualified name (:ns\_name:profile\_name).

The file does not need to be read in a single atomic read, and should
always with finish will a completed profile context line. If policy
is updated between reads there is no guarantee of consistency, so
it is recommended that the revision file (if available) is checked
before and after reading the output of the file, and the file reread
from the beginning if the revision number has changed.

The fseek) function can be used to move through the file, moving one
profile line per position change. However consistency between read and
seeks is not guaranteed if the policy revision number is changed. It
is recommend if fseek is used it only be used to rewind the file to
the beginning.

Eg. output of *profiles* file output

```
/usr/sbin/dnsmasq (complain)
/usr/sbin/dnsmasq//libvirt_leaseshelper (complain)
/usr/sbin/dovecot (complain)
/usr/sbin/identd (complain)
/usr/sbin/cups-browsed (enforce)
udm-extractor (enforce)
/usr/lib/*/mediascanner-2.0/mediascanner-extractor (enforce)
/usr/sbin/avahi-daemon (complain)
/usr/lib/snapd/snap-confine (enforce)
/usr/lib/snapd/snap-confine//mount-namespace-capture-helper (enforce)
/usr/bin/evince-thumbnailer (enforce)
/usr/bin/evince-thumbnailer//sanitized_helper (enforce)
/usr/bin/evince-previewer (enforce)
/usr/bin/evince-previewer//sanitized_helper (enforce)
/usr/bin/evince (enforce)
/usr/bin/evince//sanitized_helper (enforce)
virt-aa-helper (enforce)
:ns1:/usr/sbin/dnsmasq (complain)
:ns1:/usr/sbin/dnsmasq//libvirt_leaseshelper (complain)
:ns1:/usr/sbin/dovecot (complain)
```

/sys/kernel/security/apparmor/revision
--------------------------------------

The revision file can be used to find the revision number of
the current task's policy namespace. The revision number will be
incremented for each change (load, replace, removal) made to policy
within a namespace.

The revisions file behaves like a pipe with the current revision
available to read when the file is opened. Subsequent reads will
sleep the reading task until the next revision is available.

eg.

```
 cat /sys/kernel/security/apparmor/revision
 66
                                                sudo apparmor_parser -r /etc/apparmor.d/bin.ping
 67
```

The revision file can also be waited on using *select* or *poll*
which will return immediately if the revision file has not been read,
or wait until the next revision is available which must be cleared
with a read before *select* or *poll* will wait for the next revision.

/sys/kernel/security/apparmor/features/
---------------------------------------

The features directory is a read only tree of features that are
supported by the kernel module. It can be used to determine whether
some features like *multi-transaction* queries are available. The
libapparmor api takes advantage of the features directory to take
ensure the best interface is used in any give situation. The individual
interfaces will document which entries in the features directory have
influence on it.

/sys/kernel/security/apparmor/policy/
-------------------------------------

The policy directory provides a directory based view of the policy
that has been loaded into the kernel. It provides access to namespaces,
profiles, subprofiles, loaded policy data and more.

As of the 4.13 kernel the policy directory is virtualized (using a
special magic symlink) to the accessing takes policy namespace view.

??? how the virualization works policy/namespaces/foo/.blah becomes
policy/.blah ????

### /sys/kernel/security/apparmor/policy/.load

This is specialized version of the
*/sys/kernel/security/apparmor/.load* interface. It uses the same
data format and has the same restrictions, the only difference is
that the policy load is relative to the policy namespace the directory
represents instead of that tasks policy namespace view.

This allows a task to load policy to a given namespace without having
to modify the binary policy to specify a namespace. It is largely
useful for tasks that want to dump policy via the raw\_data interface
and load it back in later. LXD uses this interface in combination
with the raw\_data interface to provide a checkpoint and restore for
its containers.

### /sys/kernel/security/apparmor/policy/.replace

This is specialized version of the
*/sys/kernel/security/apparmor/.replace* interface. It uses the same
data format and has the same restrictions, the only difference is
that the policy load is relative to the policy namespace the directory
represents instead of that tasks policy namespace view.

This allows a task to load and replace policy to a given namespace
without having to modify the binary policy to specify a namespace. It
is largely useful for tasks that want to dump policy via the raw\_data
interface and load it back in later. LXD uses this interface in
combination with the raw\_data interface to provide a checkpoint and
restore for its containers.

### /sys/kernel/security/apparmor/policy/.remove

This is specialized version of the
*/sys/kernel/security/apparmor/.remove* interface. It uses the same
data format and has the same restrictions, the only difference is
that the policy load is relative to the policy namespace the directory
represents instead of that tasks policy namespace view.

This allows a task to unload policy from a child namespace.

### /sys/kernel/security/apparmor/policy/revision

This is specialized version of the
*/sys/kernel/security/apparmor/revision* file. It can be used to
modify policy changes for a given namespace instead of the using the
tasks policy view.

### /sys/kernel/security/apparmor/policy/enabled

If present this file provides a namespace specific view that is
equivalent to the value of */sys/module/apparmor/parameters/enabled*,
except instead of the using the task's namespace it is the value
associated with the namespace. This file allows a task from a parent
namespace to introspect and change this value for a child policy
namespace.

### /sys/kernel/security/apparmor/policy/lock\_policy

If present this file provides a namespace
specific view that is equivalent to the value of
*/sys/module/apparmor/parameters/lock\_policy*, except instead
of the using the task's namespace it is the value associated with
the namespace. This file allows a task from a parent namespace to
introspect and change this value for a child policy namespace.

### /sys/kernel/security/apparmor/policy/mode

If present this file provides a namespace
specific view that is equivalent to the value of
*/sys/module/apparmor/parameters/lock\_mode*, except instead of
the using the task's namespace it is the value associated with
the namespace. This file allows a task from a parent namespace to
introspect and change this value for a child policy namespace.

### /sys/kernel/security/apparmor/policy/audit

If present this file provides a namespace specific view that is
equivalent to the value of */sys/module/apparmor/parameters/audit*,
except instead of the using the task's namespace it is the value
associated with the namespace. This file allows a task from a parent
namespace to introspect and change this value for a child policy
namespace.

### /sys/kernel/security/apparmor/policy/debug

If present this file provides a namespace specific view that is
equivalent to the value of */sys/module/apparmor/parameters/debug*,
except instead of the using the task's namespace it is the value
associated with the namespace. This file allows a task from a parent
namespace to introspect and change this value for a child policy
namespace.

### /sys/kernel/security/apparmor/policy/hash\_policy

If present this file provides a namespace
specific view that is equivalent to the value of
*/sys/module/apparmor/parameters/hash\_policy*, except instead
of the using the task's namespace it is the value associated with
the namespace. This file allows a task from a parent namespace to
introspect and change this value for a child policy namespace.

### /sys/kernel/security/apparmor/policy/logsyscall

If present this file provides a namespace
specific view that is equivalent to the value of
*/sys/module/apparmor/parameters/logsyscall*, except instead of
the using the task's namespace it is the value associated with
the namespace. This file allows a task from a parent namespace to
introspect and change this value for a child policy namespace.

### /sys/kernel/security/apparmor/policy/audit\_header

If present this file provides a namespace
specific view that is equivalent to the value of
*/sys/module/apparmor/parameters/audit\_header*, except instead
of the using the task's namespace it is the value associated with
the namespace. This file allows a task from a parent namespace to
introspect and change this value for a child policy namespace.

### /sys/kernel/security/apparmor/policy/path\_max

If present this file provides a namespace specific view that is
equivalent to the value of */sys/module/apparmor/parameters/path\_max*,
except instead of the using the task's namespace it is the value
associated with the namespace. This file allows a task from a parent
namespace to introspect and change this value for a child policy
namespace. This file can be used to lower the path\_max buffer size
for the namespace but it can not increase the buffer size to a value
larger than what was allocated at boot.

### /sys/kernel/security/apparmor/policy/default

If present this file provides a namespace specific view that is
equivalent to the value of */sys/module/apparmor/parameters/default*,
except instead of the using the task's namespace it is the value
associated with the namespace. This file allows a task from a parent
namespace to introspect and change this value for a child policy
namespace.

### /sys/kernel/security/apparmor/policy/raw\_data/

The raw\_data files if available allow the userspace to dump the policy
that is currently loaded into the kernel. There is one file per *load*
or *replace* of policy. The file may contain multiple profiles as it
is the data of one atomic profile load set. raw\_data files that are
no longer in use will be automatically removed.

The raw\_data file names are based on the revision number of the load
that caused the creation of the file. eg.

```
 123
```

The file may be compressed in which case its name will end with the
appropriate suffix. eg.

```
 123.gz
```

Reads of the raw\_data files do not have to be atomic, the file
will remain in existence as long as the file is held open even if
the profiles referencing it are removed. The data within the file
will never be modified so reading the data out over multiple reads
is guaranteed to result in correct output. The fseek() function can
be used to move the read position within the file.

When using raw\_data files for checkpoint and restore and multiple
raw\_data files are available, all files must be saved off and then
restored in the order that they were initially loaded. This can be
figured out by using the timestamps or the revision number encoded
in the file name.

The raw\_data files are referenced by the profiles that their load
created. The profiles reference the raw\_data files via a symlink

```
 ls -l /sys/kernel/security/apparmor/policy/profiles/<profile1>/raw_data -> ../../raw_data/123
```

#### Ubuntu specific

Before the 4.13 kernel Ubuntu carried a patchset where the raw\_data
files where available from the profiles/ directory but from a single
global location.

```
 /sys/kernel/security/apparmor/policy/profiles/<profile1>/raw_data
```

was an actual data file and not a symlink. To do checkpoint and restore
under this layout, all profile directories had to be walked. The

```
 /sys/kernel/security/apparmor/policy/profiles/<profile1>/raw_hash
```

file was read and used to dedup the raw\_data file and if the raw\_data
file was unique then it could be read out and saved. The raw\_data
files needed to be sorted by file time stamp and then they could be
loaded back into the kernel in order as described above.

### /sys/kernel/security/apparmor/policy/namespaces/ - mkdir

The mkdir function can be used in the
*/sys/kernel/security/apparmor/policy/namespaces/* directory to create
a policy namespace of the given name in the creating tasks view policy
namespace. The namespace will have default parameters with its *view*
and *scope* set to it self.

```
 mkdir /sys/kernel/security/apparmor/policy/namespaces/ns_child
```

A grand child namespace can be created by traversing the namespace
tree, but the child must exist before a grand child can be created.

```
 mkdir /sys/kernel/security/apparmor/policy/namespaces/ns_child/namespaces/ns_grandchild
```

If properties like *view* or *scope* of the namespace are to be
changed from the default values, they must be changed by writing to the
appropriate namespace files before policy is loaded to the namespace.

eg. set the namespace view to the creating tasks policy name space view.

```
 | cat /sys/kernel/security/apparmor/policy/namespaces/ns_child
 ns_child
 | echo “.//” >/sys/kernel/security/apparmor/policy/namespaces/ns_child
 | cat /sys/kernel/security/apparmor/policy/namespaces/ns_child
 .//
```

### /sys/kernel/security/apparmor/policy/namespaces/ - rmdir

The rmdir function can be used to remove a policy namespace and all
its loaded profiles.

```
 rmdir /sys/kernel/security/apparmor/policy/namespaces/ns_child
```

If the ns property ??? set then the namespaces profiles must be
removed before rmdir can remove the namespace

SO\_PEERSEC
===========

The socket option SO\_PEERSEC can be used to retrieve the apparmor
security context for a socket

```
   ret = getsockopt(fd, SOL_SOCKET, SO_PEERSEC, buf, &size);
   if (ret < 0 && errno == ERANGE) {
       char *newbuf;
       newbuf = realloc(buf, size);
       if (newbuf) {
           buf = newbuf;
           memset(buf, 0, size);
           ret = getsockopt(fd, SOL_SOCKET, SO_PEERSEC, buf, &size);
       }
   }
```

The value returned is the same as that returned by the
/proc/<pid>/attr/ interfaces except without the trailing *\\n\\0*. That
is

```
 <label> (<mode>)
```

Examples:

```
 unconfined
 firefox (enforce)
```

D-Bus
=====

The apparmor security context of a peer's connection can be found
using the org.freedesktop.DBus.GetConnectionCredentials() bus method.

Example. in C++

```
  struct BusDaemon {
    static const std::string &name() {
        static std::string s = “org.freedesktop.DBus”;
        return s;
    }
    struct GetConnectionCredentials {
        typedef BusDaemon Interface;
        static const std::string &name() {
            static std::string s = “GetConnectionCredentials”;
            return s;
        }
        static const std::chrono::milliseconds default_timeout() {
            return std::chrono::seconds{1};
        }
    };
  };
  std::string get_client_apparmor_context(const [Message::Ptr](Message::Ptr) &message) {
        if (!aa_is_enabled()) {
            return “unconfined”;
        }
        auto service = core::dbus::[Service::use_service](Service::use_service)(
            impl->access_bus(), “org.freedesktop.DBus”);
        auto obj = service->object_for_path(
            ObjectPath(“/org/freedesktop/DBus”));
        core::dbus::Result<std::map<std::string,Variant>> result;
        try {
            result = obj->invoke_method_synchronously<BusDaemon::GetConnectionCredentials,std::map<std::string,Variant>>(message->sender());
        } catch (const std::exception &e) {
            fprintf(stderr, “Error getting connection credentials: %s\n”, e.what());
            return std::string();
        }
        if (result.is_error()) {
            fprintf(stderr, “Error getting connection credentials: %s\n”, result.error().print().c_str());
            return std::string();
        }
        const auto& creds = result.value();
        auto it = creds.find(“LinuxSecurityLabel”);
        if (it == creds.end()) {
            fprintf(stderr, “Connection credentials don't include security label”);
            return std::string();
        }
        std::vector<int8_t> label;
        try {
            label = it->second.as<std::vector<int8_t>>();
        } catch (const std::exception &e) {
            fprintf(stderr, “Could not convert security label to byte array”);
            return std::string();
        }
        return std::string(aa_splitcon(reinterpret_cast<char*>(&label[0]), nullptr));
    }
```

X
=

todo
