# WARNING

This document is a work in progress and requires at a minimum the
development version of apparmor 4.x???

# Requirements

-   AppArmor Kernel module - 4.x??
-   AppArmor Userspace - 4.x???

# Related Documentation

-   [Stacking profiles in AppArmor](AppArmorStacking)
-   [AppArmor Policy Namespaces](AppArmorNamespaces)
-   [Confining Users with Stacking](StackingConfiningUsers)
-   Applying system wide restrictions in AppArmor
    -   Recommended Method to enforce system wide restriction (This Document)
    -   [Application White Listing](ApplicationWhiteListing)
    -   [Alternative methods to enforce system wide restrictions](AlternativeMethodsforSystemWideRestrictions)
-   [User defined policy in AppArmor](AppArmorUserDefinedPolicy)
-   [Programatic application policy in AppArmor](AppArmorProgramaticApplicationPolicy)
-   [Putting it all together](AppArmorStackingAndNSFullPolicy)

# Introduction

??? This is a details document on how to implement, need to make this easier by having some tooling and conventions, have a simple how to use document.
???

AppArmor [stacking](AppArmorStacking) and [Policy
Namespaces](AppArmorNamespaces) provides the ability to apply system
wide restrictions, while continuing to apply other policy to tasks
and users.

There are several possible ways to deploy system wide restrictions each with their own advantages and disadvantages. This document covers the recommend method of implementing system wide restrictions.

# The Basics

The basic global confinement profile is fairly simple to set-up. 

```
 profile global {
   allow file,
   allow network,
   # ... everything else to allow by default
   ix /**,

   deny w /etc/passwd,
 }
```

This profile is loaded early in boot and applied to every process on the system.


# Early policy

For global confinement to be truly system wide it has to be loaded very early in boot and init must attach to it. This means that a global confinement policy needs to be built into the initrd/initramfs and the init system must have apparmor support built in.

## Late policy load requires an alternate approach.

It is possible to provide a mostly system wide policy by specifying an attachment.

```
profile global /** {
   #...
}
```

This profile will attach to every application executed after the profile is loaded. If the profile is loaded before init re-execs itself it will attach to the new init. However if loaded later or after init this profile will not be applied to tasks that are already running.

This may be sufficient as init and early boot may not need to be confined by the global policy. This approach is also incompatible with the recommended method of applying system wide restrictions, and requires profile transitions to be accounted for in the global profile. For more information on this method please see [alternative methods to enforce system wide restrictions](AlternativeMethodsforSystemWideRestrictions).


# Working with system policy

The basic profile presented above will not allow for regular application policy to be used (the system has a single profile). We can fix this by using setting up a profile stack between the global profile and unconfined in init.

```global//&unconfined```

The global profile remains the same as above

```
profile global {
   allow file,
   allow network,
   # ... everything else to allow by default
   ix /**,

   deny w /etc/passwd,
}
```

The unconfined profile will take care of transitions to application profiles that are defined.

When introspecting a tasks confinement, this will look like

```
$ ps -Z
LABEL                             PID TTY          TIME CMD
firefox//&global                 9724 pts/3    00:03:17 firefox
evince//&global                 11898 pts/3    00:01:44 evince
global//&unconfined             17167 pts/3    00:00:02 bash
global//&unconfined             21420 pts/3    00:00:00 ps
```

# Application white listing

A global profile can be used to implement application white listing. Instead of just allowing any file to be run under the global profile. The single exec rule is replaced with a list of files that are allowed to be executed.

```
profile global {
   include <global>

   allow ix /bin/bash,
   allow ix /bin/ls,
   allow ix /usr/bin/**,
}
```

Files that do not have a matching exec rule will not be allowed to be executed even if allowed by ```unconfined``` or by an application profile.

# Hiding System wide restrictions

use namespaces

# Realities of System Wide restrictions

reality is in most cases you don't want system wide restrictions applied to all tasks. You want to split it to separate groups.

admin

user


## IMA integration

## Application whitelisting for signed binaries

AppArmor can intigrate with the IMA/EVM subsystem to provide policy restrictions based on integrity measurements. This allows for application white listing to be done based on whether the application has a valid signature.

TODO: ????

## Limitations

???


# Pain points
- early boot policy
- mount namespacing, disconnected paths ...
- special casing of applications
  - setuids/setguid
  - change_profile/change_hat  (use namespacing, need scope and view work for visibility)
  - having to enumerate all allowed mediation classes instead of a generic allow rule.
  - overlapping exec rule permissions
- interpreters
- mounts
- developing/maintaining the white list for a distro

# Dealing with mount

mount can be used to by pass global restrictions so it must be tightly controlled


# older text follows, needs to be reworked and incorportated


## Application policy escape
??? move else where to discussion about breaking up global policy into sub units with different permissions

To allow application policy to escape the global profile the exec rule is modified to use ```pix```.

```
profile global {
   #...

   allow pix /**,
}
```

??? once escaped application profile would be responsible for setting up stack again.

This will allow the application policy to be applied without the global policy profile. If Application policy desires to opt back into global confinement it needs to setup the stack again.

Eg.
```
   allow px /** -> &global,
```

Note: if [namespacing is used to hide global policy](???insert link here???) then application policy is not capable of re-establishing the stack.
Visible system wide restrictions
================================




Allowing selective applications elevated privileges
===================================================

It may be desired that the global policy restrictions apply to all
tasks except a few select applications. In this case instead of a
single global confinement profile there is a set of global confinement
profiles what work in concert. Each profile in the set enforces one set
of unique restrictions and transitions are set up within the profile
so that tasks are transitioned to the correct global restriction.

It is important to remember that task confinement has been setup
as a stack and that the transition between the global confinement
profiles does not affect what other profiles in the confinement stack
are doing and the application may still be confined by other profiles
(eg. application profile) as well.

Example 2: Set of global confinement profiles
---------------------------------------------

```
 profile global flags=(default-allow) {
   deny w /etc/passwd,
   deny w /etc/shadow,
   deny rwlk /etc/ssh/**,
   deny rwlk @{HOMES}/.ssh/**,

   px /usr/bin/passwd -> global_passwd,
   px /usr/bin/ssh -> global_ssh,
 }

 profile global_passwd flags=(default-allow) {
   deny rwlk /etc/ssh/**,
   deny rwlk @{HOMES}/.ssh/**,

   px /usr/bin/passwd -> global_passwd,
   ix /usr/bin/ssh -> global_sh,
   px /** -> global,     # will override default-allow ix /**
 }

 profile global_ssh flags=(default-allow) {
   deny w /etc/passwd,
   deny w /etc/shadow,

   px /usr/bin/passwd -> global_passwd,
   ix /usr/bin/ssh -> global_ssh,
   px /** -> global,      # will override default-allow ix /**
 }
```

while the example is incomplete and some what contrived, it provides
the template needed to setup a set of global restrictions and the
transitions between them. Common rules can of course be abstracted
out into includes and the global-allow flag unsupported by earlier
versions of appamor stacking can be recreated using a set of rules.

Using a policy namespace to hide the system wide restrictions
=============================================================

Policy namespaces provide the ability to hide the global confinement
layer of the stack from user or applications view. To do this
application profiles are placed into a policy namespace and the
initial stack is set to use the unconfined profile of the application
policy namespace.

To do this the default profile would be set to

```
 global//&:application://unconfined
```

Tasks confined by this stack would only see their confinement as

```
 unconfined
```

or if the application is attached to a profile, the application
profile name would be reported. The stack is still enforced but the
application can not see it.

Being able to view the full stack
---------------------------------

There are a problems with just setting the default profile to the
above stack.

-   The global confinement profile has to be load in the initrd before even init starts. Because once init starts it becomes confined by the stack and can not load or replace the global confinement profile.
-   All applications including init are started under the stack and the global confinement profile can not be introspected or changed.

Because the above stack makes it a requirement to manage the global
confinement profile via reboots, it is recommended that a slightly
different setup is user for most systems.

Instead of setting up the application namespace as part of the default
confinement at boot, it can be setup and loaded as part of early boot
and a subset of system tasks can be left outside of the application
namespace confinement. It may even be convenient to only put tasks
into the global confinement namespace based on the user. This would
leave root able to manage the full stack, while the user only sees
part of their confinement.

Setting up User based global resrictions
========================================

There are a couple of different ways that user based restrictions can
be setup dependent on the version of apparmor being used. The basics
of how to do this are covered in ???.

PamAppArmor
-----------

User rule conditionals
----------------------

Performance
===========

The performance of stacking and system wide restrictions is dependent
on the policy, how many layers are used and what parts of policy are
compiled together. When policy is compiled together many parts of a
stack can be resolved at compile time so that they incur no additional
overhead. However the parts of the stack that cross policy namespace
layer must be resolved at run time, even then caching and smart use
of data structures generally prevents the performance cost of adding
a new layer from being a linear increase.
