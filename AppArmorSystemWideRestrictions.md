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

For global confinement to be truly global it has to be loaded very early in boot and init must attach to it. This means that a global confinement policy needs to be built into the initrd/initramfs and the init system must have apparmor support built in.

## A slightly more flexible approach

Instead of building support for apparmor into the init system, a profile with an attachment can be used.

```
profile global /** {
   #...
}
```

This profile will attach to every application executed after the profile is loaded. If the profile is loaded before exec re-execs itself it will attach to the new init. However if loaded later or after init this profile will not be applied to tasks that are already running.

This may be sufficient as init and early boot may not need to be confined by the global policy.

# Working with system policy

The basic profile presented above will not allow for regular application policy to be used. We can fix this by changing the exec rule.

## Application policy escape

To allow application policy to escape the global profile the exec rule is modified to use ```pix```.

```
profile global /** {
   #...

   allow pix /**,
}
```

This will allow the application policy to be applied without the global policy profile. Application policy then needs to use ```px/pix``` exec transition rules, transition children tasks back to the global profile if an application profile is not defined.

## Composing with application policy

Generally when using a global policy it is because there are restrictions you want applied to everything (or almost everything), and allowing application policy to escape the restriction is not desirable. There are two ways to address this.

### composing through an include.

The application policy restrictions, without the exec rule, can be placed in an include file. That is included by the global profile as well as all the application profiles.

```
$ cat /etc/apparmor.d/abstractions/global
   allow file,
   allow network,
   # ... everything else to allow by default

   deny w /etc/passwd,
```

The global confinement profile then becomes
```
profile global /** {
   include <global>

   allow pix /**,
}
```

The application profiles can be modified to have ```include <global>```, or if modifying every application profile is not desirable the ```abstractions/base``` file is included by almost every profile, so the ```include <global> rule could be added to it.

### dynamic composition

Recent apparmor versions provide an alternate method to achieve profile composition through profile [stacking](AppArmorStacking). To use stacking the global policy exec rule is modified.

```
profile global /** {
   include <global>

   allow pix /** -> &@{profile_name},
}
```

This rule will allow application policy to be used at the same time as the ```global``` profile is being applied and when there isn't an application profile only the global profile will be applied.

When introspecting a tasks confinement, this will look like

```
$ ps -Z
LABEL                             PID TTY          TIME CMD
firefox//&global                 9724 pts/3    00:03:17 firefox
evince//&global                 11898 pts/3    00:01:44 evince
global                          17167 pts/3    00:00:02 bash
global                          21420 pts/3    00:00:00 ps
```

When tasks confined by a stack of profiles execute an application without an application profile, the stack will collapse back into the application being confined by just the global profile.


# Application white listing

A global profile can be used to implement application white listing. Instead of just allowing any file to be run under the global profile. The single exec rule is replaced with a list of files that are allowed to be executed.

```
profile global /** {
   include <global>

   allow pix /bin/bash -> &@{profile_name},
   allow pix /bin/ls -> &@{profile_name},
   allow pix /usr/bin/** -> &@{profile_name},
   ...
}
```

Files that do not have a matching exec rule will not be allowed to be executed.

## xattr tagging

As a further restriction on what can be run. Known executables can be tagged with an xattr that can be tied to apparmor policy. This can be used to restrict applications from running that are not properly tagged. For example applications downloaded to a users home directory could be restricted from running if they have not been tagged.

```
profile user_downloaded
        xattrs=(
           # TODO: RFC on tag format
           security.tagged=allowed
        )
{
  ...
}

profile global /** {
   include <global>

   allow px @{HOME}/** -> user_downloaded//&@{profile_name}
   ...
}
```

Notice there needs to be a special profile that specifies the xattr tag. This profile can be shared between all applications run under the tag or different profiles can be used. Also notice that the exec rule uses ```px``` in this case instead of ```pix``` to force execution to fail if the xattr tag does not match.


### xattr tagging with userspace prompting.

It is possible to use xattr tagging in combination with a desktop prompt to allow the user to interactively control whether files downloaded from the internet should be run.

In the previous examples apparmor will just deny the execution if the application is not tagged. To have apparmor prompt the user the profile must be modified by replacing the ```allow``` keyword with the ```prompt``` keyword.

```
profile global /** {
   include <global>

   prompt px @{HOME}/** -> user_downloaded//&@{profile_name}
   ...
}
```

This tells apparmor that if the rule fails it should send a prompt notification to userspace. This also requires that the user is running a notification daemon that will do the actual prompting and tag the file if the user decides to allow execution.

### xattr tagging for different trust levels

In apparmor xattr tagging can go beyond just the basic allow/deny decision used in the example above. It can also be used to run applications at different trust levels. Extending the above example instead of out right denying untagged files they can be run in a very restrictive profile.

```
profile trusted @{HOME}/**
        xattrs=(
           # TODO: RFC on tag format
           security.tagged=allowed
        )
{
  ...
}

profile untrusted @{HOME}/** {
  ...
}

profile global /** {
   include <global>

   allow px @{HOME}/** -> &@{profile_name}
   ...
}
```

While the trusted and untrusted profiles use the same attachment the xattr specification will make the trusted profile preferred over the untrusted profile if the xattr tag is present. Conversely if the xattr tag is not present the trusted profile will not match so the untrusted profile will be used.

## Application whitelisting for signed binaries

AppArmor can intigrate with the IMA/EVM subsystem to provide policy restrictions based on integrity measurements. This allows for application white listing to be done based on whether the application has a valid signature.

TODO: ????


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

# Dealing with mount

mount can be used to by pass global restrictions so it must be tightly controlled


# older text follows, needs to be reworked and incorportated


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
