# Related Documents
- [Compiler Development Guide](apparmor_compiler_development_guide)
- [Library Development Guide](apparmor_library_development_guide)
- [Profile Tool Guide](apparmor_profile_tools_guide)
- [Kernel Development Guide](apparmor_kernel_development_guide)
- [Policy Development Guide](aparmor_policy_development_guide)

# Disclaimer
- This assumes you know about the basics of apparmor policy and enforcement ???link
- This document probably lags the code
- This document is a general overview, and some hints and tips. It can't cover everything the code does.

# ```General code notes```
- aa_ : prefix is used to namespace apparmor fns that are visible.
- __XXX: leading underscores indicates fn needs to be used within locking or other special conditions
- AA_BUG(): is used in new code to check (when debug is on) and document preconditions. Not all code has been converted to using this convention.


# ```Layout```
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

### security/apparmor/ directory overview
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

# Configuration
If building go with default value for configuration items. 

AppArmor needs to be enabled from the security menu (security/Kconfig). And configured via the apparmor menu (security/apparmor/Kconfig).

## Configuration conditional code

AppArmor code does NOT ifdef access to certain fields that are ```ifdef```ed within general kernel code. This is because these fields are required by apparmor and those conditionals are guarenteed to be selected. Specifically apparmor depends on

```
	SECURITY
	NET
```

and will select
```
	AUDIT
	SECURITY_PATH
	SECURITYFS
	SECURITY_NETWORK
```

The Crypto code is ifdefed by SECURITY_APPARMOR_HASH_DEFAULT which if enabled will select 
```
	CRYPTO
        CRYPTO_SHA1
```

New code that have conditional dependencies should use ifdef instead of select for no core mediation. Eg. secmark, netfilter, keys code should all be wrapped in ifdefs.


# ```LSM```
The LSM is infrastructure that operates on kernel objects, at a deeper level in the kernel. It does not provide syscall filtering (provided by seccomp). It is possible to combine the two but apparmor does not at this time.

The LSM provides a set of hooks
- see include/linux/security.h and include/linux/lsm_hooks.h
- see security/apparmor/lsm.c apparmor_hooks[]

and blobs via a security (void *) field off of several kernel object (inode, file, sock, superblock,...). The blobs should always be accessed by accessors macros (in older kernels the blobs can't be shared but LSM stacking is making is sharing them and requires access macros).

Calls to the hooks are spread through out the kernel, called using
- security_XXX.

## return no value.
Security hooks return either

  void security_XXX()

for hooks that can not fail, or int

  int security_XXX()

for hooks that can fail where the return int is 0 for success and ```-ERRNO``` on failure.

## lsm hooks and state update

Because the LSM allows at least minor stacking, it is important to not just update apparmor's internal state on any hook because another LSM or DAC may cause a denial after the apparmor permission hook is called. Consult the LSM documentation for the hooks where state updates can be performed.


# ```task labeling```
The task's label (domain type) is stored off of the task's cred security blob, not the task security blob.
Except in a few special cases NEVER directly use the cred's label. Doing so could result in using a STALE label, that can lead to strange problems and bug reports.

## pre 4.13
  ```task -> cred -> security (task_ctx *) -> profile```

## 4.13 - 4.16
   ```task -> cred -> security (task_ctx *) -> label```

## 4.17
  ```task->cred->security (label *)```

4.17 removed the domain label from the task_ctx and moved the remain parts of the task_ctx to the task->security blob,


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


# ```profile replacement and label update```
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

# ```Profiles, Labels, proxies, and namespaces```

## Profile
Profile is a name (identity), a set of rules (authority) and an optional attachment (rules about where the profile can be applied).

The profile is currently the base unity of mediation. Every profile is a label, and has a label embedded within it that points to it self.

The profile currently exists on two structures. Its parent (either a namespace, or a profile) list and the namespace label set tree. The profile list provides lockless lookup, label tree is not yet lockless read.

The plan is to remove the label list eventually.

Profiles are NOT removed from the label tree automatically because their ref count does not normally go to 0. They are manualy removed on profile replacement/removal when their replacement is inserted.

Profiles are freed in RCU callback.

## Label (Domain Type)

The label is the domain type. It provides dynamic composition, of profiles. It was brought into the upstream kernel in 4.13 but existed in ubuntu kernels for longer.

### kernel 4.13+
The label is a vector of profiles. It is the means by which apparmor does dynamic policy composition (stacking and delegation). Once created it is read only except for its stale flag, which can be flipped one way, and its refcount.

Labels exist in the tree when valid.

Labels are sorted and stored in a Canonical form.

Labels are expensive to build, vector allocations are kept down by using a small stack based vec for smaller labels, and short circuiting in code when possible.

Labels can expire without being invalid, as tasks or objects referencing them cause their refcount to go to 0. Labels will be automatically removed from the tree in RCU call back if they were not removed before.

If a label refcount is already held it is safe to use
- aa_get_label()

it a refcount is not held
- aa_get_label_rcu()

should be used. There are only a couple places where this should be needed (like accessing the proxy).

### ??? Future kernels

The label will be moving away from a simple vector of profiles to more organized, tagged arrays. All profiles within a policy namespace will be grouped into a sublabel, and delegation will see the same treatment.

This is a wip. And has not landed yet. This note is here to encourage use of the provided fns, and macros to make switching over easier.


## aa_proxy

The proxy is an object to help with profile/label replacement, it keeps a reference to the most recent version a profile/label. It does cause a circular reference on profiles/labels that have not been replaced. This has to be manually broken by profile replacement at this time (there are plans to change this).

The proxy has a special refcount, that can point to a label/profile thats label refcount has gone to 0. Use aa_get_label_rcu() to access.


## policy namespaces

The policy namespace to user for a task is determined by its label. Use either
- labels_ns if you have a reference on the label
- aa_get_current_ns() to get the current task's ns.

scope

view

# ```Domain Transitions```

## pre 4.13

Domain transitions pre 4.13 are fairly straight forward. The task's confinement is a single profile, refcounted from the apparmor task_ctx struct off of the the task's cred.

  ```task -> cred -> security (task_ctx *) -> profile```

The confining profile can transition to single profile, either the same or something different dependent on the profile rules.

| Profile |  |Transition |
|---------|--|-----------|
| A | -> | D |

### no new privs (nnp)

When seccomp nnp is set, it affects what domain transitions can occur. Specifically exec time inheritance of the current profile is allowed but regular profile transitions are blocked. The only transition that is allowed is from ```unconfined``` to any profile as this always results in less privileges.

## 4.13 - 4.16
AppArmor replaced the profile* stored in the task_ctx stored off the cred with a label*.

   ```task -> cred -> security (task_ctx *) -> label```

Domain transition now consist of building a new label. To do this we walk each profile in the cred label and they can each have a transition

| Profile |  |Transition |
|---------|--|-----------|
| A | -> | D |
| B | -> | B |
| C | -> | A&C |

the transition is usually to a single profile (actually label of the profile) but it can be to a label
stack, C -> A&C above. This allows setting up stacks etc.

The label computation takes and place each transition into a vector (label*[]). The build macro takes the transition vector and finds a matching label in the label tree or builds a new label out of it.

So for the above example the built label would be

  A&B&C&D

### no new privs (nnp)

??? subset test

## post 4.17
In 4.17 the confinement label was split from the task specific tracking information (change_hat, onexec, ...). The label becomes directly referenced by that cred->security field while that task_ctx is now referenced by the task's security field.

  ```task->cred->security (label *)```

  ```task->security (task_ctx *)```

Domain transitions are basically the same as in 4.13 except now they check the task_ctx for change_hat, change_onexec and nnp, and update the cred->security field with a label directly.

### no new privs (nnp)

In addition the task_ctx picked up a new nnp field

the task->security->nnp field is also a label but it might not be the
same as the task's cred because we are already allowing some limited
transitions.  The current restriction is that nnp must be a subset of
the current label so if the current label is A&B&C, nnp might be A&C,
but won't be A&D.
nnp field


## 4.21

### no new privs (nnp)

nnp override



# Permission Computation
When computing permissions it is almost always done and on a per profile level, and audited at on a per profile level.

Profile based callback fns are used to provide a per profile access with special macros used to handle permission composition.

While not all code has been converted to the following pattern where possible the new pattern should be used as it will make it easier to land delegation when it is ready.

## Label access patterns
???
There are several macros to help with writing 


## permission pattern

The permission pattern is
- lookup permission in dfa
- apply run time modes to dfa perms
- run the permission check, and audit (if the audit structure is defined)

An example code snippet

	// permission lookup
	state = aa_dfa_next(profile->policy.dfa,
			    profile->policy.start[AA_CLASS_SIGNAL],
			    aad(sa)->signal);
	aa_label_match(profile, peer, state, false, request, &perms);
	
	// apply modes to the permissions
	aa_apply_modes_to_perms(profile, &perms);
	
	// perform the permission check and audit if needed
	return aa_check_perms(profile, &perms, request, sa, audit_signal_cb);

## dfa access patterns

The dfa access pattern is how permissions are encoded in the dfa

start with a class

order - and

alternation - or

permission on each state
????

# ```userspace interface, introspection and api```
The userspace interface is split between procfs and securityfs.

## <code> /proc/<pid>/attr/* </code>

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

# Notifications

Notifications are based on apparmor audit messages. However instead of being converted to a text format they are passed in machine native binary structure.

profile flags
rule flags

multiple notification
- profile loads
- per profile in stack

apparmor audit msg -> audit msg cache -> dedup -> copy msg -> insert in msg cache -> send to audit subsystem
                   |                          |            |
                   |                           -> drop msg |
                   |                                        -> send to audit subsystem
                   -> notify queue -> insert queue on waiting

multicast
- policy changes
- complain messages
prompting
- synchronous
- asynch + ptrace and restart

prompt failing
- interrupts
- time outs

how to keep alive
restarting request

down grading prompt

## ???
audit structs


## user space api

### include file

### setting up notifications

open file
use ioctl to set notification
wait for notifications
read file
- reply

link to userspace api provided by library.

## ???





# Delegation
- TODO: this is not implemented yet.
