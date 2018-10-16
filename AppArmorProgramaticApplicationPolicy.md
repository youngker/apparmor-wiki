WARNING
=======

This document is a work in progress and requires at a minimum the
development version of apparmor 4.x???

Requirements
============

-   AppArmor Kernel module - 4.x??
    - scope & view work
    - namespace based policy accounting
    - application policy api
    - auto cleanup policy
    - policy management rules
      - namespace and profile
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

# 0. Mediation of application policy
- needs to be allowed by the namespace
  - is accounted to the namespace
- needs to be allowed by any policy for the namespace scope
  - profiles can have rules to allow or disallow policy management

# 1. Checking for AppArmor support

api call
- high level
- low level
- checks kernel support and permission to load application policy

# 2. Creating Policy

- lives in ns of current_ns
  - can load multiple profiles if using child profiles

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

5. Interaction with other policy in the namespace

- several rule types specify the identity that a profile can interact with
  - this can prevent policy from interacting with application policy
  - need set name for application profile OR kernel var
  - applications may want to name their profiles for interaction
- want to be able to interact with policy
  - other wise could not block communication with some services.
  - wants visibility to interact
    - problem system policy is not necessarily designed to interact with user/application policy
      - could handle with globbing rules


??? using policy ns vs. not
---------------------------
