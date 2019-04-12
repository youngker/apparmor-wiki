
# Upstream kernel

| Kernel Version | Feature |
|----------------|---------|
| 2.6.36         | Base functionality lands upstream mediation of: <ul><li>File<ul><li>owner conditional</li><li>read, write, link, lock, mmap exec</li></ul></li><li>Execute<ul><li>px, cx, ix, ux, pix, named transitions</li><li>attachment conditional separate from profile name</li></ul></li><li>Change hat<ul><li>single hat</li></ul></li><li>Change Profile</li><li>Capability</li></ul> |
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
| 4.11           |         |
| 4.12           |         |
| 4.13           |         |
| 4.14           | <ul><li> mount mediation<ul><li>new mount</li><li>remount</li><li>bind mount</li><li>change type</li><li>umount</li><li>pivot_root</li></ul><li>signal mediation</li></ul> |
| 4.15 - 4.16 | Bug fixes |
| 4.17           | <ul><li> v8 abi</li><li>generic socket mediation </li><li>improved profile attachment logic<ul><li>handle overlapping expression resolution up to 8 characters dynamic overlap in kernel</li><li>xattr attachment conditional</li><li>no_new_privs improved attachment with subset test based on confinement at time no_new_privs was entered</ul></li><li> signal mediation of profile stacks</li></ul>       |
| 4.18           | <ul><li>add support for secids and using secctxes</li><li>the ability to get a task's secid</li><li>add support for audit rules filtering. AppArmor task label can be used in audit rule filters</li></ul>        |
| 4.19           | Bug fixes |
| 4.20           | Secmark mediation for custom policy |
| 5.0            | Bug fixes |
| 5.1            | LSM stacking with generic blobs (sara/landlock) |
|----------------|-------------------------------------------------|

# Suse kernel

# Ubuntu Kernel
