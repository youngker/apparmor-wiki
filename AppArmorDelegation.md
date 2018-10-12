# Warning this document and feature are a WIP

Required Versions
=================

-   AppArmor kernel module 4 or later
-   AppArmor userspace 4 later

Related Documentation
=====================

todo

# Availability of Delegation
The following table identifies which version of AppArmor different types of delegation are available in.


| ** Delegation** | Policy Directed | Application Directed |
|--------------|-----------------|----------------------|
| object based |        ?        |          ?           |
| rule based   |        ?        |          ?           |


# Introduction
AppArmor 4 extends AppArmor to be a hybrid of [Domain Type Enforcement (DTE)](http://citeseer.ist.psu.edu/viewdoc/summary?doi=10.1.1.37.1501) and a [capability system](https://en.wikipedia.org/wiki/Capability-based_security). This is achieved by allowing a profile or task to delegate some of its [authority](AppArmorDelegation#authority) to other applications, allowing them to perform operations or access data that they could not normally do or access under their confinement.

Delegation of Authority helps with authoring policy that adheres to the [principle of Least authority](AppArmorDelegation#principle-of-least-authority-pola). Which means policy can be tighter and then expanded to allow access to data as needed. Delegation can also help avoiding problems like the [confused deputy](https://en.wikipedia.org/wiki/Confused_deputy_problem)


# Delegation in AppArmor

In AppArmor delegation is always temporary as it based on passing [authority](AppArmorDelegation#authority) to a task and unless [inheritance](AppArmorDelegation#inheritance) is defined will only lasts the life time the task. 


## Authority
Authority is the right/permission to do something, This is expressed by the set of rules often in a profile but could be a special block of rules.


## Identity
In AppArmor identity is the name associated with a set of rules. This is often a profile name but could be a random name associated with a set of rule. When a task has multiple identities assigned to it, all identities are equal.

In real world term this is like saying a person Bob is also a police man. The individual is both of those things and has the rights and responsibilities of both Bob and a police man.

In AppArmor terms this is exposed in the label by listing the profile names that contribute to the task's authority.

  Bob//+police_man

Identity is also used in policy to control which authority is or can be delegated.



## Aspects of delegation

It is important to understand that delegation in AppArmor has multiple aspects to it.



## Object or Rule
One aspect is whether the delegation is happening at the object or rule level.

* object based - when an object (file handle, socket, ...) is delegated between tasks.
* rule based - when rules are used to extend what a task can do

Object delegation allows for live objects to be passed without giving the receiving task privileges beyond access to the already opened object. Rule based delegation extends a tasks confinement so that it has increased authority and can create new objects its old confinement wouldn't allow.

Object delegation happens through three mechanisms
- fd inheritance
- fd passing
- application directing delegation of the fd

Object delegation and rule delegation are often combined to provide for [inheritance](AppArmorDelegation#inheritance) of delegated objects.


## Policy directed or Application directed
* Policy directed - the delegation is specified by rules in policy
* Application directed - the application takes action to delegate some authority. The ability to do this is it self mediated by policy.


## Inheritance

Inheritance only applies to task and is always temporary

Delegation is across profile boundaries

Inheritance of delegation is not based only based on a tasks parent child hierachy. Instead, all inheritance, whether object or rule based, is controlled by a set of rules that is it self part of the delegation.

Without inheritance rules the delegated objects and rules can not cross a profile boundary.


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

The authority block is just a specialized profile that can't be directly transitioned to by an exec transition or the change_profile/change_hat apis. In fact profiles can be used as an authority block and be delegated to another profile.


## Delegating rules at profile transition in policy

Rules can be delegated as part of a domain transition.

```
 # transition to profile bar with the added rules in foo
 px /foo -> bar + foo;
```


```
 # transition to the profile that attaches to /foo with the added rules in foo
 px /foo -> + foo;
```

### localized authority

Authority blocks can be localized to a profile just like child profiles.

```
 profile A {
   authority foo {
     ...
   }

   px /foo -> + foo,
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

E.g. the confinement of /foo when started from the confinement of profile A with the following rules

```
 profile A {
   authority foo {
     ...
   }

   px /foo -> B + foo,
 }
```

would be

```
 B//+A//foo

B is the profile transitioned to and A//foo is the name of the local authority block.
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

# Delegation and Domain transitions

When an exec transition or change_profile happens causing a profile transition. The tasks confinement and delegation is re-evaluated.

First each component of the task confinement is evaluated for transitions, this is done for the profile as well as the delegated rules.  ??? do the delegated rule objects replace themselves if they have exec rules or the profile ???

After the transition evaluation the delegated rules are checked for inheritance.


# delegtion and stacking

confinement is always expressed in conjunctive normal form

delegating to a stack
(A//&B)//+C is transformed into (A//+C)//&(B//+C)



Application Directed Delegation
===============================

Delegation is inherently an task initiated operation. Either via an
exec that has the delegation encoded in policy, or by an action at
run time such as passing an object to another task.

object delegation via fd passing
--------------------------------

rule delegation vis api
-----------------------

Applications can delegate rules instead of just objects. Rule delegation requires access to the rule delegation api (which requires access to certain files and the ability to load policy in certain places), and there is some overhead associated with it.

Rules being delegated go through a compile to transform the rules into a form that can be used by the kernel. It is best to group multiple rules into a single group. Each group of rules delegated results in a new profile block and that will be used as part of the delegated label.

  A//+xxxx

If the profile block is not part of existing policy it will be dynamically constrained by the label is delegated from. That is if a task with confinement A delegates a block of rules A.13 to a task confined by B. Task B's resulting confinement is
  B//+A.13 but A.13 rules will not blindly extend B, they will first be dynamically intersected with A to ensure that A.13 is a true subset of A.


Since delegation requires a confined user to be able to be able to do a restricted policy load, the loaded rule set must be verified to be a subset of the confinement of the task doing the delegation. To guarantee that the rule set is a subset, apparmor will do a dynamic intersection check of the delegated rules and the confinement of the task that did the delegation.

  effectively B//+(A.13&A)

the intersection check can be avoided if the delegated object is predefined in policy (normal delegating tasks cannot change the predefined policy objects so it is possible to know in advance whether the intersection is needed).


delegation api basically needs to cache and create per task the delegated object. Attempt to load the object
and then use the object. Can fail at any point.
 
each delegated rule set also carries a mark where the authority came from, and that is used to limit

delegation through inheritance
onexec
- load object
- set onexec to target + object

delegation through rule passing
- load object
- write object delegation to api, identifying task
- mark task->security object
- task notices, task invalidation, does label update



Making Delegation Permanent
===========================

While delegation in AppArmor is temporary there are several techniques
available to achieve similar affects permanently.

### Authority based attachment

Instead of defining a profile for an application attachment it is possible to declare the applications confinement is based on a combination of other profiles.

authority A//+B//+C /usr/bin/foo,

### ????

???. However this requires being able to update the policy
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

??? dynamic includes

AppArmor provides the ability to control who can edit and load
certain pieces of policy via fine grained controls. A [policy
chunk](AppArmorPolicyChunks) (specialized profile) can be dedicated
to storing delegation changes, and the service/trusted helper that
is doing the delegation can be given permission to edit the policy
chunk. The profile can then be made to reference the chunk, and it
will dynamically pickup the changes every time the chunk is updated
in the kernel.

- in view sub policy namespaces - references and restricted policy update


# Privilege Separation

Portals

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

# Glossary

## Authority

Authority is the right or permission to perform an action. An example
would be a police officer has the authority to make an arrest but
he does not have the authority to determine the sentence of the
arrested person. Authority in the real world is often associated
with an identity or position and some time authority can delegated
to another, Eg. a sheriff deputizing a person to assist him.

In AppArmor authority can be delegated to extend profiles with access
to objects or rules that they would not normally have.

## Identity

In AppArmor authority and identity are different. Authority is the
right/permission to do something while identity is the who. In real
world terms this is like saying Bob the police man. Where Bob is who
and police man is a name of associated with some authority, which is it self a form
of identity.

In AppArmor terms identity is the profile name or label, and delegated
authority can be associated with the identity. The delegated authority
may or may not have an name (identity) associated with it.

## Principle of Least Authority (PoLA)

The [principle of least authority](https://en.wikipedia.org/wiki/Principle_of_least_privilege) (also known as the principle of least privilege) is that a task
should not have any more privilege/permissions than it needs to do
its job.

Unfortunately for many applications the data an application may need to access is determined dynamically, resulting in a need for very broad access rights if 

??????

. Which profiles have to be written
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

# [Confused Deputy Problem](https://en.wikipedia.org/wiki/Confused_deputy_problem)
 The [confused deputy problem](https://en.wikipedia.org/wiki/Confused_deputy_problem)