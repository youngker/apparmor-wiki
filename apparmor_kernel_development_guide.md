layout

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

