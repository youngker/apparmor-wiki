Understanding AppArmor
======================

### What is AppArmor?

AppArmor is an mandatory access control (MAC) like security system
for Linux. It is designed to work with standard Unix discretionary
access control (DAC) permissions while being easy to use and deploy,
by allowing an admin to confine only specific applications.

### What is mandatory access control (MAC)?

Mandatory access control is a security method where what is allowed
is explicitly defined by policy. A user or program can not do any
more than is allowed by the policy confining it.

### What is DAC?

DAC stands for discretionary access control and is the standard
Unix/Linux permission system consisting of a subject (ie owner,
group and other) and its access permission (ie, read, write, execute,
setuid, setgid and sticky).

### What is Default Deny (Allow listing)

Default deny describes the default action applied to a request that
is not listed in policy, in this case deny. Policy that is based on
default deny provides a list of actions that are allowed and any thing
not listed in policy is denied.

Default deny is considered more secure than default allow because it
only allows known good behavior.

### What is Default Allow (Block listing)

Default allow describes the default action applied to a request that
is not listed in policy, in this case allow. Policy that is based
on default allow provides a list of actions that are NOT allowed
and any thing not listed in the policy is allowed.

Default allow is considered less secure than default deny because it
only stops known bad behavior.

### Is AppArmor policy Default Deny (Allow listing)

Yes, and no. AppArmor profiles are based on default deny, that is the
rules list what is allowed and everything else is denied. AppArmor
policy allows specifying rules to deny actions but these are
convenience function to document known bad behaviors, or to specify
actions that should never be allowed even if an rule allowing it was
added to the profile.

At a system level AppArmor can be consider either default deny or
default allow depending on how it is deployed. If AppArmor is deployed
with a complete system policy so that all tasks are confined then
the deployment can be considered default deny. However AppArmor
allows policy to be deployed incrementally so that only a subset
of the system is actually confined, in this case AppArmor could be
considered to be default allow at the system policy level but anything
that is confined is confined by policy that is default deny.

### Isn't incremental deployment (ie only mediating a subset of the system) insecure?

No, or more simply, not any more so than running a Linux system
without any MAC security. AppArmor, like all other MAC security
systems in Linux, is applied after standard Unix DAC permissions
are applied. While true that confining only certain applications on
a system is not as secure as having a policy for all applications,
this is a trade off in policy versus ease of deployment that an admin
should be aware of.

### What is a Profile? Policy?

Profiles are how AppArmor defines policy. A profile is a listing
of access rules for an application. It starts with the name of the
application to be confined followed by the set of rules specifying
what it is allowed to do. A profile defines a policy which is compiled
and loaded into the kernel. The kernel enforces policy on applications.

AppArmor Policy
===============

### Where is AppArmor Policy Stored?

AppArmor system profile files and related files are traditionally
stored in the directory /etc/apparmor.d/. For more information,
see the [Policy Layout Reference](Policy_Layout).

### Why are profile files named the way they are?

By convention the filenames for AppArmor profile files have a name
related to the profile they contain. The convention used has been to
convert **/** in the pathname to a **.** and if there is a leading
**.** it is droped. For example a profile for /usr/bin/firefox would
be write in a file named usr.bin.firefox

This is just a convention, and does not have to be followed, the file
name does not have to be related to the profile it contains. For more
information, see the [Policy Layout Reference](Policy_Layout).

### Can a profile file contain multiple profiles?

Yes. The convention is to break out profiles into multiple files,
as this makes it easier for package management to add new profiles
by just adding a file. However there is no reason a profile file can
not contain multiple profiles, and this is even done in some cases
where profiles are closely related.

### Why doesn't renaming a profile file disable the profile?

The profile file name does not determine whether a profile gets
loaded. The initscripts that load the profiles will look at all
files in the /etc/apparmor.d/ directory and load all profiles that
it finds, whether or not the filename is related to the profile that
is contained within.

### How do I disable a profile

Profiles can be disabled by either adding a symlink to the profile
file in /etc/apparmor.d/disable/ directory, or by removing the profile
file from the /etc/apparmor.d/ directory.

### Does editing/removing a file in /etc/apparmor.d/ update the loaded policy

No. For the policy being enforced to be changed it must be loaded
into the kernel. The recommended way of doing this is either

```
 /etc/init.d/apparmor restart
 restart apparmor                # if upstart is being used, with initscripts that have been fully updated to support upstart
```

With that being said, if you modify a profile from logprof, or genprof,
the tool with reload the profile for you.

### Why does AppArmor not have separate permissions for file rename and file copy?

Because having a separate rename permission only gives a small amount
of extra control over just mediating file copy. That is having a
separate file rename permission encodes expected behavior of how
the data is expected to be moved, while file copy just worries about
whether a files contents can be copied. As long as reading the source
and writing to the destination are allowed rename restrictions can be
worked around simply by copying the files' contents to the destination.

In the end the trade off in extra control over complexity this feature
was considered not worth implementing at this time.

Controlling AppArmor
====================

### Can enforced policy be changed without rebooting

Yes. To do this the policy needs to be reloaded (replaced). This is
done by either

```
 /etc/init.d/apparmor restart
 restart apparmor                #if upstart is being used, with initscripts that have been full updated to support upstart
 apparmor_parser -r /etc/apparmor.d/profile.file.name    #to replace a single profile
```

### What is Profile Replacement?

Profile replacement allows for replacing a currently loaded profile
with a new version of the same profile. When this is done all tasks
using the old profile begin using the new profile for all access
decisions going forward. Doing this has several security implications
that the administrator must be aware of:

-   Tasks retain access to all resources they had open under the
    older version of the profile
-   Any children tasks (applications a task has launched) retain the
    confinement that was decided by the old version of the profile. For
    children inheriting the parent profile (ix), then new children
    tasks will use the new profile.

The most secure solution is to always restart services after a
profile has been updated, but in practice this is not always desired
or required, which is why replacement is provided despite the above
limitations.

### Can I set/change the confinement of a running task?

You can not apply brand new (ie unloaded) policy to a running
process. Policy must be loaded into the kernel before the application
starts. For processes that are already confined, you can perform
profile replacement in a controlled and limited way to change the
policy for that process.

#### Why not?

There are several reasons not to do this and a technical limitation
prevents it at this time.

-   Due to the credentials implementation, the linux kernel was changed in a way that makes this very difficult to implement. As a result, when AppArmor was rewritten to use credentials this ability was dropped.
-   Changing the confinement of a task after it is started is fraught with problems:
    -   The task may have already accessed resources that the new profile would not allow
    -   The task may have already launched applications that wouldn't be allowed under the new profile
    -   The task may have launched children that have different confinement than what the new profile would have allowed. For example, in the simplest case the task and its children are all unconfined such that all the tasks could be walked and correct confinement assigned, with the caveat that the task may have access to resources it shouldn't. In the worst case, the different operating environment caused by the confinement changed the behavior of the task so that it or its children are in a different state that is not functionally compatible with the new profile being applied to it.

#### But couldn't earlier versions of AppArmor do this?

Yes. AppArmor 2.0.1, 2.1, and 2.3 provide the ability to change
the profile on a specific task through a special set\_profile
interface. This interface was removed because setting a task's profile
is no longer possible due to the recent technical changes made in
the Linux kernel.

#### If setting a tasks confinement after the fact is so problematic, why was it offered in the first place?

This was initially implemented both because it is convenient
and because imperfect confinement may be better than no
confinement. Sometimes it is not possible or desired to shutdown a
service and restart it when you would like to mitigate a possible
breach of security.

### Why isn't profile removal and then load equivalent to replacement

Because removing a profile removes the confinement information for
the task making the task unconfined. The following profile load will
then load the profiles but will not be able to attach them to already
running unconfined tasks.

Resolving Problems
==================

### How do I see if AppArmor is causing a problem on my system?

Please see the section on [AppArmor Failures](AppArmor_Failures).

### I have an application that is unconfined but I have a profile for it. What can I do?

You must load the policy into the kernel, then restart your
application. If an application is started before a profile is loaded
or if a profile is removed after an application is started the
application will be unconfined. You may not change the confinement
of a running application by applying a new profile to an unconfined
application or change the profile on a currently running application
(See Controlling AppArmor:Can I set/change the confinement of a
running task for further information).

### I have an application that is not confined and aa-status does not report this correctly

A known limitation of aa-status is it will not report when an
application for which a profile is loaded is running unconfined if
the profile uses path name globbing to specify the application to
be confined.

### Does specifying directory write access (eg “/foo/ w”) allow unlinking or renaming of files in the directory?

No, write permission on a directory does not imply permission to
write to or create files in that directory. This type of access
only gives write access to the directory itself, which means you
are allowed to change the directory meta information (chown, chmod,
..) and create/remove the directory.

### What causes the error “Profile doesn't conform to protocol”

This is caused by a mismatch between what the policy is compiled into
and what the kernel expects. Generally speaking this is caused by a bug
in either the apparmor\_parser (parser is generating bad policy) of the
kernel module (kernel is not accepting valid policy). Historically this
bug has been caused by compiling the apparmor\_parser against newer
kernel headers than the currently running kernel. This is because the
apparmor\_parser compile extracts information from the header about
what capabilities, network protocols, etc. Occasionally kernel header
changes will tease out a bug, that prevents the apparmor\_parser from
being able to generate correct policy for older kernels.

Generally the quick fix is recompiling the apparmor\_parser against
the current kernel headers.

### What causes “Failed name lookup” and how do I fix it

Failed name lookups are caused when AppArmor tries to resolve the
path name of a previously opened file object and fails. There are
three distinct reasons this can happen.

#### Failed name lookup - deleted entry

The file has been marked for deletion and no longer has a valid name
(for the referencing object) in the file system. When this happens
AppArmor will do one of two things.

-   Implicitly delegate access to the file if there are no hard links
    (alternate names) to the file. This case will not cause Failed
    name lookup messages.
-   Attempt to revalidate by doing a name lookup which fails. For
    deleted files AppArmor can mediate the file object via the
    pathname it would have had by passing the mediated\_deleted flag
    on the profile.

```
 /example flags=(mediate_deleted) {
   # ...
 }
```

#### Failed name lookup - disconnected path

lazily unmounted device path opened outside of current namespace

#### Failed name lookup - name too long

AppArmor has a maximum pathname size that can cause pathname lookups
to fail. If this is a problem the value can be adjusted by increasing
the cut off value. Eg.

```
 echo 8096 > /sys/module/apparmor/parameters/path_max
```

Deploying AppArmor
==================

AppArmor Policy
===============

### Does directory writing (“/foo/ w”) allow unlinking or renaming of files in the directory?

No, write permission on a directory does not imply permission to
write to or create files in a directory. This type of access only
gives write access to the directory itself, which means you are
allowed to change the directory meta information (chown, chmod, ..),
and create/remove the directory.

AppArmor & other LSMs
=====================

### Can AppArmor be used simultaneously (stacked) with SELinux or other LSMs?

No, the LSM does not currently allow for generic stacking. While
technically possible to stack two LSMs, both LSMs must
cooperate. AppArmor, SELinux, Smack, and TOMOYO all make use of
security structures that make cooperatively stacking difficult without
modification.

### Can an AppArmor profile be automatically converted to SELinux policy or vice versa?

No, there is currently no tool to do this nor is it generally
possible. While AppArmor profiles and SELinux policies share a
lot in common and can be used as a basis for each other, there are
semantic differences that are not easily converted. Some examples of
differences are:

-   AppArmor encodes certain expected behavior semantics in its profiles (eg 'm' versus the different forms of 'x')
-   AppArmor and SELinux handle domain transitions differently
-   AppArmor and SELinux handle user space directed domain transitions (in AppArmor change\_hat(), change\_profile(), change\_onexec()) differently
-   While AppArmor and SELinux both use the LSM, they use some of the hooks differently which can result in semantic policy differences
-   SELinux policy tends to consider the system as a whole while AppArmor policy is generally application by application, so any conversion would need to deal with differences caused by this view of the system.

While it is possible to create a tool that would automatically convert
AppArmor profiles and SELinux policy, the converted policy would be
incomplete which is why this tool has not been created.
