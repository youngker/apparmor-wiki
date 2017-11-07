Required Versions
=================

-   AppArmor kernel module 5 or later
-   AppArmor userspace 5 later

Related Documentation
=====================

Introduction
============

AppArmor allows a task to Delegate some of its authority to other
applications, allowing them to perform operations or access data that
they could not normally under their confinement.

??? [labels](AppArmorLabelsandTypes)

Authority
---------

Authority is the right or permission to perform an action. An example
would be a police officer has the authority to make an arrest but
he does not have the authority to determine the sentence of the
arrested person. Authority in the real world is often associated
with an identity or position and some time authority can delegated
to another, Eg. a sheriff deputizing someone to assist him.

In AppArmor authority can be delegated to extend profiles with access
to file objects or rules that they would not normally have.

Identity
--------

In AppArmor authority and Identity are different. Authority is the
right/permission to do something while Identity is the who. I real
world terms this is like saying Bob the police man. Where Bob is who
and police man is the name of the authority, which is it self a form
of identity.

In AppArmor terms Identity is the profile name or label, and delegated
authority can be associated with the Identity.

How Delegation is Expressed
===========================

In AppArmor delegation is exposed to the label by appending the
Delegate information to the profile name (label) with character
sequence *//+*. If the permission rule set R1 is delegated to a task
confined by profile A, its confinement label will be expressed as:

```
 A//+R1
```

If multiple permission rule sets are delegated the delegation have
each component in arbitrary order

```
 A//+R1//+R2
```

The order of the Delegation is important in that the profile name
(identity) will always be first with the Delegate information following
in an implementation determined order.

While the delegation syntax will seldom be used in policy it will
show up during task introspection and in audit logs.

Names and authority
-------------------

Authority combinations can be grouped together into a common name.

```
 label police = A//+B//+C
```

Delegation applies to tasks
===========================

Delegation is a task oriented extension of permissions that in effect
makes a special “new” version of the profile that will confine
the task. That is to say delegation is a dynamic property, and it can
be lost when a service is restarted, policy is removed or the system
is rebooted.

With that said it is possible to express delegation within policy
making it permanent. When a profile specifies

```
 px /bin/foo -> +bar,
```

The delegation will always be applied as part of the exec transition,
resulting in a confinement of /bin/foo//+bar for that task.

Delegation in Policy
====================

declaring policy chunks that can be delegated
---------------------------------------------

Permissions can be grouped together into a set of authority that can
be named.

```
 authority foo {
   ...
 }
```

The authority block is just a specialized profile can specify the block
as a profile authority block can't be directly transitioned to

Delegating authority in policy
------------------------------

To delegate authority in policy the rules that are being delegated
are specified as part of a domain transition.

```
 px /foo -> bar + foo;
```

```
 px /foo -> + foo;
```

### localized authority

```
 profile A {
   authority foo {
     ...
   }

   px /foo -> + ^foo,
 }
```

#### unnamed local authority

A rule block without an associated name can be specified by doing

```
 px /foo -> bar + {
    ...
 }
```

```
 px /foo -> + {
   ...
 }
```

This will result in a localized authority with a unique local name
that is generated by the compiler.

### how localized authority appears in labels

Just as authority in a label is not distinguished from a profile, the
localized authority set is treated as a special hat or child profile,
and will appear as such in labels.

E.G. the profile for /foo given the following rules

```
 profile A {
   authority foo {
     ...
   }

   px /foo -> B+^foo,
 }
```

would be

```
 B//+A//foo
```

controlling application directed delegation
-------------------------------------------

What rules an application can delegate are controlled by rules in the profile.

```
 delegate +foo,
```

tying the delegation to an exec

```
 delegate /** -> +foo,
```

tying delegation to a target label. Hrmmm should this be used instead
of the exec tying, and then tie to the exec label.

```
 delegate ???
```

### restricting to delegation to an object

Normally delegation is passed as a rule, but delegation can be
restricted to an object that matches a delegation rule by specifying

```
 delegate object +foo,
```

Allowing a profile to receive delegation
----------------------------------------

Application Directed Delegation
===============================

Delegation is inherently an task initiated operation. Either via an
exec that has the delegation encoded in policy, or by an action at
run time such as passing an object to another task.

object delegation via fd passing
--------------------------------

rule delegation vis api
-----------------------

Making Delegation Permanent
===========================

As mentioned above it is possible to make some delegation a permanent
part of policy. However this requires being able to update the policy
for the given delegation. This is fine for some types of delegation,
but for application directed delegation granting rights to arbitrarily
edit and load policy is not desirable, in these cases AppArmor provides
a couple of alternative means that an application can grant privileges
without using delegation.

### Re-delegate

Applications/services can always store the delegation information
in a local trust store and re-delegate objects or rules again at a
later date. This will only work if the service can an identify the
application at a later date.

Using this approach is not recommended as it stores extra information
outside of regular policy making it so a regular policy audit won't
reveal the extension. It is true that this is no different than what
is allowed for by temporary delegations but since it is being stored
it would be ideally part of policy.

### object labeling

If the object being delegated is a file that is stored on disk then it
can be given a permanent label that can be used to determine permission
access. See [object labeling in AppArmor](AppArmorObjectLabeling)
for more details.

### Selective limited policy updates

AppArmor provides the ability to control who can edit and load
certain pieces of policy via fine grained controls. A [policy
chunk](AppArmorPolicyChunks) (specialized profile) can be dedicated
to storing delegation changes, and the service/trusted helper that
is doing the delegation can be given permission to edit the policy
chunk. The profile can then be made to reference the chunk, and it
will dynamically pickup the changes every time the chunk is updated
in the kernel.

- in view sub policy namespaces - references and restricted policy update

Principle of Least Authority (PoLA)
===================================

The principle of least authority (or privilege) is that a task
should not have any more privilege/permissions than it needs to do
its job. Unfortunately many applications may need very broad access
to perform their jobs sometimes. Which profiles have to be written
in a very broad fashion giving them more permissions than is required
most of the time because the application might need access to certain
resources some times. An example of this is the Firefox web browser
which may want to save files to a user's home directory. To allow for
this the profile must grant Firefox permissions to access all of the
users directory even though it doesn't need access to all the files,
nor does it even need access to any files in the directory most of
the time.

Having such broad profiles means policy is broader than it needs
to be and can allow an attacker access to information that could be
blocked most of the time.

Ideally an application profile would be written with the minimal set
of permissions required, eg. no generic access to the users data,
and selectively extended when needed. Delegation of Authority is one
of the tools available in AppArmor to craft policy in such a manner,
where an application is given a base profile and it is given additional
access when required, either via policy or trusted helpers.

Trusted helpers and Delegation
==============================

[Trusted helpers](AppArmorTrustedHelpers) are services with
elevated privileges that AppArmor trusts to mediate or even enforce
some permissions. Examples of trusted helpers are the dbus daemon
(entrusted with enforcing dbus policy), xapparmor xace plug-in for
the xserver, and the ubuntu touch media hub. Some of these trusted
helpers just interact with policy while others like the media hub
can grant access to files and services that an application would not
normally have access.

The primary means of providing this access is via delegation.

Storage of delegated perms, custom local storage vs doing it in policy

Having policy have a reference that the trusted helper can update

File dialogue

--- examples of delegating authority

advantages tighter confinement PoLA/P

user interaction and trusted helpers

delegation of authority vs identity

Static vs. Dynamic Delegation
=============================

Like static some delegation can be computed statically at compile
time, while some needs to be computed dynamically. Just what can be
computed statically will depend on the rule sets and what parts of
policy (profiles) are compiled together. Simple policy rules like

```
 px /bin/A -> B//+C,
```

can be precomputed as long as the necessary profiles are compiled
together. Object delegation and application directed delegation will
almost always have to be computed dynamically.

As with stacking static computation can result in longer compile times
and larger compiled policy but reduces the computational overhead at
run time.

Types of Delegation
===================

There are two distinct ways that authority can be delegated, either
by extending an applications policy with a set of rules, or by passing
an object handle to an application.

Rule Delegation
---------------

Object Delegation
-----------------

Policy based Delegation
-----------------------

Application Directed Delegation
-------------------------------

Inheritance of Delegation by Children
=====================================

Delegation is inherited by children as long as their AppArmor
confinement remains unchanged. When a tasks confinement is changed
via exec rules, application directed transitions or other means the
delegation is re-evaluated and my be entirely dropped, partially
dropped, or full inherited depending as specified by policy.

To determine how the delegation will be handled the original delegotor
policy is checked to determine if it allows delegating to the new
confinement.

At this time it is not possible for the original delegator to delegate
this ability to a child, but it may be added in the future if the
need arises.

Delegation across profile transition boundaries
===============================================

Delegation on objects??? may be dropped, and access still retained,
if the the target confinement contains permissions.

Delegation and Stacking
=======================

Policy Rules controlling Delegation
===================================