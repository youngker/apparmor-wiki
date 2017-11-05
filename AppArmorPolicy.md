WARNING
=======

This document is a work in progress and requires at a minimum the
development version of apparmor 4.x???

Requirements
============

-   AppArmor Kernel module - 4.x??
-   AppArmor Userspace - 4.x???

Related Documentation
=====================

-   [Stacking profiles in AppArmor](AppArmorStacking)
-   [AppArmor Policy Namespaces](AppArmorNamespaces)
-   [Confining Users with Stacking](StackingConfiningUsers)
-   [Applying system wide restrictions in AppArmor](AppArmorSystemWideRestrictions)
-   [User defined policy in AppArmor](AppArmorUserDefinedPolicy)
-   [Programatic application policy in AppArmor](AppArmorProgramaticApplicationPolicy)
-   [Putting it all together](AppArmorPolicy)

Introduction
============

AppArmor is a flexible security system and can be used to enforce
a variety of different confinement strategies, from enforcing
system wide restrictions, role based policy, user restrictions,
or application policy. The system provides the ability to apply any
combination of these different confinement strategies simultaneously,
allowing policy to be easily tailored for a given situation.

Types of Policy
===============

??? guides to cover the basics of creating different types of policy ???

-   Policy Basics
-   System Policy
-   Application Policy
-   Confining Users
-   Roles
-   User defined policy
-   Application defined policy

Advanced Policy
===============

-   Stacking
-   Namespaces

Setup, putting it all together
