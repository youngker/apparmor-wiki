Introduction
============

The scope of mediation determines how profiles interact during
mediation. The scope of mediation is used to control whether a
namespace's policy is allowed to interact with and influence the
mediation being enforced by an ancestor namespace's policy, or whether
the policy within a namespace should stand on its own.

Example 1: container based VM
-----------------------------

For LXD which provides a container based VM for an operating system, the scope of the container's
namespace is set so that the policy loaded by the containers operating system, which does not need to be
aware of the rest of the system's policy.

Example 2: containerized application
------------------------------------

Application's that have been containerized to control their potential impact on the rest of the
system, still need to interact with system policy. The scope of the application confinement
must be set so that its confinement can interact with the rest of system confinement

Example 3: User defined policy
------------------------------

The user is allowed to load their own policy into a namespace that will be applied against the applications
the user runs. When user policy is applied it is desirable for the user to still see system policy is
applied, so the View of the user defined policy namespace is set to that of the system, but the user defined
policy should not interact with system policy as its rules are not designed interact with arbitrary user
define policy so the Scope of the user defined policy namespace is set (left) to be itself (the default).

Scope of Mediation
==================

The scope of mediation only comes into affect when a task or object
has a compound (more than one profile/label stacked) label that
references more than one policy namespace. Within the compound label
stack the scope of mediation controls all profile interactions (ipc,
networking, transitions...)

The scope of mediation affects all profile interactions (ipc,
networking, transitions...), and limits which profiles are used in
userspace api requests. The scope is a subset of (or the same as)
the view allowing for tasks to view confinement that they can not
directly interact with.

Effects on policy
=================

Policy must be authored with the scope of mediation in mind. The scope

Defining/Setting the Namespace Scope
------------------------------------

There are two ways that a namespace scope can be set. It can be defined
in policy, or if policy does not explicitly define the namespace
scope it can be set via the apparmorfs interface (used by the api).

### Defining the Scope in Policy

**TODO:** add, view affecting scope examples because scope must be a
subset of view

The view is set relative to the namespace the policy is being defined for.

```
 namespace ns1 {
     scope ./, # view of ns1 is set to the policy root 
      namespace ns2 {
         scope ns1, # view of ns1//ns2 is set to that of ns1 
          profile D {
             ...
         }
     }
      namespace ns3 {
         # scope not defined so set to ns1//ns3
         profile E {
             ...
         }
     }
      profile C {
         ...
     }
 }
  profile A {
     ...
 }
  profile :ns1:B {
     # profile defined external to its namespace block, view is still set in namespace block 
     ...
 }
```

### Setting the Scope via the apparmorfs interface

The apparmorfs interface can be used to set a namespace scope when
policy does not define the scope of the namespace. This is usually
occurs because the namespace is being dynamically created by a trusted
helper (lxd, pam, ..), and the policy of the new namespace doesn't have
to directly interact with existing policy. When a namespace is created
without its scope being set, either via the profile load interface,
or the apparmorfs interface, the namespace's scope is defaulted to its
self (its policy will not interact with existing policy on the system).

The Scope of a policy namespace can be set by a task in an ancestor
namespace with admin rights over the namespace. The setting task can
set the namespace scope to any namespace it can View including its
own View. A task confined by the namespace can not change the Scope
of the namespace, in fact once a task is confined by the namespace
its scope can not be changed.

The Scope of a namespace can be set by writing to the *scope* file
under the its namespace directory. Eg. to set the Scope of namespace
ns1 to the current tasks view it could write

```
 echo “.” > /sys/kernel/security/apparmor/policy/namespaces/ns1/scope
```
