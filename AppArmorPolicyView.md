Related Documents
=================

Introduction
============

The policy view determines whether a namespace is treated as the
root for policy visibility. By default when a new policy namespace
is created it is also treated as a new policy view. This feature is
used by containers like LXD to load and treat their policy as if it
is the root system policy. Note that there are cases where the use of
policy namespaces is beneficial but a new view is not desired. Eg,
setting up user roles or application sandboxing where visibility of
system policy is desirable.

The policy view affects how policy is authored because it affects
the names that are used in transitions and for communication between
tasks. As such policy must be authored with a certain view in mind. It
is the policy author's responsibility to ensure the policy and views
required for a namespace are correctly setup.

**TODO**: transitions between profiles in different namespaces...

Namespace View
==============

AppArmor policy namespaces are strictly hierarchical but the full
hierarchy may not be visible to all tasks. A virtualized view of the
policy namespace hierarchy may be set and tasks under that view can
as a result only see the portion of the hierarchy within their view.

Example:

> Given a hierarchy of :ns1//ns2:  and a view set to :ns1: then only the profiles in ns1 and ns2 will be visible.

The profile introspection and management interfaces are virtualized
to the current namespace view of the task doing profile management
or introspection. Note: See Interfaces and current view???(or is it
current namespace???) for additional notes

The View acts as the root namespace for tasks assigned to that view,
and the name of the View namespace will not be reported to tasks in
that view.

Example:

> If profile foo is loaded into :ns1:, and Task A has its View set to :ns1: then when task one introspects
> task B confined by :ns1:foo it will see the confinement of task B as just *foo*. Tasks outside the
> view may be able to see the full hierarchy or nothing depending on their own views.

While the AppArmor interfaces are virtualized, depending on how the
kernel and other system namespaces (proc, user, ...) are configured it
may be possible to introspect a task that is outside of the namespace
View. In this situation the confinement of the task will be reported
as having the name **---**. AppArmor will not expose the namespace
name nor the profile name of the task.

For example, given the namespaces

```
   root policy namespace
   :ns1:
   :ns1//ns2:
   :ns3:
```

And the following tasks and views

-   Task A has a view set to the root system policy namespace, with it confinement set to unconfined
-   Task B has a view set to ns1, with its confinement set to unconfined of namespace :ns1:
-   Task C has a view set to ns1//ns2, with its confinement set to unconfined of namespace :ns1//ns2:
-   Task D has a view set to ns3, with its confinement set to unconfined of namespace :ns3:

Then

-   Task A
    -   can see profiles in the system namespace
    -   can see the namespaces :ns1:, :ns1//ns2:, and :ns3: and the profiles in them
    -   sees the confinement of
        -   Task A (itself) as *unconfined*
        -   Task B as *:ns1:unconfined*
        -   Task C as *:ns1//ns2:unconfined*
        -   Task D as *:ns3:unconfined*
-   Task B
    -   can NOT see profiles in the system namespace
    -   can see profiles in namespaces :ns1: which appears to be the root system namespace to it
    -   sees namespace :ns1//ns2: as :ns2: and can see the profiles in it
    -   can NOT see namespace :ns3: nor the profiles in it
    -   sees the confinement of
        -   Task A as --- 
        -   Task B (itself) as *unconfined*
        -   Task C as *:ns2:unconfined*
        -   Task D as ---
-   Task C
    -   can NOT see profiles in the system namespace
    -   can NOT see namespace :ns1: nor :ns3: nor their profiles
    -   can see profiles in namespaces :ns1//ns2: which appears to be the root system namespace to it
    -   sees the confinement of
        -   Task A as ---
        -   Task B as ---
        -   Task C (itself) as *unconfined*
        -   Task D as ---
-   Task D
    -   can NOT see profiles in the system namespace
    -   can NOT see namespace :ns1: nor :ns1//ns2: nor their profiles
    -   can see profiles in namespaces :ns3: which appears to be the root system namespace to it
    -   sees the confinement of
        -   Task A as ---
        -   Task B as ---
        -   Task C as ---
        -   Task D (itself) as *unconfined*

How the Namespace View affects Policy
-------------------------------------

The namespace view not only affects what policy is visible from a
task but also affects how policy needs to be authored, and managed.

**TODO**

Namespace View and policy transitions
-------------------------------------

Namespace View and policy management
------------------------------------

??? how is the view of a namespace set

-   Note: Before AppArmor 4, the View and current namespace are the same

Defining/Setting the Namespace View
-----------------------------------

There are two ways that a namespace view can be set. It can be defined
in policy, or if policy does not explicitly define the namespace view
it can be set via the apparmorfs interface (used by the api).

### Defining the View in Policy

The view is set relative to the namespace the policy is being defined for.

```
 namespace ns1 {
     view ./, # view of ns1 is set to the policy root 
      namespace ns2 {
         view ns1, # view of ns1//ns2 is set to that of ns1 
          profile D {
             ...
         }
     }
      namespace ns3 {
         # view not defined so set to ns1//ns3
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

### Setting the View via the apparmorfs interface

The apparmorfs interface can be used to set a namespace view when
policy does not define the view of the namespace. This is usually
occurs because the namespace is being dynamically created by a trusted
helper (lxd, pam, ..), and the policy of the new namespace doesn't have
to directly interact with existing policy. When a namespace is created
without its view being set, either via the profile load interface,
or the apparmorfs interface, the namespace's view is defaulted to
its self (its policy will not see existing policy on the system).

The View of a policy namespace can be set by a task in an ancestor
namespace with admin rights over the namespace. The setting task can
set the namespace view to any namespace it can view including its
own view. A task confined by the namespace can not change the view
of the namespace, in fact once a task is confined by the namespace
its view can not be changed.

The View of a namespace can be set by writing to the *view* file under
the its namespace directory. Eg. to set the View of namespace ns1 to
the current tasks view it could write

```
 echo “.” > /sys/kernel/security/apparmor/policy/namespaces/ns1/view
```

Current Namespace(s)
--------------------

When a task is created it is assigned to a policy namespace (usually
inherited from its parent). When stacking is used the task may be
assigned to multiple policy namespaces. From a mediation point of view
all of the task's namespaces are active and enforced (even the ones
out of the task's view). Once a task has entered a namespace there is
no way for it to leave that policy namespace unless so directed/defined
in the policy of that namespace. The current namespace of a given task
is the namespace of the task's label or if the label is composed of
multiple profiles it is namespace that is the furtherest descendent
from the system's root namespace.

That is if the task is confined by

```
 profile_A//:ns1:profile_B//:ns1//ns2:profile_C
```

the current namespace will be ns1//ns2.

The current task's namespace is not directly used in mediation but
is used to find a task's

-   current View namespace
-   current Scope namespace

which are used in mediation.

The current namespace is always the Scope namespace or one of its
children, it can never be a parent of the namespace Scope, and the
Scope namespace is always the same as the View namespace or one of
its children. (ie. current View &gt;= current Scope &gt;= current
namespace)

-   **Note:** Before Version 4.0 of the AppArmor kernel module the
    current namespace was always set as the current View such that
    the current namespace always appeared to be the root namespace
    for tasks in that namespace.

Current View
------------

The current View namespace of a task governs which namespace is used
for profile lookups, profile attachments, and transitions. It also
determines what part of the confinement stack can be viewed by a task
when doing introspection. The current View is determined by the view
namespace of the the task's current namespace. The current view will
be the same as the current namespace or an ancestor of it.

E.G., given a child namespace :child1: with its view set to the root namespace and with profile R in the root namespace, profile C in the the child namespace, task Tr confined by R and task Tc confined by C, then

-   Tr
    -   views Tc confinement as :child1:C
    -   views its own confinement as R
-   Tc
    -   views its own confinement as :child1:C
    -   views Tr confinement as R

Now lets say Tc and Tr both try to transition to a profile X

-   Tr will transition to X in the root namespace
-   Tc will transition to a different profile X in the namespace
    :child1: because its current namespace is :child1: and profile
    lookups that don't specify a namespace are relative to the current namespace.

However if the transitions is to :child1:X

-   Tr will transition to the X profile in :child1:
-   Tc will still transition to the X profile in :child1:

This is because the namespace lookup is relative to the view not the
current profile.

Similarly if the transitions is to :child2:Y

-   Tr will transition to the Y profile in :child2:
-   Tc will transition to the Y profile in :child2:

??? Eg. A transition of

```
 px /** -> /bin/foo,
```

is relative to the current namespace.

While a transition of

```
 px /** -> :foo:/bin/foo,
```

is relative to the current view.
