**WARNING**: Draft WIP

Required Versions
=================

-   AppArmor Kernel module - 3.5-beta1 (Ubuntu 16.04)
    -   Limitations
        -   Stacking is limited to crossing a single policy namespace boundry
        -   The secruity/apparmor/policy interface is not virtualized
        -   The /sys/module/apparmor/parameters/\* interfaces are not virtualized
        -   policy namespaces can not be transitioned to automatically when other system namespaces are entered
        -   system namespacing may still affect policy in ways not desired in a stack, such as the disconnected path applying to all parts of the stack
        -   The 3.5 kernel has a bug that breaks ''ix /foo -&gt; &bar, use ''px /foo -&gt; @{profile\_name}//&bar, instead
-   AppArmor Userspace - 2.11-beta (2.10.95 shipped in Ubuntu 16.04)

Using Stacking in combination with [Policy Namespaces](AppArmorNamespaces)
=====================================================================================

Stacking is not limited to use with profiles within a [policy
namespace](AppArmorNamespaces) as was done in the examples
above. It can be used to set up confinement that crosses [policy
namespace](AppArmorNamespaces) boundaries. This allows AppArmor to
enforce a system policy on a container/chroot, while allowing the
container to specify its own set of policy. It can also be used
to enforce system policy while allowing a user to specify their
own policy.

Expressing a stack that crosses a policy namespace boundry
----------------------------------------------------------

Like a regular profile transitions that changes namespace boundaries

```
 px /bin/foo -> :ns1:profile,
 change_profile -> :ns1:profile,
```

```c
 aa_change_profile(“:ns1:profile”);
```

the stack that is to cross an namespace boundary just includes the
namespace name as part of the profile specification

```
 px /bin/foo -> local_profile//&:ns1:profile,            # specify the stack
 px /bin/foo -> &:ns1:profile,                           # stack against current confinement
 change_profile -> local_profile//&:ns1:profile,
 change_profile -> &:ns1:profile,
```

```c
 aa_change_profile(“local_profile//&:ns1:profile”);
 aa_stack_profile(“:ns1:profile”);
```

Stacks and policy namespace views
---------------------------------

When a task is confined by a stack the profile with the namespace
that is lowest in the namespace hierarchy determines the namespace
view for the task.

Using the above example and assuming that the view is the same as the
current namespace then task C can not see the profile vm1 nor that its
current namespace is `:ns1:`. When it introspects it self it will see

```
 unconfined
```

However another task S that is only confined by the system's root
policy namespace when introspecting task C will see

```
 vm1//&:ns1:unconfined
```

Task C if it tries to introspect task S will only see

```
 ---
```

When a stack contains profiles from different namespaces, some profiles
will be proceeded by their namespace specification.

```
 A//&:ns2:B
```

Profiles from the current namespace will always be shown without
their namespace specifier while all other profiles in the stack will
be proceeded by their namespace specifier.

Stacks and the current namespace
--------------------------------

Because a stack has multiple profiles there is no singular current
namespace, instead the namespace of each profile is the current
namespace for that profiles transitions, and rule enforcement. That is
if a domain transition or rule does not specify a specific namespace
then the profile label it is referencing is relative to the namespace
of the profile making the reference.

For example, given the stack `A//&:ns1://C` with the rules:

```
  A {
      px /bin/foo -> B,
      signal peer=B,
  }
  :ns1://C {
      px /bin/foo -> D,
      signal peer=D,
  }
```

then A's rules are relative to its namespace, while C's use of D
references a profile *D* in *:ns1:* so an exec of /bin/foo would
result in a new stack of

```
  B//&:ns1:D
```

the *current namespace* is always the namespace of the profile in
the stack having its permissions evaluated

label permissions in stacking
-----------------------------

When permissions on a given label are required (like in IPC) the
permission check is done against each component of the target stack
label.

For example:

-   Task 1 is confined by A, and it is communicating with Task 2 confined by B//&C.
-   Profile A must contain the correct permissions to communicate with label B and with label C.

The component test that is done is two fold: a first test is done to
check if profile A can communicate with the stack B//&C, if that test
fails the individual components of the stack are checked, that is can
profile A communicate with profile B and can profile A communicate
with profile C, and the intersected permissions of the two tests is
used to determine whether the communication can occur.

This two fold test allows for granting permissions to communicate with
a specific label stack without granting permission to communicate
with a single component of the stack, ie. the task can communicate
with B//&C but not with a task labeled with just B.

stacking IPC and the cross check
--------------------------------

IPC in AppArmor uses a cross check to ensure policy consistency,
that is can task 1 send some communication to task 2, and can task
2 receive communication from task 1. In the context of stacking the
label permission check is done for each direction.

Again, using the example from above. Task 1 is confined by A and Task 2 by B//&C:
-   A is checked for permission to send to B//&C if successful
-   B is checked for permission to receive from A
-   C is checked for permission to receive from A

If the full stack check was not successful then:
-   A is checked for permission to send to B
-   A is checked for permission to send to C
-   B is checked for permission to receive from A
-   C is checked for permission to receive from A

IPC and labels crossing namespace boundaries in a stack
-------------------------------------------------------

In a stack the General rule for evaluating labels that cross namespace
(not just view boundaries) is that only the portion of the stack that
is in the same namespace is considered.

Example: given Task 1 confined by the stack `A//&:ns1:B` and Task 2 confined by `C//&:ns1:`, then:

-   the label or IPC check will be between A and C as they are in
    the same namespace and B and D

The exception is when a stack does not have profile from the same
namespace as the label or stack being considered.

Example: given Task 1 confined by the stack `A//&:ns1:B`, then if Task 2 is labeled with:
-   `C`        - there is no profile in the same namespace as :ns1:B
-   `:ns1:D` - there is no profile in the same namespace as A
-   `:ns2:E` - there is no profile in the same namespace as A nor :ns1://B
-   `C//&:ns2:E` - there is no profile in the same namespace as :ns1:B

In this situation a few different things can happen.

-   If access has been delegated to the missing namespace then the [delegation](AppArmorDelegation) can take the place of the missing element
-   If the stacks share a parent namespace then a missing child namespace is treated as unconfined

For example, if Task 1 is confined by A and Task 2 is confined by B//&:ns1:C
-   then for permission check purposes A is treated as A//&:ns1:unconfined

-   If the stacks do not share a common namespace, nor have delegation
    permission is denied (this is the same as cross namespace
    communication discussed in [namespaces](AppArmorNamespaces)

Example: label A communicating with :ns1:B

Example:  :ns1:A//&:ns2:B communicating with D//&:ns3:E

Application Directed transitions and Stacks that cross namespace boundaries
---------------------------------------------------------------------------

Like IPC the behavior of application directed (aa\_change\_profile(),
...) transitions changes slightly when the stack of profiles crosses
a namespace view boundry. The application directed transitions
only applies to the portion of the stack that are visible (in
view) to the application. This means that the permission check for
change\_profile is also only done against the portion of the stack
that are visible. The remaining parts of the stack that are not visible
(out of view) don't participate and remain unchanged.

For example, if a task is confined by A//&:ns1:B, with a view set to
:ns1: and the task requests to aa\_change\_profile(“C”). Because
the request for C does not specify a namespace it will transition
relative to the profile's namespace which for A is the root and
profile B is :ns1:. However A is not visible to the task so :ns1:B
(which appears as just B to the task) is the only profile that
participates in the transition.

Given:

```
 A {
 }
 :ns1:B {
   change_profile -> D,
 }
```

then the request will fail because B does not allow changing to profile C.

Given:

```
 A {
 }
  :ns1:B {
    change_profile -> C,
 }
```

then the request will succeed and the stack of A//&:ns1:B will be
replaced by the stack A//&:ns1:C which the task sees as just C.

Given the above example but with a request to aa\_change\_profile(“C//&D”), and:

```
 A {
 }
  :ns1:B {
    change_profile -> C//&D,
 }
```

then the request will succeed and the stack of A//&:ns1:B will
be replaced by the stack A//&:ns1:C&:ns1:D which the task sees as
just C//&D.

As another example, if a task is confined by A//&:ns1:B//&:ns1//ns2:C,
with a view set to :ns1: and the task tasks sees its confinement as
B//&:ns2:C. The task requests to aa\_change\_profile(“D”). Because
the request for D does not specify a namespace it will transition
relative to the profile's namespace which for A is the root, profile
B is :ns1:, and C is :ns1//ns2:

Given

```
   A {
      change_profile -> D,
   }
    :ns1:B {
      change_profile -> F,
   }
   :ns1//ns2:C {
      change_profile -> D,
   }
```

then the request will fail because B does not allow changing to profile D.

Given

```
 A {
    change_profile -> D,
 }
  :ns1:B {
    change_profile -> D,
 }
 :ns1//ns2:C {
    change_profile -> D,
 }
```

then the request will succeed with the stack A//&:ns1:B//&:ns1//ns2:C
transitioning to the stack A//&:ns1:D//:ns1//ns2:D which appears to
the task to be D//&:ns2:D. Notice that the transition to profile D
is actually to two different profiles, :ns1:D and :ns1//ns2:D

**TODO**: transition to specific profile in ns view.

transition to specific profile at root in ns view

transition to stack in ns view

transition involving onexec, show out of view profiles use their exec transition
