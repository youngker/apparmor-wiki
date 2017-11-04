Required Versions
=================

-   AppArmor 2.3 with 2.6.??? kernel
    -   Limitations
        -   Limited experimental flat namespace support. Only partially like documented here

<!-- -->

-   AppArmor 2.5 with 2.6.??? kernel
    -   Limitations
        -   Hierarchical namespaces, no stacking, basic creation/deletion via profile load and removal
        -   View == current namespace

<!-- -->

-   2.11 with AppArmor 3.5-beta1 - (Ubuntu 16.04)
    -   Limitations
        -   View == current namespace
        -   stacking is buggy without backported patches
        -   No mkdir/rmdir namespace interface
        -   apparmor/policy is not virtualized
        -   /sys/module/apparmor/parameters/\* are not virtualized

<!-- -->

-   2.11 with AppArmor 3.6-beta1 - (Ubuntu 16.10)
    -   Limitations
        -   View == current namespace
        -   apparmor/policy is not virtualized
        -   /sys/module/apparmor/parameters/\* are not virtualized

<!-- -->

-   AppArmor 4
    -   Limitations
        -   In development

Introduction
============

AppArmor policy namespaces allow separating AppArmor profiles into
different logical groups that provide the scope for the profile's
names, variables and attachments. In addition they provide a
mechanism for control over profile management and are the basis for
how confinement is viewed.

Policy namespaces serve 4 main purposes

-   providing alternate namespaces for profile names to aid in profile management and avoid name collisions
-   providing better control over who/what can manage policy, or specific pieces of policy
-   providing a view that defines what policy is visible
-   providing a scope for which profiles interact during mediation

A few examples

Example 1: LXD
--------------

The lxd container management system can set up a new policy namespace as part of its container configuration. The
container is free to load policy into the policy namespace without affecting system policy even when the container's
profiles have the same names as those in the system policy. The confinement scope of the policy in the container is
limited to that of the tasks within the container (ie, they're scoped to have no effect on other system tasks). For
the tasks within the container, the policy within the container looks like system policy (the actual system policy
is not even visible to these tasks).

-   The *control* is set to allow the container to manage the policy in the namespace
-   The *view* is that of the policy namespace itself
-   The *scope* is that of the policy namespace itself

Example 2: snapd confinement (planned)
--------------------------------------

The snappy system consists of applications called 'snaps' and snapd which, among other things, manages the
application sandbox for installed snaps. When AppArmor is available, snapd uses it as the main component for
confinement and generates per-snap policy for each snap's exposed commands. snapd is thus allowed to manage the
snap's Apparmor policy, but it is not allowed to manage system policy. Because snapped applications must be able to
interact with system policy in controlled ways, their policy is written with system policy in mind.

-   The *control* is set to allow snapd to manage the policy namespace
-   The *view* is set to that of the system policy namespace
-   The *scope* is set to that of the system policy namespace

Example 3: user confinement (roles)
-----------------------------------

Sometimes it is desirable to limit specific users' access to certain files and resources while at the same time
allowing users to reuse available application profiles without modification (for applications the user is allowed
to access). A policy namespace can be used to manage the user policy such that the profiles within this namespace
can coexist with system policy, therefore allowing use of application profiles where system application policy is
also visible and available.


-   The *control* is set to that of the system
-   The *view* is set to that of the system
-   The *scope* is that of the policy namespace itself so the system application policy does not need to be changed for unique user confinements

Example 4: user defined policy
------------------------------

If enabled, it is possible to allow users to define their own application policy and load it into the system.
This policy is only applied to the users' own tasks such that system policy can remain unmodified. In general, this
would also allow the user to view what system policy is being applied so that the use of user policy does not
change the regular behavior of the system.

-   The *control* is set to allow the user to manage the policy of the namespace
-   The *view* is that of the system policy namespace so that the user can see both system and user policy
-   The *scope* is that of the policy namespace itself, so that the user policy does not affect system policy

Example 5: application privilege separation
-------------------------------------------

If enabled, it is possible to allow an application to define its own policy and load it into a custom namespace.
This policy only applies to the parts of the application that the application deliberately separates. The
application is then free to choose what the privilege-separated component can view of its confinement.
The *control* is set to allow the application to manage its policy namespace

##### Option 1:

-    The *view* is set to the parent namespace so the privilege-separated components can view the confinement stack.
-    The *scope* is set to that of the parent namespace so that policy is authored from the view of the system.

##### Option 2:

-    The *view* is set to the parent namespace so the privilege-separated components can view the confinement stack.
-    The *scope* is set to that of the application namespace so the policy does not interact with system policy.

##### Option 3:

The *view* and scope are set to the application namespace so that the privilege-separated component can only
view its own confinement and its policy does not interact with the rest of the system.

##### Option 4:

For the application policy one of options 1-3 is chosen. A new sub-namespace is setup within the application namespace
with its *view* and *scope* set to the new sub-namespace. The privilege-separated part of the application is
transitioned into a stack of the application policy and *unconfined* in the sub-namespace. This blocks the
privilege-separated components of the application from even being able to view their own confinement.

It is important to understand that policy namespaces are not in and of
themselves sufficient to cover each of these situations and are meant
to be used in conjunction with [stacking](AppArmorStacking). Each of
these will be covered in depth in different sections.

Creating a policy namespace
===========================

There are three basic ways to create a policy namespace, by specifying
the namespace in policy, by forcing a profile to be loaded into
namespace, and by creating a namespace directory in apparmorfs. Each
method covers a different use case???..

Specifying the namespace as part of policy
------------------------------------------

Policy can be authored so that profiles can specify the namespace
they belong in. This is done by specifying the namespace name along
with the profile. When the profile is loaded the namespace will be
created with default values if needed.

```
:foo:/does/not/exist {
  #include <includes/base>
  ...
}

profile :foo:/does/not/exist {
  #include <includes/base>
  ...
}

:foo:unattached {
  #include <includes/base>
  ...
}

profile :foo:unattached2 {
  #include <includes/base>
  ...
}
```

This method of namespace creation is appropriate when policy from
different namespaces need to interact (see scopes). Or when the parent
namespace needs to setup a child namespace and its base policy.

Namespaces can also be created using a namespace block, which allows
declaring the namespace and setting some of its control parameters.

```
namespace foo { 
    profile one {
     ...
   }
} 
 profile :foo:two {
   ...
}
```

When using namespace blocks, their profiles do not need to be declared
within the block as long as the namespace is declared as part of
its name.

Forced loading of a profile into a namespace
--------------------------------------------

The apparmor\_parser can be used to force a profile or profiles into
a specified namespace at load time.

```
apparmor_parser -n name-of-namespace -r /path/to/profile
```

This technique is useful for when a profile needs to be replicated
into another namespace without modifications (useful for testing ..).

Creating a namespace with mkdir
-------------------------------

A namespace can be made with using the filesystem mkdir command in
the securityfs apparmor/policy/namespaces directory, which is usually
mounted at /sys/kernel/security/. Making the full path

```
 /sys/kernel/security/apparmor/policy/namespaces/
```

This will make a namespace with the specified name and default
properties, which can be changed before policy is loaded.

This method is most useful for loading binary policy blobs like what
is done for checkpoint restore functionality.

Caveats with loading policy namespaces via files:

-   you also cannot (currently) specify namespace scope or namespace view in the policy
-   you should specify only one namespace per profile file (ie, you shouldn't use \`profile :foo:...\` with \`profile :bar:...\`) for predictable policy behavior (because you can't yet specify the namespace view, the sibling namespaces can't interact)
-   you cannot specify parent and child namespaces within the same profile file

TODO:

-   using raw\_data interface??? At a different point where we can get more indepth???

Attributes of a policy namespace
================================

Each policy namespace consists of a:

-   unique set of variables
-   unique unconfined profile
-   unique default profile
-   set of profiles that can be applied
-   set of controls for who can manipulate the namespace.

name
----

Each policy namespace is named using alpha-numeric chararacters and
begins and ends with a **:** (the character set may be broadened in
the future somewhat, but the first character will always need to be
alpha-numeric). The beginning and ending colons (**:**) are technically
not part of the name (they are a prefix and separator) but the name is
always shown with the colons to disambiguate them from profile names.

```
 :namespace:
 :jail1:
```

The namespace's name when combined with a profile name is known as
the fully-qualified path. The fully-qualified path is calculated by
concatenating the profile name to the namespace name using the prefix
and suffix **:** affixed with an optional separator in between.

```
:namespace:profile_name                # ssh style (recommended)
:namespace:/profile/name               # ssh style
:namespace://profie_name               # url style ie :// as separator
:namespace:///profile/name             # notice **///** always parses as the separator (**//**) first then **/**
:namespace://profile//child            # fully qualified name of a child profile
```

The maximum length for the namespace's name is technically unlimited
but best practice says to keep them short since they often appear with
profile names as fully-qualified path and different parts of the system
have hard limits that might interfere with long fully-qualified paths.

Examples:

-   The proc interface used for introspecting tasks is limited to a PAGE\_SIZE
-   Cipso labeling for networking is limited to 40 characters
-   ...

### Why do namespace names need to begin with a *:*

When AppArmor profile names are used as an attachment, they can have
any character or pattern that a filename may have which greatly limits
how namespace names can be expressed. In addition, non-attaching
profile names put their own constraints on how the namespace's name
can be expressed. Because of these limitations, a unique leading
character is used (the *:*) to indicate the beginning of a namespace
name. The same technique is also used to indicate [instances, stacks,
delegation, and variants](AppArmorProfileSpec).

The trailing *:* separates the namespace name from the profile name
and the optional */* and *//* separators are provided as a convenience
for those familiar with ssh and protocol urls.

view
----

The view determines what other namespaces and their corresponding
policy are visible to the profiles (and the tasks that they confine)
in this namespace. This is discussed further in [AppArmor Policy
Namespace Views](AppArmorPolicyView).

scope
-----

The scope of the namespace determines which policy from other
namespaces may interact with policy in this namespace. This
is discussed further in [AppArmor Policy Namespace Scope of
Mediation](AppArmorPolicyScope).

unconfined profile
------------------

Each namespace gets its own unique unconfined profile that is used to
track tasks assigned to or created withing the namespace that should
not be confined.

default profile
---------------

Each namespace has a default profile that is assigned to tasks when
no other profile in the namespace has an attachment that matches. The
standard default profile is the unconfined profile.

Note: as of AppArmor 3.5, setting the default profile to something
other than unconfined is considered experimental. The default profile
can only be applied to the root system policy namespace via a grub
kernel parameter.

lifetime
--------

The lifetime controls whether a namespace should be automatically
removed from the system based on certain conditions:

-   *persistent* - the namespace remains loaded until it is explicitly removed from the system
-   *profiles* - if all the namespace's profiles are removed, the namespace is removed
-   *tasks* - if all tasks assigned to the namespace exit, the namespace is removed
-   *in-use* - only remove when nothing is using the namespace (eg profiles, tasks, ...)

variables
---------

TODO

controls
--------

TODO

Referencing profiles and profile transitions within a policy namespace
======================================================================

NEEDS REVIEW AND MORE EXAMPLES

-   inherit from current profile relative to current namespace: \`/foo ix,\`
-   transition to sibling profile relative to current namespace: \`/foo Px -&gt; sibling,\`
-   transition to child profile relative to current namespace: \`/foo Cx -&gt; child,\`
-   transition to profile in another namespace: \`/foo Px -&gt; :other-ns:foo,\`

Policy Namespaces are Hierarchical
==================================

AppArmor policy namespaces are arranged in a tree structure with the
system policy namespace at the root. The system policy namespace
can have children namespaces, which in turn can have their own
children. Like profiles names, namespace names can be expressed as
a path using **//** as a component separator.

Examples:

```
 :parent//child:
 :parent//child//grand_child:
```

Policy Namespace names are Relative to the View
===============================================

AppArmor policy is authored and introspected from the point of view
of a task's confinement.

When authoring policy
---------------------

When a profile specifies a profile (or label) without specifying a
namespace the reference is relative to the namespace the profile is
in. Eg, given:

```
 :ns1:A
 :ns1:B
```

with profile `:ns1:B` having the transition

```
  px /** -> A,
```

the profile `*A*` referenced is for the profile `:ns1:A`

Furthermore, when a profile specifies a profile (or label) with an
explicit namespace prefix, the profile name is then always relative
to the [namespace view](AppArmorPolicyView).

When viewing policy
-------------------

When viewing a task's confinement, the name reported will always
be relative to the [namespace view](AppArmorPolicyView). If the
viewed namespace of the target is the same as the task doing
the viewing then the namespace name will not be reported. If
the namespaces differ then visibility and view virtualization
considerations will be enforced (see [view](AppArmorPolicyView) and
[visibility](AppArmorNamespaces#Policy_Namespace_Visibility)).

### Policy Namespace Visibility

Policy namespace visibility is determined by the [namespace
view](AppArmorPolicyView) and namespace hierarchy. Parent namespaces
can view their descendants (children, grand children, ...) but the
full namespace hierarchy may not be visible to all tasks: descendant
namespaces may or may not be able to view parent or sibling namespaces
dependent on the [namespace view](AppArmorPolicyView).

Further discussion of visibility will occur in the section on
[namespace views](AppArmorPolicyView).

Profile transitions between namespaces
--------------------------------------

????
====

relative vs abs ns path

root ns is not express via a name

ns: refers to a namespace within the current root ns

When the View is not the current namespace
------------------------------------------

In AppArmor 4 it is possible to have the view be a namespace that is
different from the current namespace. When this occurs the view will
be an ancestor of the current namespace, and the view will affect
what profiles and namespaces are used when directly referenced.

For transitions and management when no namespace is specified the
current namespace is used and if a namespace is specified then it is
from the view.

`:: is the namespace of the view`

`No equiv of file path ..`

`If you want the root of the view, or ns that is not a descendent the must specify abs path for view root `

how to do relative vs abs ns path

### Simulating flat namespaces by setting the view

While AppArmor namespaces are hierarchical they can simulate a flat
set of namespaces by setting up a *root* namespace that is only used
to provide the a view for its children namespaces, no policy will
be loaded to the root namespace. A set of children namespaces are
then created under the *root* namespace and policy is loaded into
the children namespaces. Policy within a child namespace that does
not reference policy outside of the specific child namespace does
not use a namespace prefix. When a namespace prefix is used it will
reference one of sibling namespaces.

```
 TODO example
```
