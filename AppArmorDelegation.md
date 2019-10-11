
# Warning this document and feature are a WIP

Required Versions
=================

-   AppArmor kernel module 4 or later
-   AppArmor userspace 4 later

Related Documentation
=====================

todo

power box
- trusted helper + delegation OR editing policy (limited or full power box)

sand box
- isolation
- isolation + controlled sharing


# Availability of Delegation of authority
The following table identifies which version of AppArmor different types of delegation are available in.


| ** Delegation** | [Policy directed](AppArmorDelegation#policy-directed-delegation) | [Application directed](AppArmorDelegation#application-directed-delegation) |
|--------------|-----------------|----------------------|
| object based |        ?        |          ?           |
| rule based   |        ?        |          ?           |


# Introduction
AppArmor 4 extends AppArmor to be a hybrid of [Domain Type Enforcement (DTE)](http://citeseer.ist.psu.edu/viewdoc/summary?doi=10.1.1.37.1501) and a [capability system](https://en.wikipedia.org/wiki/Capability-based_security). This is achieved by allowing a profile or task to delegate some of its [authority](AppArmorDelegation#authority) to other applications, allowing them to perform operations or access data that they could not normally do or access under their confinement.

Delegation of [Authority](AppArmorDelegation#authority) (abreviated to just delegation in this document) helps with authoring policy that adheres to the [principle of Least authority](AppArmorDelegation#principle-of-least-authority-pola). Which means policy can be tighter and then expanded to allow access to data as needed. Delegation can also help avoiding problems like the [confused deputy](https://en.wikipedia.org/wiki/Confused_deputy_problem)

## A real world example

The sheriff deputizes Bob (a father) giving him the authority to enforce the law while he is a deputy. Bob now has both the authority of being a parent to his children and the authority of a deputy, and Bob can has the [identity](AppArmorDelegation#identity) of Bob, Father and Deputy.

# Delegation (of authority) in AppArmor

In AppArmor the [authority](AppArmorDelegation#authority-privielge) is the rules in the profile, and the [identity](AppArmorDelegation#identity) is the profile name. Delegation is always temporary and dynamic as it based on passing [authority](AppArmorDelegation#authority) to a task. The delegated authority is not given to other tasks in the system even if those tasks are confined by the same profile. The delegated authority might be [inherited](AppArmorDelegation#inheritance) by a tasks children, or [redelegated](AppArmorDelegation#redelegation) if allowed by policy.

As noted in the [availability of delegation](AppArmorDelegation#availability-of-delegation) there are multiple ways in which delegation can be used and expressed. [Application directed](AppArmorDelegation#application-directed-delegation) delegation is when a task takes a deliberate action to delegate to another task (usually a child). [Policy directed](AppArmorDelegation#policy-directed-delegation) delegation is used when the policy causes delegation to occur without an explicit action from the task. Both of these types of delegations can be further split into whether rules are being delegated or just access to a specific object (file descriptor).


# Application directed delegation

Applications can take action to delegate some or all of their authority to another application, if allowed by the confining profile. This is done by the application taking an explicit action to either delegate an open file descriptor or to delegate profile rules. This mean the application must have code that will direct apparmor on what should be delegated.

To delegate objects an application uses
- unix domain socket fd passing, to pass fd objects to an existing task+
- apparmor api for object delegation, to control inheritance of open fds*

To delegate rule the application uses
- apparmor api for rule delegation

In addition to the application having to take explicit action to delegate authority (rule or object) the applications confinement must also allow the delegation.

+ the use of standard unix fd passing over sockets means many applications support object delegation without needing to be moified explicitly to support apparmor.
\* It is important to note that the default of task's inheriting open files is not an explicit action and does cause delegation of authority. If this behavior is desired it can be achieved through [policy directed delegation](AppArmorDelegation#application-directed-delegation).


### Application directed delegation has to be allowed by the profile

For an application to perform delegation the profile must contain a rule allowing delegation.

```
profile example {
  # allow delegating to any profile
  allow delegation,
}
```

Delegation can be restricted to specific profiles by specifying the profile name as a target.

```
profile example {
  allow delegation -> foo,
  allow delegation -> bar,
}
```

Like capability names profile names can be collapsed into a list.

```
profile example {
  allow delegation -> (foo bar),
```

Pattern matching can be used in the target profile name to allow multiple profiles with a single name.

```
profile example {
  allow delegation -> foo*,
```

This can be particularly useful when combined with namespaces to allow all profiles in a given namespace.

```
profile example {
  allow delegation -> :ns:**,
```

### Delegation can have additional restrictions
Delegation can have additional restrictions and qualifiers. In particular delegation can be restricted to children tasks.

```
profile example {
  # allow delegating to any child, no matter its profile
  allow child delegation,
}
```

and these restriction can be combined with the profile restriction

```
profile example {
  allow child delegation -> foo,
}
```

### A task can not delegate permissions it doesn't have

In addition to delegation being limited by the delegation rule, the application can only delegate permissions granted in the profile.

The profile

```
profile example {
  allow delegation -> foo,
  allow delegation -> bar,
}
```

does not actually allow delegating anything as it does not have any rules that grant permission.

The profile

```
profile example {
  allow delegation -> foo,
  allow delegation -> bar,

  rw @{HOME}/**,
}
```

would be limited to delegating read write access to files in @{HOME}.

### The set of rules that can be delegated can be limited in policy

By default the task can delegate any permission granted by its profile, but the set of what can be delegated to a profile can be further controlled to a limited subset of the profiles permissions.

```
profile example {
  allow delegation -> foo <= {
      rw /**,
  },
  allow delegation -> bar,
}
```

It is important to note that if the set of rules limiting the delegation contains permissions not granted by the profile those permissions will be dropped. The rule block is only for reducing the possible set of permissions delegated it is not possible for the task to delegate permissions the profile does not have.

#### Rule sets can be named

Rule sets can be given a name, by making them a profile, which can then be used in place of the block of rules.

```
profile bar {
  rw /**,
}

profile example {
  allow delegation -> foo <= bar,
}
```

### Delegation can be restricted to open objects

The profile can limit the delegation to already open files using the ```open``` qualifier. This prevents the child task from being able to open new files that match the delegated rule.

```
profile example {
  rw @{HOME}/**,

  allow delegation -> /usr/bin/child <= {
      open rw @{HOME}/**,
  }
}
```

Rules that do not have the ```open``` only restriction restriction will also allow for object delegation, but the open restriction does not allow rules to be delegated.


# Policy directed delegation

Policy directed delegation is done on behalf of the task at exec time without any additional task initiated action and is expressed as extending a task's profile with additional rules. In effect it is defining a new custom extended profile except that ipc rules to the profile label will continue to work and there is the possibility of partial dynamic replacement.

It is important to note that policy directed delegation is done at exec time and hence is always limited to children tasks.

### Policy directed delegation is always done through profile rules.

To delegate some rules to a child task the exec rule can be extended with additional rules that extend the target profile.

```
profile example {
  rw @{HOME}/**,

  px /usr/bin/child + {
      # extend the profile for /usr/bin/child with the following rules.
      rw @{HOME}/**,
  }

}
```

When executed the child task will be confined by the profile for /usr/bin/child but extended by the additional rules.

### The task can not delegate permissions it doesn't have

The above example the ```rw @{HOME}/**,``` rule appears twice, once in the profile and once in the block of rules being delegated. This is because the task (and hence the profile) can not delegate permissions that it does not have.

```
profile example {
  rw @{HOME}/**,

  px /usr/bin/child + {
      rw @{HOME}/**,
      rw /etc/passwd,
  }
}
```

Since the example profile does not have access to ```/etc/passwd``` it can not be delegated. The compile will fail with an error message. In the case of run time application directed ?link? delegation the delegated rule set will be dynamically bounded by the profile ensuring that this restriction is enforced.

#### Policy directed delegation exception

With policy directed delegation it is possible to delegate authority that the profile does not have. This is akin to specifying a transition to a profile that has more permissions. To do this a tag is added to the delegation.

```
profile example {
  rw @{HOME}/**,

  px /usr/bin/child +(override???) {
      rw @{HOME}/**,
      rw /etc/passwd,
  }
}
```

??? better tag than (override???)

### Delegation can be restricted to open files

The profile can limit the delegation to already open files using the ```open``` qualifier. This prevents the child task from being able to open new files that match the delegated rule.

```
profile example {
  rw @{HOME}/**,

  px /usr/bin/child + {
      open rw @{HOME}/**,
  }
}
```

### Overlapping rules can be used to control delegation

Overlapping rules can be used to determine delegation permissions. The ```open``` qualifier is not accumulated like regular permissions but instead applied on a most specific match basis similar to exec rule qualifiers.

```
profile example {
  rw @{HOME}/**,

  px /usr/bin/child + {
      open rw @{HOME}/**,
      rw @{HOME}/Downloads/*,
  }
}
```
In this example @{HOME}/Downloads/* is more specific so the rule is delegated instead of only open files.


### Rule sets can be named to avoid having to retype rules.

Like with application directed delegation the rule sets can be named by making them non-attaching profiles.

```
profile example {

   profile foo {
      rw @{HOME}/**,
      r  /tmp/**,
  }

  px /usr/bin/child + foo,

}
```

### Multiple rule sets can be delegated
```
profile example {

   profile foo {
      rw @{HOME}/**,
      r  /tmp/**,
  }

  profile bar {
      r /etc/passwd,
      rw @{HOME/.config/**,
  }

  px /usr/bin/child + foo + bar + {
      mr /usr/lib/**,
  }
}
```

### Sharing rules sets

Rule sets can be shared between multiple profiles

```
profile shared {
      rw @{HOME}/**,
      r  /tmp/**,
  }

profile one {
  px /usr/bin/child + foo,

}

profile two {
  px /usr/bin/bash + foo,

}

```

# Notes on object & rule delegation

There are aspects of object and rule delegation that are common regardless if the delegation was done via an application directing the delegation or by policy rules.

dynamic masking????
 +(A&B)
- dynamically computed using stacking
- statically computed where possible

### Object Delegation

delegator is tracked .... ????

#### object delegation against non-open rules.

When object delegation is used permission to delegate the object is not limited to rules with the open qualifier. Eg.

```
profile example {
  rw @{HOME}/**,

  allow delegation -> /usr/bin/child <= {
      rw @{HOME}/**,
  }
}
```

#### What happens when object delegation fails

If the application tries to delegate an object and the delegation is not allowed the object may still be allowed to be passed, it just won''t be done under delegated authority. Instead when delegation fails the object is revalidated against the target tasks confinement, and if allowed by the target tasks confinement the object may still be passed. This fall back is how apparmor handle object passing and inheritance before delegation was supported.


Profile example can be used to delegate any object allowed by the rule `rw @{HOME}/**`. Basically for object delegation all rules are treated as if the ```open``` qualifier was applied. That is to say object delegation can only be used to pass already open object and not rules.


### Rule delegation

Delegating rules are a subset of the delegating profile.

#### Application directed rule delegation
When using the delegation api a task can specify a set of rules to delegate. This rules will be restricted to be a subset of the rules allowed by the delegation rules.

profile, delegator is tracked, intersection ... ????

?????
Rules being delegated go through a compile to transform the rules into a form that can be used by the kernel. It is best to group multiple rules into a single group. Each group of rules delegated results in a new profile block and that will be used as part of the delegated label.

  A//+xxxx

If the profile block is not part of existing policy it will be dynamically constrained by the label is delegated from. That is if a task with confinement A delegates a block of rules A.13 to a task confined by B. Task B''s resulting confinement is
  B//+A.13 but A.13 rules will not blindly extend B, they will first be dynamically intersected with A to ensure that A.13 is a true subset of A.


Since delegation requires a confined user to be able to be able to do a restricted policy load, the loaded rule set must be verified to be a subset of the confinement of the task doing the delegation. To guarantee that the rule set is a subset, apparmor will do a dynamic intersection check of the delegated rules and the confinement of the task that did the delegation.

  effectively B//+(A.13&A)

the intersection check can be avoided if the delegated object is predefined in policy (normal delegating tasks cannot change the predefined policy objects so it is possible to know in advance whether the intersection is needed).


delegation api basically needs to cache and create per task the delegated object. Attempt to load the object
and then use the object. Can fail at any point.
 
each delegated rule set also carries a mark where the authority came from, and that is used to limit

#### Policy directed rule delegation and named rule sets.

Policy directed delegation has flexibility on how to handle named rule sets. If the named rule set is compiled with the profile doing the delegating AppArmor can do compile time optimizations that can guarentee the delegated rule set is a subset of the delegating profile. This optimization will reduce the run time overhead of delegating rules as AppArmor will not have to perform an intersection on the delegated rule set like it have to do for Application directed rule delegation.


# Task labels under delegation

Delegation affects what the task label is and how it is displayed. How the label is affected different based on whether rules or objects are being delegated.

### Task labels under Rule delegation

Because delegation is task based a tasks label will be composed of the different [identities](AppArmorDelegation#identity). This is done by appending the delegated information to the profile name with the character sequence *//+*. separating the components.

For the introductory real world example, the task label in apparmor might be expressed as

```
Bob//+deputy
```

and if the authority of ```father``` is split into its own [identity](AppArmorDelegation#identity) separate from Bob we would have

```
Bob//+deputy//+father
```

The order of the Delegation is unimportant, the [identity](AppArmorDelegation#identity) of the task is all of the profiles in the label.


#### Unamed rule blocks and labels

When an unamed rule block is used to specify delegation. Eg.

```
profile example {

  px /usr/bin/child + {
      rw @{HOME}/**,
  }
}
```

The rule block will be a created as child profile assigned a unique name based on the exec rule, and that will be name that appears as part of the task label during delegation.

Eg.

```
/usr/bin/child//+example//delegate_usr/bin/child
```

#### Task delegated Rules and labels

When a task explicitly delegates rules as special child profile with a unique name is created for the delegation. Eg.

* task T1 is confined by profile example
* task T2 is confined by profile target

```
profile example {
  allow delegation -> target,
  ...
}
```

task T1 delegates some rules to T2 via the apparmor rule delgation api. Then task T2 is confined by the label

```
target//+example//#<T1 pid>_<unique>
```

where `<T1 pid>` is task T1s pid and `<unique>` is an additional unique identifier if needed.


### Task labels under Object delegation

When delegation to a task is limited to objects it does not explicitly extend the task (subjects) authority with a new set of rules, so it does not manifest in the tasks labell ing the same way as rule. Instead the tasks label remains the same as a task that has no delegation except it carries a trailing ```//*``` mark indicating that objects have been delegated extending its authority.

```
bob//*
```

the actual set of objects that have been delegated to the task can be found by looking in the tasks ```/proc/<pid>/fd/``` directory.

### conjunctive normal form

When apparmor labels contain both stacking and delegation, the label can be complicated to understand. In the label stacking always has higher priority than delegation. The exception is when ```()``` to indicate a higher priority grouping. Eg.

```
(bob//&jane)//+police
```

the above label may be decomposed into [conjunctive normal form](https://en.wikipedia.org/wiki/Conjunctive_normal_form) to remove the ```()``` grouping.

```
bob//+police//&jane//+police
```

Both forms are valid and what form is used during display is implementation specific.

Regardles of the form used to display the label, the object delegation mark will always be the last element of a label.



# Inheritance
### Delegation is inheritable

Delegation is inheritable by tasks but whether it will be inherited is controlled by policy.

If a task inherits its parent confinement it will also inherit any delegation it parent has. However if the tasks confinement changes the delegation will be re-evaluated to determine if the delegation was allowed to the new profile. If the originally delegating profile allows the delegation to the new profile then the delegation will be inherited.

eg. ....


Also need way to allow to delegate to any child.


Does the whole delegation get dropped or do we do intersections.

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




# Advanced topics

## Run time implications of delegation

### delegation and unconfined

The unconfined state delegates its open object access. This behavior has always existed in apparmor, but the ability was not available in profiles. It is equivalent to

```
profile unconfined {
  allow delegation <= {
    open /**,
  }

}
```



## Delegating permissions to a stack

### Permission evaluation

## Profile transitions


## Delegation to Profiles that contain the delegated authority

It is possible that a task could delegate authority to another task that is confined by a profile that contains all the delegated authority. In this situation the delegation is not dropped or thrown away, it is kept and carried as part of the task label. This ensures that inheritance of the delegation will not be lost in a profile transition occurs.


## Delegation and profile replacement

the reference profile is updated, potentially resulting in new restrictions or new privileges.

## Delegation vs. Revalidation

## Delegation and Change_hat/Change_profile


## Delegation and no_new_privs (nnp)

Delegation can not be used to circumvent the no_new_privs restrictions. While delegation may be allowed task confinement will be enforced such that neither object nor rule delegation can be used to expand a tasks authority.







### Delegation of exec rules

- if delegated rule set has a matching exec rule it causes the profile to transition (the delegator allowed this), it does not replace the delegator though.
- if the delegated rule set does not have a matching exec rule. The delegator is checked to see if the rule set can be delegated to the new target. What is the new target? when the label existing of multiple profiles.
- if no delegation allowed the rule set dropped.

  exec rule will cause delegated blob to transition. It says this is inheritable to X

dont put exec rules in delegated rule sets unless you want this

Each delegated permission set will be evaluated based on the profile it was delegated by. If the permission set is allowed then the delegation will carry, otherwise it will be dropped.

what about reducing permissions, allowing delegated set to reduce to a smaller set automatically?
Requires knowing the smaller set. Could be done with dynamic masking via stack. In some cases could be precomputed.


What of fd delegation. fds will be closed if transition profile does not allow it
Permission check order
  Find target label
  fd delegation label is in target label
  if not check revalidation


### Delegation of change_profile rules



#### Profile composition

labels can be given attachments

While not delegation (Move else where) profile attachment can be used with labels.

label example//+foo   /usr/bin/**,
label example//&bar//+foo  /usr/bin/**,
label example//&(bar//+foo)  /usr/bin/**

compound labels can be given a short name alias
label example=free//&bar

???? ordering with stacking

how is composition different from delegation
- same as delegation when evaluating permission
- doesn''t dynamically track where the permission came from (delegation label is null)
- segments just drop based on exec, no check to see if it can be inherited?

file inheritance
- check if original delegator allows passing to target
- if not revalidate


What happens delegation of a block comes from 2 areas (say inheritance + explicit delegation), asingle reference to the block is kept and the delegation label contains both sources. If inheriting/passing both sources are consulted if one doesn't allow it is dropped. Delegation block is only dropped when all its sources are dropped.

This means a label now needs to consist of 2 vecs. 1st vec is label, 2nd is delegators/source with NULL used for authority that wasn't delegated.
Hrmmm how does this work with nesting? source could have same nesting.
How does it work with stacking? Just does, its nesting that is the issue.
Other potential format, vec is array of struct (profile, source)






# Delegation and Domain transitions

When an exec transition or change_profile happens causing a profile transition. The tasks confinement and delegation is re-evaluated.

First each component of the task confinement is evaluated for transitions, this is done for the profile as well as the delegated rules.  ??? do the delegated rule objects replace themselves if they have exec rules or the profile ???

After the transition evaluation the delegated rules are checked for inheritance.


# delegtion and stacking

confinement is always expressed in conjunctive normal form

delegating to a stack
(A//&B)//+C is transformed into (A//+C)//&(B//+C)






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




Delegation and Stacking
=======================


# Glossary

### Authority & Privielge
Authority is the right or permission to perform an action (do something). An example would be a police officer has the authority to make an arrest but he does not have the authority to determine the sentence of the arrested person. Authority in the real world is often associated with an identity or position and some time authority can be delegated to another, Eg. a sheriff deputizing a person to assist him.

Privilege is usually (and in AppArmor) used as a synanym for authority.

In AppArmor authority can be delegated to extend profiles with access to objects or rules that they would not normally have.


### Identity

The fact of being who or what a person or thing is

name with semantic meaning??????

In AppArmor identity is the name associated with a set of rules. This is often a profile name but could be a random name associated with a set of rule. When a task has multiple identities assigned to it, all identities are equal.

In real world term this is like saying a person Bob is also a police man. The individual is both of those things and has the rights and responsibilities of both Bob and a police man.

In AppArmor terms this is exposed in the label by listing the profile names that contribute to the task's authority.

  Bob//+police_man

Identity is also used in policy to control which authority is or can be delegated.


### Identity

In AppArmor authority and identity are different. Authority is the
right/permission to do something while identity is the who. In real
world terms this is like saying Bob the police man. Where Bob is who
and police man is a name of associated with some authority, which is it self a form
of identity.

In AppArmor terms identity is the profile name or label, and delegated
authority can be associated with the identity. The delegated authority
may or may not have an name (identity) associated with it.

### Principle of Least Authority (PoLA)

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

### [Confused Deputy Problem](https://en.wikipedia.org/wiki/Confused_deputy_problem)
 The [confused deputy problem](https://en.wikipedia.org/wiki/Confused_deputy_problem) is a type of privilege escalation where an application with more privilege is tricked by an application into doing some for it that it does not have the authority to do. Delegation can help reduce/remove the confused deputy problem by giving an application the minimum privelege to do what it needs removing the need for applications to interacte with application that have more privilege than is required.
 
