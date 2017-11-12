[[_TOC_]]

**WARNING**: Draft WIP

Required Versions
=================

-   AppArmor Kernel module - 3.5-beta1 (Ubuntu &gt;= 16.04)
    -   Limitations
        -   Stacking is limited to crossing a single policy namespace boundary
        -   The secruity/apparmor/policy interface is not virtualized
        -   The /sys/module/apparmor/parameters/\* interfaces are not virtualized
        -   policy namespaces can not be transitioned to automatically when other system namespaces are entered
        -   system namespacing may still affect policy in ways not desired in a stack, such as the disconnected path applying to all parts of the stack
        -   The 3.5 kernel has a bug that breaks ''ix /foo -&gt; &bar, use ''px /foo -&gt; @{profile\_name}//&bar, instead
-   AppArmor Userspace - 2.11-beta (2.10.95 shipped in Ubuntu 16.04)

![AppArmorPolicyTOC](AppArmor Policy Table of Contents)

Introduction
============

AppArmor now supports (see [\#Required\_Versions](#Required_Versions))
the stacking of two or more profiles to create a confinement that
is the intersection of all profiles in the stack. Stacking is done
dynamically (at run time) and provides several abilities to the policy
author: it can be used to ensure that confinement never becomes
more permissive, to reduce the permissions of a generic profile on
a specific task, and to provide both system level and container and
user level policy (when combined with policy namespaces).

How a stack is expressed
========================

In AppArmor a stack is expressed by concatenating two or more
profile names together with the character sequence **//&** between
each profile. For example, if profile A and profile B are stacked,
the stack is expressed as **A//&B**. For a larger stack of profile A,
profile B and profile C, the stack is expressed by **A//&B//&C**.

A stack may also be expressed using the current confinement as an
implicit component by specifying the stack starting with an **&**
character. For example, if a process is confined by profile A and
**&B** is specified for stacking, the stack is expressed as **A//&B**
while with **&B//&C** the stack is expressed as **A//&B//&C**.

The order of the profiles in the stack expression is unimportant. For
example, **A//&B** is equivalent to **B//&A**. Internally AppArmor will
always create a unique ordering and use that ordering in reporting,
but this is an implementation detail subject to change and user
applications and policy can specify the stack in any order that
is convenient.

While the stacking syntax may not show up in policy, for many use
cases it is important to know the syntax because it will show up
under introspection and in audit logs.

Application and policy behavior when using stacked confinement
==============================================================

The majority of AppArmor rules are evaluated as the intersection of
profile permissions where a given rule must exist in each profile
in the stack to be available for it to apply to the process running
under that stack. For example,

```
  profile A {
    /foo r,
    /bar r,
    /baz r,
  }
  profile B {
    /foo r,
    /bar r,
    /norf r,
  }
  profile C {
    /foo r,
    /baz r,
    /norf r,
  } 
```

In the above, a profile running under

-   A is allowed to read '/foo', '/bar' and '/baz' (no stacking involved)
-   B is allowed to read '/foo', '/bar' and '/norf' (no stacking involved)
-   C is allowed to read '/foo', '/baz' and '/norf' (no stacking involved)
-   A//&B is allowed to read '/foo' and '/bar' ('/baz' isn't in B and '/norf' isn't in A so those aren't allowed)
-   A//&C is allowed to read '/foo' and '/baz' ('/bar' isn't in C and '/norf' isn't in A so those aren't allowed)
-   B//&C is allowed to read '/foo' and '/norf' ('/bar' isn't in C and '/baz' isn't in B so those aren't allowed)
-   A//&B//&C is allowed to read '/foo' ('/bar' isn't in C, '/baz' isn't in B and '/norf' isn't in A so those aren't allowed)

Note: when a permission request is allowed, some rules (eg, resource
and exec) have effects beyond granting the permission. This will be
discussed below.

Resource rules
--------------

Resource rules, like rlimits in general, will grant permissions based
on the lowest resource bounds allowed. Therefore when setting the
task's rlimits the limit will be set to the smallest resource bound
allowed by the stacked policy.

Exec rules
----------

In addition to allowing the exec, rules that control exec transitions
have effects on confinement beyond allowing an exec and they control
which profile(s) confine the process after the exec is allowed. When
a process is confined by a set of stacked profiles, exec transitions
are evaluated and applied on a per profile basis with the result being
evaluated into the new confinement. If any duplicate confinement is
encountered then the duplication is eliminated (eg, the intersect
of profile A with another profile A (**A//&A**) reduces to just
profile **A**).

### Examples

Given a process that is confined by profile A and profile B (ie,
the application is confined by the stack **A//&B**), consider the
effect on policy for the exec transitions in the following:

#### Example 1

```
  profile A {
    /bin/example ix,
    ...
  }
  profile B {
    /bin/example px -> C,
    ...
  } 
```

when an application running under **A//&B** exec()s /bin/example:

-   the exec rule in profile A specifies no transition (ie, stay in profile A)
-   the exec rule in profile B specifies to transition to profile C

Therefore the resulting confinement of /bin/example is the stack of
profile A and profile C (**A//&C**).

#### Example 2

```
  profile A {
    /bin/example px -> C,
    ...
  }
  profile B {
    /bin/example px -> D,
    ...
  } 
```

when an application running under **A//&B** exec()s /bin/example:

-   the exec rule in profile A specifies to transition to profile C
-   the exec rule in profile B specifies to transition to profile D

Therefore the resulting confinement of /bin/example is the stack of
profile C and profile D (**C//&D**).

#### Example 3

```
  profile A {
    /bin/example px -> B,
    …
  }
  profile B {
    /bin/example px -> C,
    …
  } 
```

when an application running under **A//&B** exec()s /bin/example:

-   the exec rule in profile A specifies to transition to profile B
-   the exec rule in profile B specifies to transition to profile C

Therefore the resulting confinement of /bin/example is the stack of profile B and profile C (**B//&C**).

#### Example 4

```
profile A {
   /bin/example px -> C,
   …
}
profile B {
   /bin/example px -> C,
   …
} 
```

when an application running under **A//&B** exec()s /bin/example:

-   the exec rule in profile A specifies to transition to profile C
-   the exec rule in profile B specifies to transition to profile C

Therefore the resulting confinement of /bin/example is the stack of profile C and profile C (**C//&C**, which is reduced to simply **C**).

### Environment variable scrubbing

Environment variable scrubbing for a stacked exec transition is
performed whenever any of the transitions in the stack specify
a transition with scrubbing. While this can result in stricter
confinement than specified in some of the profiles, it is required
to meet the stricter confinement specified by the transition that
specified scrubbing. For example:

```
profile A {
   /bin/example px -> C,
   …
}
profile B {
   /bin/example Px -> C,
   …
} 
```

when an application running under **A//&B** exec()s /bin/example:

-   the exec rule in profile A specifies to transition to profile C
-   the exec rule in profile B specifies to transition to profile C with environment scrubbing

Therefore the resulting confinement of /bin/example is the stack of
profile C and profile C with scrubbing (**C//&C**, which is reduced
to simply **C** with scrubbing).

Expressing stacking in Policy
=============================

Versions of AppArmor that support stacking have policy syntax which
may also specify profile transitions to profile stacks. A stack is
expressed through a named transition using the **//&** stack separator.

Discrete exec (**px**, **Px**, **cx** and **Cx**) rules can be used
to directly transition to a profile stack. Inherit exec (**ix**)
rules may *not* be used to transition profile stacks.

Direct stack transition
-----------------------

```
# specify exec transition to a stack
/bin/** px -> one//&two,
```

The above rule specifies to transition to the stack **one//&two**
without regard to the current profile or current profile stack (ie,
the current profile is not added to the stack and the current stack
isn't part of the transitioned to profile).

The above rule may also be written equivalently with the newer leading
permissions syntax:

```
# specify exec transition to a stack with leading permissions syntax
px /bin/** -> one//&two,
```

Direct stack transition with current profile
--------------------------------------------

To specify a stack that includes the current profile, the
**@{profile\_name}** variable can be used. Eg:

```
# specify exec transition to a stack that includes the current profile
px /bin/** -> @{profile_name}//&one//&two,
```

Relative stack transition
-------------------------

Discrete exec rules also support specifying stacks relative to
the profile transition in order to allow stacks to be based on
the executable and profile transitions instead of just the current
profile. Relative stacks indicate that the named transition should
combined with an existing transition. Eg:

```
 # specify exec transition to a stack relative to the profile transition
 /bin/** px -> &two,
```

The above rule computes the profile transitions for **/bin/\*\* px,**
and then stacks **two** on top of that transition. More specifically:

```
  profile one {
    /bin/** px -> &two,
  }
  profile foo /bin/foo {
    ...
  }
  profile bar /bin/bar {
    ...
  } 
```

When a process running under **one** exec()s **/bin/foo**, /bin/foo
runs under the stacked profile **foo//&two**. When a process running
under **one** exec()s **/bin/bar**, /bin/bar runs under the stacked
profile **bar//&two**.

The relative stack syntax also supports the profile transition with
fall back (ie pix and pux), where if a target profile transition
is not found either the current profile or unconfined is used as a
fallback. Eg:

```
# specify exec transition to a stack relative to the profile transition
/bin/** pix -> &two,
```

Note: earlier versions of AppArmor supported 'ix' rules with stack
transitions. This feature is obsoleted. When transitioning, please
consider these discrete rules instead:

-   */bin/\*\* px -&gt; @{profile\_name}//&foo* (stack 'foo' on top of @{profile\_name})
-   */bin/\*\* px -&gt; &foo* (stack 'foo' on top of existing profile in /bin/\*\*)
-   */bin/\*\* px -&gt; &@{profile\_name}//&foo* (stack 'foo' on top of @{profile\_name} and existing profile in /bin/\*\*)

Evaluating stacking transitions when a stack is applied
-------------------------------------------------------

Stacking transitions may appear fairly complicated when a task is
already confined by a stack but calculating the the final transition
is straightforward so long as the stack is evaluated per profile.

Eg, consider the following profiles that define transitions:

```
 profile A {
   px /bin/** -> C//&D,
 }
 profile B {
   px /bin/** -> &C,
 } 
```

when an application running under the stacked policy A//&B exec()s
/bin/foo, the permissions for both A and B are checked:

-   the exec rule in profile A allows the exec and specifies the transition to **C//&D**
-   the exec rule in profile B allows the exec and specifies the transition to **/bin/foo//&C**.

Therefore the resulting confinement of /bin/foo are the two stacks
combined to the stack **/bin/foo//&C//&C//&D** which is reduced to
the final stack **/bin/foo//&C//&D**.

Evaluating stacking with profile variables
------------------------------------------

**TODO**: needs proof-reading from jjohansen

As mentioned, the resulting policy of stacked profiles is the
intersection of the profiles. When considering rules with the
'@{profile\_name}' variable, it is very important to understand that
the value is a *compile-time* value, and not a runtime value. Consider
the following profiles:

```
foo {
  ptrace (readby),
  ptrace (tracedby),
  ptrace (read) peer=@{profile_name},
  ptrace (read, trace) peer=bar,
  /bar/** ixr -> &bar,
}

bar {
  ptrace (readby),
  ptrace (tracedby),
  ptrace (read) peer=@{profile_name},
  ptrace (readby, tracedby) peer=foo,
}
```

When 'foo' exec()s 'bar', the exec transition 'bar' runs under is
'bar//&foo', not 'bar'. Therefore, 'foo' can:

-   be readby anyone
-   be tracedby anyone
-   read itself
-   read and trace foo//&bar

Note that 'read and trace foo//&bar' is not an implicit rule. Because
'foo' can read itself via the '@{profile\_name}' rule and we have an
explicit read rule for 'bar', then because 'foo' has access to 'foo'
and has access to 'bar', then must have access to the subset of 'foo'
and 'bar' (ie, 'foo//&bar').

'bar//&foo' can:

-   be readby anyone \# therefore, by foo
-   be tracedby anyone \# therefore, by foo

Importantly, 'bar//&foo' *cannot* read itself because there is no rule
'ptrace (read) bar//&foo' in 'bar' because '@{profile\_name}' at the
time of profile compilation evaluated to 'bar', *not* 'bar//&foo'.

Application directed Stacking
=============================

AppArmor provides an API for processes to change their own
confinement. This API is controlled in policy by `change_profile`
rules (see below), but is unrestricted for tasks that are unconfined.

Stacking API
------------

```
int aa_stack_profile(const char *profile);
int aa_stack_onexec(const char *profile);
```

The `aa_stack_profile()` is analogous to the `aa_change_profile()`
function, if successful it provides an immediate change of confinement
by stacking the requested profile(s) on top of the tasks current
confinement.

Example: given a task confined by the profile **one**

```
     if (aa_stack_profile(“two”) < 0)
        exit(1);  /* fail */
     /* now confined by **one//&two** */
```

The `aa_stack_profile()` function will accept a stacked profile label
as an argument resulting in an even deeper stack.

Example: given a task confined by the profile **one**

```
     if (aa_stack_profile(“two//&three”) < 0)
        exit(1);  /* fail */
     /* now confined by **one//&two//&three** */
```

The profiles specified in the stack must exist and be loaded into the
kernel or the stack function will fail. See *aa\_stack\_profile(3)*
for other failures.

The `aa_stack_onexec()` api function is analogous to the
`aa_change_onexec()` function in that it is applied at exec time. The
specified profile if allowed will override rule based profile
transitions and like `aa_stack_profile()` will accept stacked profile
labels as an argument. Like `aa_stack_profile()` `aa_stack_profile()`
can fail both at the api call and at exec time depending on the
*change\_profile* rules in the profile and the executable called.

Extended change\_profile API
----------------------------

```
int aa_change_profile(const char *profile);
int aa_change_onexec(const char *profile);
```

The `aa_change_profile()` and `aa_change_onexec()` functions have
been extended to support stacking. The argument can now specify a
stack of profiles to switch to.

Example: given a task confined by the profile **one**

```
       if (aa_change_profile(“two//&three”) < 0)
          exit(1);  /* fail */
       /* now confined by **two//&three** */
```

Notice that profile *one* has been dropped because
`aa_change_profile()` changes the current confinement it does not
add to it.

Introspection of confinement
----------------------------

```
int aa_getcon(char **label, char **mode);
int aa_getpeercon_raw(int fd, char *buf, int *len, char **mode);
```

The standard introspection APIs have been extended to return a
stacked label string when present. If the mode will be **mixed**
if the profiles in the stack have different modes.

aa\_stack\_profile vs. aa\_change\_profile
------------------------------------------

The `aa_stack_profile()` function can be emulated by doing

```
 aa_get_con(old);
 strcat(new, “//&”);
 strcat(new, old);
 aa_change_profile(new);
```

The same thing can be done to recreate `aa_stack_onexec()` with
`aa_change_onexec()`. However the *aa\_stack\_* versions are more
efficient as they do not need to introspect the kernel nor do they
have to build the stacked transition label in userspace.

change\_profile rules
---------------------

Like *change\_profile()* and *change\_onexec()* stacking uses
change\_profile rules to determine if a stack is allowed. To do
this the change\_profile rule has been extended to allow specifying
stacking permissions.

```
 # Allow stacking profile A
 change_profile -> &A,
 # Allow stacking the stack A//&B
 change_profile -> &A//&B,
```

To limit a stack to exec time the rule becomes

```
 # Allow stacking the profile A at exec of /bin/**
 change_profile /bin/** -> &A,
 # Allow stacking the stack A//&B at exec of /bin/**
 change_profile /bin/** -> &A//&B,
```

It is important to note that the stack request of *stack\_onexec()*
is applied against the current profile confinement and at this time
their is no way to specify the onexec stack should take place against
a computed profile transition as the policy rule using *px -&gt;
&* allows.

In addition to the relative stack specification, the change\_profile
rule also supports specifying the absolute stack

```
 change_profile -> A//&B,
 change_profile /bin/foo -> A//&B,
```

Stacking does not require that a change\_profile with a stack specifier (leading *&*) be used. Just as

```
 A {
   change_profile -> A//&B,
 }
```

allows aa\_change\_profile(“A//&B”) to transition to
the stack A//&B, it also allows aa\_stack\_profile(“B”)
because the end result of the stack A//&B is the same as if
aa\_change\_profile(“A//&B”) was called.

### secure exec

change\_onexec and stack\_onexec by default use safe exec which scrubs
several dangerous variables from the environment. It is possible
to specify that an unsafe exec needs to be done by using the unsafe
keyword in change\_profile rules.

```
 change_profile unsafe /bin/foo -> bar,
```

Note: that the unsafe keyword requires the exec portion (*/bin/foo*
in the example above) of the change\_profile rule to be present.

### stacks and sets of change\_profile rules

The above rules allow fully specifying a stack however stacking is
also allowed if the set of profiles within a stack are individually
allowed. That is if profile contains.

```
 change_profile -> A,
 change_profile -> B,
```

then

```
 aa_change_profile(“A”);
 aa_change_profile(“B”);
 aa_change_profile(“A//&B”);
```

are all allowed, because A//&B is a subset of profile A and of
profile B.

At this time the transition is only allowed if the full set of profiles
in the stack is allowed. That is to change to the stack A//&B rules
allowing the change to both profile A, and profile B are required. Even
though the stack A//&B is strictly a subset of the permissions in
profile A, and is also a subset of the permissions in profile B.

At this time it is recommended to use tight change\_profile rules
to control stacking, in the future the controls may be loosened to
allow more flexibility around stacking.

Application Directed transitions and Stacks
-------------------------------------------

When a task is confined by a stack of profiles and it
requests to change its confinement via any of the API functions
(*aa\_change\_profile*, *aa\_change\_onexec*, *aa\_stack\_profile*,
*aa\_stack\_onexec*) then transition must be allowed by all the
profiles in the stack in view and with replace all profiles in the
stack in view.

Example: if a task is confined by A//&B, and the task requests to
aa\_change\_profile(“C”)

Given:

```
   A {
      change_profile -> C,
   }
    B {
      change_profile -> F,
   }
```

then the request will fail because B does not allow changing to profile C.

Given

```
   A {
      change_profile -> C,
   }
    B {
      change_profile -> C,
   }
```

then the request will succeed and the stack of A//&B will be replaced by profile C.

Example: if a task is confined by A//&B, and the task requests to
aa\_change\_profile(“C//&D”)

Given

```
   A {
      change_profile -> C//&D,
   }
    B {
      change_profile -> F,
   }
```

then the request will fail because B does not allow changing to
neither C nor D.

Given

```
   A {
      change_profile -> C,
   }
    B {
      change_profile -> C,
      change_profile -> D,
   }
```

then the request will fail because A does not allow changing to profile D.

Given

```
   A {
      change_profile -> C//&D,
   }
    B {
      change_profile -> C,
      change_profile -> D,
   }
```

then the request will succeed and the stack A//&B will be replaced by C//&D

Low level interfaces
--------------------

If access to libapparmor is not available then the low level
interfaces may be accessed directly. This will require direct access
to the proc file system or in the case of peer creds the getsockopt
function. Please note that correct policy rules allowing the stack
will still be required, you will have to be careful to handle any
errors and separate the mode from the label. Use of libapparmor is
the only way to ensure compatibility between versions.

To stack against current confinement

```
echo -n “stack profile_a” > /proc/<tid>/attr/current
```

To stack against confinement at exec time

```
echo -n “stack profile_a” > /proc/<tid>/attr/exec
```

To change current confinement

```
echo -n “changeprofile profile_a” >/proc/<tid>/attr/current
```

To specify confinement to change to at exec

```
echo -n “changeprofile profile_a” >/proc/<tid>/attr/exec
```

To find the current confinement

```
cat /proc/<tid>/attr/current
```

To find the confinement specified to be used at exec

```
cat /proc/<tid>/attr/exec
```

To find a peer’s confinement

```
getsockopt(fd, SOL_SOCKET, SO_PEERSEC, buf, &optlen);
```

Threading and Self Directed Transitions
---------------------------------------

The above examples used **<tid>** to indicate that the kernel
thread id should be used. Under most circumstances using
the **/proc/self/attr/...** symlink is sufficient. However
in multi-threaded processes this can result in a failure. In a
multi-threaded application the thread must write the proc file that
matches its thread id, not the pid of the process.

Writing to the wrong proc file is not the only source of failure that
can occur when a multi-threaded application is using self directed
transition. Self directed transitions only apply to the thread that set
them. So if a thread successfully sets a transition, it only applies to
that thread, other threads with-in the process will remaine confined
by the old confinement. This can lead to weird transitions failures
as transitions become dependent on which thread does the exec.

Due to these issues AppArmor may reject self directed transitions of
multi-threaded processes unless policy allows them.

AppArmor provides a special change\_instance api for dealing
applications that need to transition multiple threads or processes.

Additional Information
======================

Introspection and Profile Mode
------------------------------

In addition to the profile name the profile mode is available
when using aa\_getcon() and aa\_getpeercon() api functions to do
introspection on a process. In the case of stacking if the mode is the
same for all profiles in the stack it will displayed as normal. That is
if all profiles are in enforce mode then the mode will be **(enforce)**
however if not all profiles in the stack have the same mode then the
mode will be reported as **(mixed)**.

The true mode of each profile can be looked up via the apparmor/policy/
directory under securityfs.

Audit Logging
-------------

Under most situations stacks are logged on a per profile basis, so
that logging only occurs for the portion of the stack that triggers
the denial or audit message.

E.G. Given a stack of A//&B then

If A allows a permission request and B denies it the there will be
a log message for the denial from B but no message for A.

If both A and B deny the permission request then there will be two
audit messages in the log one with one message with profile=A and
another message with profile=B.

In neither case is the profile=A//&B.

In a few cases it is possible that the stack will be logged especially
when reporting an object label or peer. It is also possible that a
userspace trusted helper (dbus, xserver) will log messages for the
entire stack instead of a message per profile.

Stacking with unconfined
------------------------

Stacking against the unconfined profile does not result in reducing
the stack to the profile(s) stacked against unconfined.

I.E., unconfined & A does not reduce to just A but instead results in
the stack unconfined//&A. While this may seem strange behavior this
allows for setting up a stack where both the transition for unconfined
and the stacking profile are applied.

E.G., supposed we have a process in the unconfined state and we have
the following profiles

```
  profile /bin/example {
     ...
  }
  profile A {
     /bin/example px -> B,
     ...
  }
  profile B {
     ...
  }
```

the call aa\_stack\_profile(A) on the unconfined process results in
**unconfined//&A**, which at exec time turns into **/bin/example//&B**.

Seccomp and no\_new\_privs
--------------------------

If seccomp is in use and no\_new\_privs is set then most
domain transitions are blocked as they could increase task
permissions. Stacking transitions that add to the current confinement
are still allowed as they strictly reduce the set of permissions
that are available. More generally any transitions that is provably
at the profile level a subset of the current confinement is allowed
(that is to say rules with in the profiles are not considered, only
the profile sets).

So when confined by A a transition to A//&B is allowed, as A//&B has
a subset of permissions allowed in A.

However if confined by A a transition to B//&C is NOT allowed even
if the resultant permissions of B//&C are a subset of A because the
subset is not provable based only the profiles in the transition.

Open file handles and memory maps
---------------------------------

Like aa\_change\_profile, open file descriptors, and memory maps may
not be remediated after a call to aa\_stack\_profile() so the calling
program should close(2) open file descriptors or ideally call exec()
to clear out process state after calling aa\_stack\_profile().

To be clear if an immediate transition (aa\_change\_profile,
aa\_stack\_profile) is used apparmor will revalidate file descriptors
on access. However there are situations that AppArmor is not capable
of revalidating, e.g. a process accessing a memory mapped file via
direct memory access. The only safe way to deal with these situations
is to either audit the process to ensure all such resources have been
closed or to create a new process via exec().

Implementation efficiency, Static vs Dynamic computation
--------------------------------------------------------

The permission check on the stack and the cross check in particular
can result in multiple sub permission checks. This is not necessarily
what is done by AppArmor at run time. The examples provide the step
by step break down of the checks for clarity. The AppArmor compiler
is free to pre-compute the results of different stacks when policy
is compiled together so that a set of checks can in fact be satisfied
by a single permission lookup.

Precomputing the stack into a static policy for run time can result
in slower compiles an larger loaded policy size but it can reduce
the runtime cost to a single permission check, the same as a single
profile.

Stacking Limitations
====================

Stacking in 3.5-beta1 is limited to crossing 1 policy namespace
boundry, with the limitation that when the current namespace of
the task is NOT the system policy namespace then the current user
namespace must not be the system user namespace.

This limitation is in place because AppArmor uses the CAP\_MAC\_ADMIN
capability to help control who can manage policy in the current
namespace. However the Linux kernel does not yet have hooks enabling
AppArmor to follow or control user namespaces. Thus it is possible
for a user to create a user namespace that has capability MAC\_ADMIN
and not transition or stack a policy namespace, thus giving the user
the ability to manage the system policy namespace.

Once proper controls around user namespaces are established this
limitation can be lifted.

References
==========

```
 aa_stack_profile(2)
 aa_change_profile(2)
 aa_getcon(2)
 apparmor.d(5)
```
