# Related Documents
- [Compiler Development Guide](apparmor_compiler_development_guide)
- [Library Development Guide](apparmor_library_development_guide)
- [Profile Tool Guide](apparmor_profile_tools_guide)
- [Kernel Development Guide](apparmor_kernel_development_guide)
- [Policy Development Guide](aparmor_policy_development_guide)

# General code notes
- aa_ : prefix is used to namespace apparmor fns that are visible.
- __XXX: leading underscores indicates fn needs to be used within locking or other special conditions
- AA_BUG(): is used in new code to check (when debug is on) and document preconditions. Not all code has been converted to using this convention.


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

### security/apparmor/ overview
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
- ipc.c, include/ipc.h, include/sig_names.h: ptrace and signals
- mount.c, include/mount.h: mediation of mount, pivot root
- net.c, include/net.h: base socket mediation and support routines for fine grained mediation
- task.c, include/task.h: task related mediation and storing off of state for nonewprivs, change_hat, change_onexec
- resource.c, include/resource.h: mediation of rlimits, and also setting rlimits to profile defined values

# LSM
The is infrastructure that operates on kernel objects, at a deeper level in the kernel. It does not provide syscall filtering (provided by seccomp). It is possible to combine the two but apparmor does not at this time.

The LSM provides a set of hooks
- see include/linux/security.h and include/linux/lsm_hooks.h
- see security/apparmor/lsm.c apparmor_hooks[]
and blobs  via a security (void *) field off of several kernel object (inode, file, sock, superblock,...).

Calls to the hooks are spread through out the kernel, called using
- security_XXX.

int return errcode

stacking

updating state vs permission check


# task labeling
The task's label is stored off of the task's cred security blob, not the task security blob. In older versions of apparmor the data stored in the task security blob was also stored in the cred security blob in addition to the label, and there was no task security blob.

Except in a few special cases NEVER directly use the cred's label. Doing so could result in using a STALE label, that can lead to strange problems and bug reports.

## critical sections
Instead use
  - task context: begin_label_crit_section/end_label_crit_section
  - atomic context: __begin_label_crit_section/__end_label_crit_section

```NEVER``` update the task's label inside of a label_crit_section.

## changing the task label
  - ensure you are NOT in a label critical section
  - get a reference count on the task's label (aa_get_current_label())
  - update the tasks label via updating the cred (aa_replace_current_label())
  - put the reference count when done with the label


# profile replacement and label update
A task's label can only be updated by it self. This means profile replacement proceeds in two phases, updating the profiles and labels, and updating the task's label. Because locking over the entire update process would be very detrimental to the kernel profile replacement is not expected to be atomic but that it will complete within a reasonable time window.

- when a profile is replaced
  - it is unpacked and verified outside of locks, only once the profile is ready are locks taken
  - a round of lookups and fs creation is done, prepping for actual replacement
  - only once all the setup that could fail is done does actual replacement take place
  - its aa_proxy is updated to point to the new profile.
  - its STALE flag in its label is set.
    - The STALE flag is updated without lock. This is okay because its a one way transition and the only flag in the set that can change at run time, so even if there is racing updates, the correct value will always be written.
  - the task doing the profile replacement then mutex locks the ns label tree and begins walking the labels updating compound labels that contain the replaced label, updating their proxies and marking them stale

- when a task enters a hook
  - it checks if its label is STALE at the beginning of the critical section
    - if the label is stale it will
      - update the task if it can (begin_label_crit_section())
      - or get a refcount to the updated label (__begin_label_crit_section())
- when a task leaves the critical section it will put the label refcount if one was taken.

It is possible that a tasks label will not be updated for some time if the task does not enter LSM hooks where the label can be updated.

This technique is used because profile replacement is expected to be infrequent compared to LSM hook entry and it is relatively expensive to do atomic operations. As long as there are thousands of hook entries between profile replacement, it is worth skipping the atomic operation.

# task's policy namespace
The policy namespace to user for a task is determined by its label. Use
- aa_get_current_ns()

# userspace interface, introspection and api
The userspace interface is split between procfs and securityfs.

## /proc/<pid>/attr/*

## securityfs (/sys/kernel/security/apparmor/)

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

# permission checks
- caching
- revalidation
- revocation
- type calculation


# stacking

# Delegation
- TODO: this is not implemented yet.
