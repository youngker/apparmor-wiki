
# Upstream kernel

| Kernel Version | Feature |
|----------------|---------|
| 2.6.36         | Base functionality lands upstream mediation of: <ul><li>File<ul><li>owner conditional</li><li>read, write, link, lock, mmap exec</li></ul></li><li>Execute<ul><li>px, cx, ix, ux, pix, named transitions</li><li>attachment conditional separate from profile name</li></ul></li><li>Change hat<ul><li>single hat</li></ul></li><li>Change Profile</li><li>Capability</li><li>policy namespaces created through policy load</li></ul> |
| 2.6.37         |         |
| 2.6.38         |         |
| 2.6.39         |         |
| 3.0            |         |
| 3.1            |         |
| 3.2            |         |
| 3.3            |         |
| 3.4            |         |
| 3.5            |         |
| 3.6            |         |
| 3.7            |         |
| 3.8            |         |
| 3.9            |         |
| 3.10           |         |
| 3.11           |         |
| 3.12           |         |
| 3.13           |         |
| 3.14           |         |
| 3.15           |         |
| 3.16           |         |
| 3.17           |         |
| 3.18           |         |
| 3.19           |         |
| 4.0            |         |
| 4.1            |         |
| 4.2            |         |
| 4.3            |         |
| 4.4            |         |
| 4.5            |         |
| 4.6            |         |
| 4.7            |         |
| 4.8            |         |
| 4.9            |         |
| 4.10           |         |
| 4.11           | <ul><ul><li>add <i>/sys/kernel/security/lsm</i> to enable detecting currently in use lsm</li><li>kernel parameters<ul><li>make path_max readonly</li><li>remove paranoid load parameter - all policy loads now do full checking</li></ul></li><li>speedup mediation by use of percpu buffers</li><li>add sysctl <i>/proc/sys/kernel/unprivileged_userns_apparmor_policy</i> to allow disabling user namespaces from loading policy</li><li>add query interface for extended profile <i>key/value</i> data store</ul><li>allow profile hashing to be disabled with a kconfing</li><li>policy namespaces<ul><li>add pernamespace policy interface file to directly load policy into a namespace<ul><li><i>policy/namespaces/NAMESPACE/.load</i></li><li><i>policy/namespaces/NAMESPACE/.replace</i><li><li><i>policy/namespaces/NAMESPACE/.remove</i></li></ul></li></ul></li><li>allow introspecting loaded profile data via the <i>policy/</i> directory</li><li> on exec dup2 opened files that the task won't have permission to access to a special <i>.null</i> device file</li><li>Complain mode<ul><li>support force complain flag</li><li>try to create null profiles using the exec name <i>null-EXECNAME</i></ul></li><li>feature set<ul><li> add <i>features/domain/fix_binfmt_elf_mmap</i> to enable userspace to detect the semantic change caused by <i>9f834ec18def</i></li></ul></li><li>report namespace name in audit messages</li></ul>        |
| 4.12           |         |
| 4.13           | <ul><li>add v7 abi</li><li>speedup path lookups with preallocated buffers</li><li>revalidate files at exec transition time</li><li>fine grained ptrace mediation</li><li>domain bounding through profile stacking<ul><li>profile stacking api</li><li>extended change_profile to support profile stacking</li><li>support profile stacks in exec transitions</li></ul></li><li>apparmorfs interface<ul><li> apparmorfs policy virtualization<ul><li>the <i>policy/</i> entry is now a special symlink to a virtualized policy directory</li><li><i>policy/</i> directory is now virtualized based on opening task confinement so tasks can only see the subset of policy in their view</li></ul></li> <li>add namespace level rawdata files<ul><li>unique profile based rawdata files for each namespace in <i>policy/raw_data/</i></li><li> profile raw_data files are now a symlink to the appropriate <i>policy/raw_data/</i> files.</li></ul></li><li>mkdir/rmdir fs based interface for creating namespaces<ul><li>mkdir <i>policy/namespaces/NAMESPACE</i></li><li>rmdir policy/namespaces/NAMESPACE</li></ul></li><li>revision file interface<ul><li>read current policy revision and select/poll for when policy changes via<ul><li> <i>revision</i> for reading the current task's policy namespace revision</li><li><i>policy/revision for the current namespace revision</li><li><i>policy/namespaces/NAMESPACE/revision</i> for a given namespace policy revision</li></ul></li></ul></li><li>query interface<ul><li>support multiple queries per query transaction</li><li>support querying if a profile supports a given mediation type</li></ul></li></ul></li><li>features set<ul><li>add namespace support to available feature set</li><li>add label data query availability to feature set</li></ul></li></ul>     |
| 4.14           | <ul><li> mount mediation<ul><li>new mount</li><li>remount</li><li>bind mount</li><li>change type</li><li>umount</li><li>pivot_root</li></ul><li>signal mediation</li><li>policy unpack log extended error messages</li></ul> |
| 4.15 - 4.16 | Bug fixes |
| 4.17           | <ul><li> v8 abi</li><li>generic socket mediation </li><li>improved profile attachment logic<ul><li>handle overlapping expression resolution up to 8 characters dynamic overlap in kernel</li><li>xattr attachment conditional</li><li>no_new_privs improved attachment with subset test based on confinement at time no_new_privs was entered</ul></li><li> signal mediation of profile stacks</li></ul>       |
| 4.18           | <ul><li>add support for secids and using secctxes</li><li>the ability to get a task's secid</li><li>add support for audit rules filtering. AppArmor task label can be used in audit rule filters</li></ul>        |
| 4.19           | Bug fixes |
| 4.20           | Secmark mediation for custom policy |
| 5.0            | Bug fixes |
| 5.1            | LSM stacking with generic blobs (sara/landlock). Does not include secids so insufficient to stack with selinux and smack. |
|----------------|-------------------------------------------------|

# Suse kernel

# Ubuntu Kernel
