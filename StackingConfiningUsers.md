WARNING
=======

This document is a work in progress and requires at a minimum the
development version of apparmor 3.5

Requirements
============

-   AppArmor Kernel module - 3.5-beta1 (Ubuntu 16.04)
-   AppArmor Userspace - 2.11-beta (2.10.95, which was a beta release of 2.11 that is sufficient for stacking, shipped in Ubuntu 16.04)
-   pam\_apparmor pam plug-in

Related Documentation
=====================

-   [Example of how to confining users with pam\_apparmor](Pam_apparmor_example)

-   [Stacking profiles in AppArmor](AppArmorStacking)
-   [AppArmor Policy Namespaces](AppArmorNamespaces)
-   [Confining Users with Stacking](StackingConfiningUsers)
-   [Applying system wide restrictions in AppArmor](AppArmorSystemWideRestrictions)
-   [User defined policy in AppArmor](AppArmorUserDefinedPolicy)
-   [Programatic application policy in AppArmor](AppArmorProgramaticApplicationPolicy)
-   [Putting it all together](AppArmorStackingAndNSFullPolicy)

Introduction
============

The addition of [stacking](AppArmorStacking) to AppArmor opens up
the ability to provide user restrictions at the same time as applying
application confinement. Previously AppArmor was limited to applying
a single profile when confining a user, either the user profile had
to confine all applications run by the user or for some applications
the user confinement were dropped in favor of the application profile
confinement.

Stacking allows both restrictions to be applied simultaneously with
few changes to policy.

Stacking provides a few of ways of structuring policy to achieve
simultaneous user and applications confinement in policy. Each method
has its advantages and disadvantages.

**TODO** restructure so recommended methods are first, other options
are an annotation of extra information.

1. Using pam\_apparmor
======================

The pam\_apparmor module provides a way to confine users instead
of applications. With minor modifications to policy designed for
pam\_apparmor stacking can be used to achieve both confinement of
the user and application confinement. The recommend method requires
modifying the pam\_apparmor glue policy and the user role profiles
but does not modify the application profiles.

Since adding policy namespaces complicate the set up, they will be
discussed in section 3??? after the basics are established.

### Limitations

This method

-   exposes the user confinement to the user via any label query; e.g.:
    ```
    confined_user//&firefox
    ```
-   for programs that don't have an application profile the user
    exposed to stacking with the unconfined profile in its label; e.g.:
    ```
    confined\_user//&unconfined
    ```
-   if ignore rules are not supported requires the user confinement
    profile to allow any application directed transitions (change\_hat,
    change\_profile, ..) that the application may need.

### Modify the role/user profile

The role or user profile needs to be modified to just use ix profile
transitions, this prevents user application from having any path to
escape the user confinement restrictions; e.g.:

```
 /bin/** Px,
```

would become

```
 /bin/** ix,
```

The modified role profiles for the example would look like

```
 #
 # This file contains the roles as referenced by pam/mappings
 #
 #include <tunables/global>
 # By default, allow users to read, lock and link to their own files anywhere,
 # but only write to files in their home directory. Only allow limited execution
 # of files.
 profile default_user {
    #include <abstractions/base>
    #include <abstractions/bash>
    #include <abstractions/consoles>
    #include <abstractions/nameservice>
    deny capability sys_ptrace,
    owner /** rkl,
    @{PROC}/** r,
 # old rules that have been modified to ix versions
 #     /bin/**  Pixmr,
 #     /usr/bin/** Pixmr,
     /bin/** ixmr,
     /usr/bin/** ixmr,
 
    owner @{HOMEDIRS}/ w,
    owner @{HOMEDIRS}/** w,
 }
 # Allow confined_users to read, write, lock and link to their own files
 # anywhere, and execute from some places.
 profile confined_user {
    #include <abstractions/base>
    #include <abstractions/bash>
    #include <abstractions/consoles>
    #include <abstractions/nameservice>
    deny capability sys_ptrace,
    owner /** rwkl,
    @{PROC}/** r,
 # old rules that have been modified to ix versions
 #   /bin/**  Pixmr,
 #   /usr/bin/** Pixmr,
    /bin/**  ixmr,
    /usr/bin/** ixmr,
    owner @{HOMEDIRS}/bin/** ixmr,
 }
```

### Modifying /etc/apparmor.d/pam/mappings

```
 # gray is a confined user. The hat contains only the permissions necessary
 # to transition to gray's login shell. All other permissions have been
 # moved into the confined_user profile.
 ^gray {
   #include <abstractions/authentication>
   #include <abstractions/nameservice>
   capability dac_override,
   capability setgid,
   capability setuid,
   /etc/default/su r,
   /etc/environment r,
   @{HOMEDIRS}/.xauth* w,
 # old transition
 #  /bin/{,b,d,rb}ash Px -> confined_user,
 #  /bin/{c,k,tc}sh Px -> confined_user,
   /** Px -> confined_user//&unconfined,
 }
```

Now the user will begin a session with the confinement of

```
 confined_user//&unconfined
```

when an application is executed, the confined\_user profile
transitions to itself while the unconfined profile with transition
to the application profile if present, and fall back to transitioning
to itself if there is no matching profile present.

For example, when executing firefox, which has a profile defined

```
 confined_user//&unconfined
```

transitions to

```
 confined_user//&firefox
```

but when executing bash, which has no profile defined the confinement
remains

```
 confined_user//&unconfined
```

Using Policy namespaces to improve user confinement
===================================================

The methods presented so far each have a few limitations. The
limitations for application directed transitions (change\_hat,
change\_profile, ...) and user visibility of stacking can be addressed
by leveraging apparmor policy namespaces.

Namespace Ordering
------------------

AppArmor policy namespaces provide a couple of useful behaviors

-   In a stacking situation, when a task is confined by profiles
    from multiple policy namespaces. The confined task can only
    see the confinement from its current namespace and any children
    namespaces. This allows hiding of the application confinement or
    the user confinement for tasks.

-   Application directed transitions are applied only to the current
    namespace, meaning profiles from another namespace do not have
    to deal with the transitions in policy.

To use policy namespaces in user confinement stacking the policy
is split into two namespaces, a user confinement namespace, and an
application policy namespace. There are two options for the ordering
of these namespaces, either

-   user confinement namespace is the parent of the application namespace
    -   user see application confinement, not user confinement
    -   works with application directed transitions
-   application namespace is the parent of the user confinement namespace
    -   user only sees the role/user profile, not application confinement
    -   does not work with application directed transitions, as they
        are applied against the user confinement profile instead of
        the application profile.

As such for most situations the user confinement namespace should be
set up as the parent of the application namespace.

Setting up ???
--------------

The user profiles can be loaded into the root namespace, and a policy
namespace (:application:) is created for the application profiles. The
application profiles are then loaded into the policy namespace.

The pam\_apparmor transitions are setup as in method one. That is user
confinement profiles are modified to use ix for transitions, and the
pam apparmor mapping profiles are modified to do the stacking. However
the stacking for the application profile includes the policy namespace
name.

Example:

```
 Px /** -> confined_user//&:application://unconfined
```

This results in applications being confined by the stack, but unlike
in Method 1 the user doesn't see the confined\_user profile, the user
only sees

```
 unconfined
```

When the user executes an application with a profile (eg firefox)
the user will see the label as

```
 firefox
```

but if the application label is inspected by task confined only by
the root namespace will see the full confinement stack of

```
 confined_user//&:application://firefox
```

or in the case of the user application being unconfined

```
 confined_user//&:application://unconfined
```

Applications that leverage application directed transitions will only
affect the application profile so the user confinement profile does
not need to include hats or change\_profile rules that an application
may need.

Multiple application namespaces
-------------------------------

If different versions of application profiles are desired, multiple
application namespaces can be setup, and users can be stacked to the
application namespace that makes sense.

Note: communication between tasks in different parallel namespaces
is restricted so care must be taken when using them.

Limitations
-----------

-   currently it is not possible for tasks in the application namespace to view or manage user confinement.
-   applications in the root/user confinement namespace will not attach to the profiles in the application namespace.
-   pattern matching transitions can not use the pattern match to transition to a profile in the application namespace.

Working around the Limitations
------------------------------

Currently using policy namespaces imposes its own set of
limitations. However with a little care they can be worked around.

### Managing the root/user confinement namespace

To be able to load/manage user confinement policy some tasks/user need
to not be transition to a stack with the application namespace. The
admin user can be left in the root/user confinement namespace and
manage policy in both the root/user confinement namespace and the
application namespace. Admin processes will still be confined by
profiles in the root/user confinement namespace, so root/admin
processes do not have to even be unconfined.

### Confining processes in the root/user confinement namespace

Processes that are not transitions to a stack with the application
namespace will not have the application profiles applied against
them. If the application profiles should be applied against processes
in the root/user confinement namespace then a copy of those profiles
can be loaded into the root/user confinement namespace. The set of
application profiles loaded into the root/user confinement namespace
does not need to be the same set or even the same version of a
profile. This allows the root namespace application confinement to
differ from the application confinement being applied to users.

Example:

> The firefox for root could be different than the firefox profile in :application:

The root version of a profile will not be applied to users confined
by the application namespace because the confined\_user profile does
ix transitions, thus never transitioning that portion of the stack
to the application profiles in root.

Note: There is currently no means to clone profiles between different
namespaces, so even if the profile is the same in each namespace it
needs to be loaded separately.

### pattern matching and stacking

Currently there is a limitation that the patten match name can not
be applied to a profile in a subnamespace. That is in the transition

```
 Px /** -> XXX//&:subns://YYY
```

there is no way to specify a YYY based on what was matched by
/\*\*. XXX in the current namespace can be specified to be the value
of /\*\* by specifying the transition like

```
 Px /** -> &:subns://YYY
```

However this transition form does not work for the way we need to
construct the application namespace. This means a specific profile in
the namespace must be targeted. This is why the transition initially
provided transitions to :application://unconfined. So that the users
session starts unconfined in the application namespace (but still
confined by user confinement restrictions), and the application
profiles are attached as the user executes them.

If a specific profile other than unconfined in the application
namespace is required, that can be specified instead. Multiple
transition rules can be used, and specific matches can be mixed with
a single generic match.

Examples:

```
 Px /** -> confined_user//&:application://unconfined            # generic transition
 Px /usr/bin/firefox -> confined_user//&:application://firefox  # specific transition
```

Using the user attachment conditional
========================================

**NOTE**: not yet available

The [user attachment conditional](userconditional) provides several
possibilities to improve user confinement. The user attachment
conditional allows for confining users without pam\_apparmor and
allows for different user variants of an application profile to exist
in the same namespace.

allows multiple version of profile for different users based on the
conditional (don't need a separate namespace) use without namespaces
- still has visibility and change\_profile limitations use with
namespaces can be used to avoid requiring pam\_apparmor

To confine a user based on the user conditional a profile with a
generic attachment, and the user conditional set to the user that
the profile should apply to. The profile then contanins the stacking
rule to allow transition to an application profile while stacking
with the current profile.

```
 profile confined_user /** user=gray {

    Pix /** -> &@{profile},
 }
```

Note: same limitation as method2

Now when the first application for a given user is executed it will
be confined by user profile and subsequent execs are covered by the
stack if it applies.

If the very first attachment requires a stack then this can be done
through pam\_apparmor rules, or by a global profile.

Examination of alternate policy configuration methods
========================================================

The above discussion covers the recommended way to setup AppArmor
policy when using both application and user confinement. The
flexibility of AppArmor policy allows for alternate methods for
achieving this style of confinement. Below some variations are
documented along with the reasons they are not the recommended method
of achieving confinement.

### using stacked transitions to remove the stacked unconfined profile

The stacked unconfined profile

```
 confined_user//&unconfined
```

looks a little weird and it can be desirable to remove it, and this
is possible to do using stacking profile transitions. Using stacked
profile transitions we can achieve

```
 confined_user
```

instead of the recommended

```
 confined_user//&unconfined
```

The exec transitions of the user/role profile are modified to have a form of either

```
 Pix /** -> &@{profile},
```

or

```
 Pix /** -> target//&@{profile}
```

dependent on whether a `-> target` transition was used in
the original rule. The ix portion of the rule is required so that
applications that do not have a profile defined will fall back to
inheriting the current confinement of confined\_user.

The user will begin a session with the confinement of

```
 confined_user
```

when an application is executed, the confined\_user profile transitions
to the matching profile and stacks itself @{profile} on top. Resulting
in final transition with the correct stack.

For example, when executing firefox, which has a profile defined

```
 confined_user
```

transitions to

```
 confined_user//&firefox
```

but when executing bash, which has no profile defined the confinement
remains

```
 confined_user
```

because the Pix transition falls back to inheriting the current
confined\_user profile and confined\_user stacked with itself is just
the confined user\_profile.

Unfortunately this method has a major limitation around application
profile exec transitions. If the application makes an exec transition
to a program with a globally defined profile and the application
profile defines a transition for that exec to a different profile,
the resultant confinement will be wrong. The reason for this is that
the user confinement profile will also make a transitions to another
application profile.

Lets look at an example say we have application profiles

```
 firefox
 evince
 firefox//evince
```

and the firefox profile specifies a transition to a custom evince
profile firefox//evince.

The user starts the session in the profile

```
 confined_user
```

and when starting evince will have a confinement of

```
 confined_user//&evince
```

and when starting firefox will have a confinement of

```
 confined_user//&firefox
```

however when firefox starts evince the confinement becomes

```
 confined_user//&evince//&firefox//evince
```

Notice how the confinement now includes both the evince and the
firefox//evince profiles. This is because the confined\_user profile
does the stacking transition

```
 confined_user -> confined_user//&evince
```

while the firefox profile does the transition

```
 firefox -> firefox//evince
```

the final confinement stack is thus the combination of those two
separate transition, confined\_user//&evince//&firefox//evince

#### Fixing the transition problem

It is possible to fix the transition problem described above, however
the solution has its own problems.??? It really isn't at least not yet.

The confined\_user profile is duplicated into confined\_user\_profile1
and confined\_user\_profile2. The difference between the two profiles
is in their domain transitions.

confined\_user\_profile1 uses a transition of

```
 Px /** -> &confined_user2,
```

which will transition to the correct stack of
confined\_user2//&application\_profile

```
 confined_user_profile2
```

however Pix can not be used as that would result in

```
 confined_user1//&confined_user2
```

however currently apparmor does not have a means of specifying that
if the targe

#### Limitations

This method

-   exposes the user confinement to the user via any label query; e.g.
    ```
confined\_user//&firefox
    ```

-   if ignore rules are not supported requires the user confinement
    profile to allow any application directed transitions (change\_hat,
    change\_profile, ..) that the application may need.

-   results in odd even bad confinement when the application profile
    contains none Px domain transitions (ie. Cx, ix, ux, and even
    px -\>).

