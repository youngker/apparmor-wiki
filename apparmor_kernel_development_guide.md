# Layout
AppArmor is largely self contained in
- security/apparmor

aa_ : prefix is used to namespace apparmor fns that are visible.
__XXX: underscores indicates fn needs to be used within locking or other special conditions
AA_BUG(): is used in new code to check (when debug is one) and document preconditions.

- lsm.c: LSM hook registration, base hook fns used in the registration and general module init. Base hook fns should be generally be static and in lsm.c

- match.c & include/match.h: state machine

- policy_namespace.c: apparmor policy namespaces, not other system namespaces

- foo



lsm.c

apparmorfs
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

