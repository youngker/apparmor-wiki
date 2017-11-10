Introduction
============

The linux kernel use a ptrace\_may\_access() as a base permission check
for the ability to access certain files in the /proc filesystem. This
permission check is done in such a way that the LSM and hence apparmor
see the ptrace request without any context where it came from. The end
result is that both file rules may be needed to access a given /proc/
file, and conversely that ptrace rules can be deliberately used to
deny access to certain /proc files.

Version of AppArmor
===================

The rules need to allow control the proc file ptrace check depends
on the version of the apparmor abi that is supported.

AppArmor 2
----------

AppArmor 2 does not have direct ptrace mediation. Instead
if the profile of the tracing/reading task did not match the
tracee/readby task it would check the tracer's profile for the
capability ptrace. This unfortunately conflates profile credentials
and capability checks and was replaced in apparmor 3.

E.G.:

```
 capability ptrace,
```

AppArmor 3 with ptrace rules
----------------------------

AppArmor 3 add ptrace rules to be able to better control ptrace. The
rules take the form of

```
 'ptrace' <perms> ['peer='<label>],
```

where the permissions can be

```
 read, trace, readby, tracedby
```

The ptrace rules perform a cross check between the tracer profile
and the tracee profile. Where if the

```
 tracer {
   ptrace read label=tracee,
 }
```

then tracee needs

```
 tracee {
   ptrace readby label=tracer,
 }
```

and similarly for the trace and tracedby perms. The label= conditional
can be left off if the identity of the peer doesn't matter.

### ptrace rules for /proc

In the context of the proc filesystem, the **tracer** is the profile
of the task attempting open the **/proc** file and the **tracee**
is the profile of the task whose proc file is being read/traced.

For example, to access /proc/1234/stat a tracer with the profile P,
and task 1234 with a profile of T, will need both

```
 profile P {
   file r @{proc}/@{pid}/stat,
   ptrace read,
 }
```

and the target task 1234's profile will need

```
 profile T {
   ptrace readby peer=P,
 }
```

### ptrace and unconfined

There is an exception to the above cross check. If the tracer is
unconfined OR it is confined by a profile under the old apparmor 2
abi. Then the cross check is not done. In the case of the tracer being
unconfined no ptrace rule is needed. In the case of the tracer being
confined by a profile under the old apparmor 2 abi, the capability
check is done.

List of /proc files checking ptrace access
==========================================

The following list of /proc/ files should be considered incomplete. The
kernel is constantly evolving and new files get added and permissions
checks or conditions under which they are performed can change.

It is also important to understand that the ptrace check may not
always be performed when accessing one of the following /proc files,
some of the checks are conditional and only performed when the cred
or user, etc is different.

List of /proc files that may cause ptrace read requests
-------------------------------------------------------

```
 /proc/<pid>/cwd
 /proc/<pid>/environ
 /proc/<pid>/exec
 /proc/<pid>/io
 /proc/<pid>/map_files/
 /proc/<pid>/maps
 /proc/<pid>/ns/
 /proc/<pid>/numa_maps
 /proc/<pid>/pagemap
 /proc/<pid>/personality
 /proc/<pid>/smaps
 /proc/<pid>/root
 /proc/<pid>/stack
 /proc/<pid>/stat
 /proc/<pid>/syscall
 /proc/<pid>/task/
 /proc/<pid>/wchan
```

List of /proc files that may cause ptrace trace requests
--------------------------------------------------------

```
 /proc/<pid>/mem
 /proc/<pid>/personality
 /proc/<pid>/stack
 /proc/<pid>/syscall
 /proc/<pid>/timerslack_ns
```
