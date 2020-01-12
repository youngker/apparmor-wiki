WARNING
=======

This document is a work in progress and requires at a minimum the
development version of apparmor 4.x???

Requirements
============

-   AppArmor Kernel module - 4.x??
-   AppArmor Userspace - 4.x???

# Related Documentation

-   [Stacking profiles in AppArmor](AppArmorStacking)
-   [AppArmor Policy Namespaces](AppArmorNamespaces)
-   [Confining Users with Stacking](StackingConfiningUsers)
-   [Applying system wide restrictions in AppArmor](AppArmorSystemWideRestrictions)
-   [User defined policy in AppArmor](AppArmorUserDefinedPolicy)
-   [Programatic application policy in AppArmor](AppArmorProgramaticApplicationPolicy)
-   [Putting it all together](AppArmorPolicy)

# Introduction


If allowed by policy users can create their own profiles that will be applied against the applications run by the user. User defined policy when used does not replace system policy, instead it compliments it and is applied at the same time through [stacking](AppArmorStacking).

1. ???
======

creating and entering the namespace

## system policy controlling user policy namespaces

System policy controls whether a user can create a policy namespace, what resources it can use and how it interacts with system policy. Specifically policy in the current namespace will control whether the user can load policy and any resources used will be accounted against the current namespace.


# User owned namespace

User defined policy is stored in a namespace with limits defined by system policy. The namespace will be owned by the user and refcounted so that it will exist as long as a user task is running.

??? auto create and enter, vs. having the ns setup and already entered ???

## Permission to load policy

Permission to load policy into a user owned namespace requires either CAP_MAC_ADMIN (allowing a system admin to modify user policy) or the [euid](https://en.wikipedia.org/wiki/User_identifier) of the task attempting to load the policy to match the namespace's owner and any profile confining the task to allow policy admin.



user defined policy and uid changes
dropping profiles when euid is switched
