WARNING
=======

This document is a wip and is not currently supported in any released
version of apparmor

Introduction
============

The user conditional is similar to the owner conditional, except that
instead of being conditional upon the objects user id it allows making
rules and profiles conditional upon the confined tasks (subjects)
user id.

The user condition is expressed via

```
 user=<user>|'('<user list>')'
 user!=<user>|'('<user list>')'
```

and can be used as a block conditional

```
 user=<cond> {
   ...
 }
```

User conditional in rules
=========================

```
 user=alice capability,
```

negative user conditionals
--------------------------

```
 user!=(alice,bob),
```

communication
-------------

```
 ptrace peer=firefox,

 ptrace peer=(label=firefox user=alice),
```

User conditional on profiles attachments
========================================

Creating profiles doing

```
 profile firefox {
    user=alice {
       ...
    }
    user=bob {
       ...
    }
 }
```

Is not always convenient, some times it is cleaner to specify them as a profile attachment conditional.

```
 profile firefox user=alice {
    ...
 }
 profile firefox user=bob {
    ...
 }
```

doing this can be more convenient and is functionally equivalent. It
does not mean there are multiple versions of the profile just different
[variants](profile_variants) based on the attachments.

>  The user conditional is not part of profile name and does NOT affect
>  any communication rules based off of profile name.  it is an attachment
>  conditional that implicitly asserts all rules in the profile have
>  the user= conditional applied.

multiple versions of a profile with different user conditions
-------------------------------------------------------------

```
 profile firefox {}
 profile firefox user=alice {}
 profile firefox user=bob {}
```

A profile with different versions based off of the attachment conditional (including user=) are known as variants. Profile variants with a user conditional are considered to be more specific than profiles with out a user attachment conditional and will be preferred when they match.



# Profile replacement

When user conditionals are used they only replace profiles that have the same user conditional. This means that if you change the profile user attachment conditional to include an extra user it will not replace a previously loaded version of the profile. This means it possible to have both the old and new versions of the profile present if replacement is not done carefully.

It is recommended that all profiles defined with user conditionals be
compiled and loaded together at the same time. This allows the compiler to ensure that
there are no inconsistencies between the conditionals and all variants
are replaced atomically. If this guideline is not being followed then
it is recommended that all variants have none over lapping attachment
conditionals, otherwise it is hard to make guarantees can be made about which profile
variant will be attached.

# Profile Variants and change_hat

profile variants when used with change_hat can result in different hats being attached based on the user invoking the change_hat call.


profile variants with overlapping profile conditions
----------------------------------------------------

When multiple profile variants are specified it is possible some of
the attachment conditionals will overlap. These types of profiles
need to be compiled together so that the overlaps can be checked and
a proper ordering for the attachments can be found. The ordering
for attachments is the same as used for exec rules where the most
specific match wins, but if two attachments overlap and the overlap
does not result in a single subset the compile will be failed.

For purposes of figuring out specificity negated attachments like

```
 user!=alice
```

is not very specific and results in a fairly broad match

combining profile and rule conditions
=====================================

It is possible to specify both profile and rule conditions.

```
profile firefox user!=(alice,bob) {

 user=fred capability,

}
```

However care must be taken as it is possible to create rule sets that
will not be applied because the intersection of the profile condition
and the rule condition is empty.

Equivalence of user conditional on profiles and rules
=====================================================

The specification of user conditionals on the profile and in the
rules are functionally equivalent. They are merely a different way
of expressing the same thing. The profile based conditional does have
the additional property that it affects profile attachment conditions.

user conditionals and uids
==========================

User conditionals require that apparmor policy be aware of user to
uid mappings. AppArmor will delay doing this mapping until policy load
time but the user information for policy will need to be regenerated
for if any user name is mentioned in policy and has its system mapping
information changed.

AppArmor may not pickup new user information unless it is also added
to the policy. This does not usually result in a problem as such uid
are not being directly matched by policy.

precompiled policy and uids
===========================

Where possible apparmor will use a special kernel variable table
for user/uid mappings for matches so that policy can be precompiled
and applied to different systems where the user uid info may be
different. The special mapping table is not stored as part of the
precompiled policy and automatically generated on policy load.

user conditionals and user namespaces
=====================================

user namespace mapping will be applied to policy where appropriate,
when an apparmor policy namespace is associated to the user namespace.
