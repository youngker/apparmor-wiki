WARNING
=======

This document is a work in progress and requires at a minimum the
development version of apparmor 4.x???

Requirements
============

-   AppArmor Kernel module - 4.x??
-   AppArmor Userspace - 4.x???

Related Documentation
=====================

-   [Stacking profiles in AppArmor](AppArmorStacking)
-   [AppArmor Policy Namespaces](AppArmorNamespaces)
-   [Confining Users with Stacking](StackingConfiningUsers)
-   [Applying system wide restrictions in AppArmor](AppArmorSystemWideRestrictions)
-   [User defined policy in AppArmor](AppArmorUserDefinedPolicy)
-   [Programatic application policy in AppArmor](AppArmorProgramaticApplicationPolicy)
-   [Putting it all together](AppArmorStackingAndNSFullPolicy)

Introduction
============

AppArmor [stacking](AppArmorStacking) and [Policy
Namespaces](AppArmorNamespaces) provides the ability to apply system
wide restrictions, while continuing to apply other policy to tasks
and users.

There are two variants of deploying system wide restrictions dependent
on whether or how the restriction should be visible.

Early policy
============

Visible system wide restrictions
================================

The apparmor initial confinement is set to be a stack of

```
default//&unconfined.
```

where default can be any profile name that is desired. When the system
starts up the default profile will be in a special unconfined state
until the profile for default is replaced. Policy will then begin to
be enforced.

Example 1: Single global confinement profile
--------------------------------------------

The basic global confinement profile is fairly simple to set-up. Generally it is treated as a white list and the set of restrictions is listed as deny rule.

```
 profile global flags=(default-allow) {
    deny w /etc/passwd,
 }
```

The profile flag is just a short hand for the set of apparmor rules
required to allow everything and an inherit (ix) transition for execs.

```
 profie global {
   allow file,
   allow network,
   ...
   ix /**,

   deny w /etc/passwd,
 }
```

The unconfined profile in the stack will proceed to behave as if
AppArmor started normally and transition to other profiles where
specified by policy.

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
