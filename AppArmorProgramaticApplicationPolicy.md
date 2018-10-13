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
-   [Putting it all together](AppArmorStackingAndNSFullPolicy)

Introduction
============

When unprivileged policy is allowed via [stacking](AppArmorStacking)
and [policy namespaces](AppArmorNamespaces) applications can you
leverage AppArmor policy to create and manage application sandboxes, allowing tasks to voluntarily
reduce their available resources and permissions to reduce attack
surface.

# 1. Checking for AppArmor support

api call
- checks kernel support and permission to load application policy

# 2. Creating Policy

Policy is in NS of current task confinement.

api
- settup rules
  - opaque *aa_new_app_policy(const char *name)
        optional name will be combined with unique value
- add rules
     error *aa_policy_add_rules(opaque *, const char *blob, size_t len)

- finalize/compile rules
     aa_policy_create(opaque *, out *blob);

     applications can own caching to save off blob, or extend caching to support app caching

- load compiled policy



3. Loading Policy
=================
- load of application policy creates/replaces profiles & enters task into profile stack (thread check)


under hood task_ctx references the profile
- need profiles that auto cleanup
- need better policy load controls, ns rule
- need control saying application policy is allowed, ns rule may limit uid?
- need application policy to be able to block policy loads, so the policy can be locked into place
  - blocks policy at ns, but if subns entered before lock down that ns could allow loading policy






4. Switch to the application policy
===================================

-   change\_hat - allows changing back

-   change\_profile - permanent transition

-   stack - may be the only thing allowed

Setting up policy to allow application policy
=============================================

??? using policy ns vs. not
---------------------------
