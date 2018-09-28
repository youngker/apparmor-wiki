- aa_ : prefix is used to namespace apparmor fns that are visible.
- __XXX: leading underscores indicates fn needs to be used within locking or other special conditions
- AA_BUG(): is used in new code to check (when debug is one) and document preconditions. Not all code has been converted to using this convention.


# Layout
AppArmor is largely self contained in
- security/apparmor/

Other locations
- include/linux/lsm_audit.h:
- include/uapi/linux/xattr.h
- security/Kconfig
- Documentation/admin-guide/LSM/apparmor.rst
- Documentation/admin-guide/LSM/index.rst
- Documentation/admin-guide/kernel-parameters.rst
- Documentation/process/3.Early-stage.rst

```security/apparmor/```
- apparmor.h: class mediation type allocation, lsm.c global vars
- lsm.c: LSM hook registration, base hook fns used in the registration and general module init. Base hook fns should be generally be static and in lsm.c

core policy handling
- policy.c, include/policy.h: profile management
- label.c, include/label.h: basic label management
- policy_ns.c, include/policy_ns.h: apparmor policy namespaces, not other system namespaces

interfaces
- policy_unpack.c: unpack and translate serialized policy to kernel internal format
- apparmorfs.c, include/apparmorfs.h: fs based api, policy, and introspection interface
- procattr.c: interface for proc file cb, will probably get rolled into apparmorfs.c at some point

support fns..
- audit.c, include/audit.h: audit layer. Also used for complain and prompt interfaces
- lib.c, include/lib.h, include/perms.h: helper fns() base permission lookup
- match.c, include/match.h: state machine unpack and matching
- crypto.c: hash generation for policy files
- path.c: path lookup fns
- secid.c: infrastructure support for mapping secids to labels

special none C files
- nulldfa.in: precompiled dfa used in null and unconfined profiles so that we don't need conditional checks on whether the dfa exists.
- stacksplitdfa.in: precompiled dfa used in label parsing

Mediation
- capability.c, include/capability.h: mediation of capability set 
- domain.c, include/domain.h: mediation of exec transitions, and change_profile, ...
- file.c, include/file.h: mediation of file objects
- ipc.c, include/ipc.h: ptrace and signals
- mount.c, include/mount.h: mediation of mount, pivot root
- net.c, include/net.h: base socket mediation and support routines for fine grained mediation
- task.c, include/task.h: task related mediation and storing off of state for nonewprivs, change_hat, change_onexec
- resource.c, include/resource.h: mediation of rlimits, and also setting rlimits to profile defined values


# apparmorfs: userspace interface, introspection and api
- virtualized policy/ directory
  - jump link trick
- aa_fs - common fn() between apparmor security fs and special virtualized apparmorfs
- aa_sfs - apparmor file in securityfs
- aafs - files in custom apparmorfs used to virtualize policy/

init
- hook init
- fs init
  - hangs off of securityfs


context
cred
task


hooks

critical_sections - accessing the task cred and labels
-

preallocated buffers
- apparmor preallocates some large percpu work buffers for path lookups
  - thy are accessed with get_buffers() and put_buffers() macros
  - the macros setup a no-preempt critical section


label
- vector of labels

aa_proxy
- used to proxy profile replacement

permission mapping

file dfa
policydb

.null

# permission checks, caching and revalidation

# stacking

# Delegation
- TODO: this is not implemented yet.
