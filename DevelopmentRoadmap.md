Development Roadmap
===================

The development roadmap has been split out into a list of work items
along with time estimates, and groups to aid in focusing development.

Work item list
--------------

```
[jjohansen] be disappointed we don't get to rehash previous sessions in a new session at UDS R: DONE
```

```
[jdstrand] templating, commit aa-easyprof tree soon (trunk only): DONE
[jjohansen] templating, expand available templates (low) (?): TODO
[jjohansen] templating, gui tool (low) (?): TODO
[jjohansen] templating, gui tool unit tests (low) (?): TODO
[jjohansen] templating, gui tool documentation/man pages (low) (?): TODO
```
 
```
[jdstrand] sandboxing, base aa-sandbox tool (low) (?): TODO
[jjohansen] sandboxing - fs overlay (low) (?): TODO
[jjohansen] sandboxing - alt X backend support. xephyr, xace (low) (?): TODO
[jjohansen] sandboxing - documentation/man pages (low) (?): TODO
```
 
```
[jjohansen] refresh/kick out cgroup prototype (low) (1): TODO
[jjohansen] refresh/kick out env matching prototype (low) (1): TODO
```
 
```
[jjohansen] fix deny mount perm bug - parser (high) (2): TODO
[jjohansen] fix deny mount perm bug - regression tests (high) (1): TODO
[jjohansen] fix change_onexec with threads test - kernel (medium) (0.5): TODO
[jjohansen] fix change_onexec with threads test - regression tests (medium) (0.5): TODO
[jjohansen] fix bug #888077 - alias only being partially applied - depends on dfa perms stored as set (high) (6): TODO
[jjohansen] fix missed transitions in handleChildren() - utils (low) (1): TODO
[jjohansen] fix null-XXX profiles to auto clean - kernel (low) (3): TODO
``` 
 
```
[jjohansen] misc, kernel - kernel export state controls in someplace other than /sys/modules (we don't control required cap check done there) needed by lxc to query state without needing cap in profile (essential) (1): TODO
[jjohansen] misc, kernel - profile rcu patch (low) (1): TODO
[jjohansen] misc, kernel - cleanup unpack (tristate return value) (???) (2): TODO
[jjohansen] misc, kernel - rework per class auditing to not call audit auto (???) (1): TODO
[jjohansen] misc, kernel auditing - split global audit flag into per type/perms flags (???) (2): TODO
[jjohansen] misc, kernel auditing - tests for split global audit flag into per type/perms flags (???) (2): TODO
[jjohansen] misc, kernel - automatically fallback to deleted file names in problematic lsm hooks (???) (1): TODO
[jjohansen] misc, kernel - evaluate need for special support of console fds (may create new work items) (???) (1): TODO
```

```
[jjohansen] misc parser, remove cruft
[jjohansen] misc parser, base conversion to C++ so that it can compile in a C++ compiler
[jjohansen] misc parser, convert structs, fns to classes
[jjohansen] misc parser, convert/cleanup allocations
[jjohansen] misc parser, split out writing binary format to object
[jjohansen] misc parser, split out parsing library
[jjohansen] misc parser, fix disabled profile using name not symlink
[jjohansen] misc parser, support overlapping x transitions - deps dfa dominance (low) (1): TODO
```

```
[jjohansen] misc, utils - switch tools to using null profile path instead of parent pid for fork tracking - utils (low) (2): TODO
[jjohansen] misc, utils - update pam_apparmor to use change_profile (???) (3): TODO
[jjohansen] misc, utils - variable support in tools (???) (?): TODO
[jjohansen] misc, utils - conditional support in tools (???) (?): TODO
[jjohansen] misc, utils - rewrite genprof/logprof in python (???) (?): TODO
```

```
[jjohansen] aa-namespaces, evaluate attaching aa-namespace to other system namespace, to aid with auditing stacking ... (???) (?): TODO
```

```
[jjohansen] aa-namespaces, add user namespace support - upstream - deps: aa-ns controls, stacking (low) (?): TODO
[jjohansen] aa-namespaces, add user namespace support - kernel - deps: aa-ns controls, stacking (low) (?): TODO
[jjohansen] aa-namespaces, add user namespace support - parser - deps: aa-ns controls, stacking (low) (?): TODO
[jjohansen] aa-namespaces, add user namespace support - regression tests - deps: aa-ns controls, stacking (low) (?): TODO
[jjohansen] aa-namespaces, add user namespace support - update userspace tools if needed - deps: aa-ns controls, stacking (low) (?): TODO
[jjohansen] aa-namespaces, add user namespace support - documentation/man pages - deps: aa-ns controls, stacking (low) (?): TODO
```

```
[jjohansen] kill tagging - parser - deps: extended perms (low) (1): TODO
[jjohansen] kill tagging - parser tests (low) (0.5): TODO
[jjohansen] kill tagging - regression tests (low) (1): TODO
[jjohansen] kill tagging - update userspace tools (low) (1): TODO
[jjohansen] kill tagging - userspace tools unit tests (low) (1): TODO
[jjohansen] kill tagging - update documentation/man pages (low) (0.5): TODO
```

```
[jjohansen] stop mode, per profile flag - kernel (low) (0.5): TODO
[jjohansen] stop mode, per profile flag - parser, includes tests (low) (0.5): TODO
[jjohansen] stop mode, per profile flag - regression tests (low) (1): TODO
[jjohansen] stop mode, per profile flag - userspace tools (1): TODO
[jjohansen] stop mode, per profile flag - update documentation/man pages (1): TODO
```

```
[jjohansen] stop mode, per rule flag - kernel - deps: extend perms (low) (1): TODO
[jjohansen] stop mode, per rule flag - parser - deps: extend perms (low) (2): TODO
[jjohansen] stop mode, per rule flag - parser tests - deps: extend perms (low) (1): TODO
[jjohansen] stop mode, per rule flag - regression tests - deps: extend perms (low) (2): TODO
[jjohansen] stop mode, per rule flag - userspace tools - deps: extend perms (low) (2): TODO
[jjohansen] stop mode, per rule flag - userspace tools unit tests - deps: extend perms (low) (1): TODO
[jjohansen] stop mode, per rule flag - update documentation/man pages (low) (1): TODO
```

```
[jjohansen] interactive learning, using stop mode + ptrace - userspace tools - deps: stop mode (low) (2): TODO
```

```
[jjohansen] update how labeling of unix domain sockets is done (high) (2): TODO
```

```
[jjohansen] explicit labeling, extend kernel perms - kernel (???) (1): TODO
[jjohansen] explicit labeling, extend policy language - parser (???) (2): TODO
[jjohanesn] explicit labeling, parser tests (???) (0.5): TODO
[jjohansen] explicit labeling, temporary explicit label - kernel (???) (4): TODO
[jjohansen] explicit labeling, stored explicit label - kernel (???) (4): TODO
[jjohansen] explicit labeling, interface to set explicit label - kernel (???) (3): TODO
[jjohansen] explicit labeling, api to set label - libapparmor (???) (1): TODO
[jjohansen] explicit labeling - regression tests (???) (3): TODO
[jjohansen] explicit labeling - update aa-logparse, including tests (???) (1): TODO
[jjohansen] explicit labeling - userspace tools (???) (2): TODO
[jjohansen] explicit labeling - userspace tools unit tests (???) (1): TODO
[jjohansen] explicit labeling - documentation/man pages (???) (1): TODO
```

```
[jjohansen] implicit delegation - RFC/discussion (???) (2): TODO
[jjohansen] implicit delegation - upstream (???) (5): TODO
[jjohansen] implicit delegation, label open objects at exec - kernel - deps labeling, fd passing (???) (1): TODO
[jjohansen] implicit delegation, label open objects at ipc - kernel - deps labeling, fd passing (???) (1): TODO
[jjohansen] implicit delegation, update learning mode - kernel (???) (3): TODO
[jjohansen] implicit delegation, extend perms to support delegation - kernel (???) (1): TODO
[jjohansen] implicit delegation, extend policy lanaguage - parser, delegate and receive (???) (2): TODO
[jjohansen] implicit delegation, extend perm mapping - parser (???) (1): TODO
[jjohansen] implicit delegation - parser tests (???) (1): TODO
[jjohansen] implicit delegation - regression tests (???) (2): TODO
[jjohansen] implicit delegation - update aa-logparser, including tests (???) (1): TODO
[jjohansen] implicit delegation - userspace tools (???) (2): TODO
[jjohansen] implicit delegation - userspace tools unit tests (???) (1): TODO
[jjohansen] implicit delegation - documentation and man pages (???) (1): TODO
[jjohansen] implicit delegation - evaluate best method to handle missing file label at hook (file hack, lsm patches, explicit pathname list) (???) (1): TODO
[jjohansen] implicit delegation - deal with disconnected and deleted files that have been delegated (???) (1): TODO
[jjohansen] implicit delegation, file discovery hack - kernel - (???) (2): TODO
[jjohansen] implicit delegation, lsm patches for getattrs and other paths that don't have sufficient info to distinguish access from file from new lookup - kernel (???) (20): TODO
```

```
[jjohansen] explicit delegation, add interface to delegate file - kernel (???) (2): TODO
[jjohansen] explicit delegation, track delegated name, or rule set? - kernel (???) (4): TODO
[jjohansen] explicit delegation, add api to delegation - libapparmor (???) (2): TODO
[jjohansen] explicit delegation - regression tests (???) (2): TODO
[jjohansen] explicit delegation - documentation/man page (???) (1): TODO
```

```
[jjohansen] env filtering stage 0, RFC/discussion (???) (2): TODO
[jjohansen] env filtering stage 0, determine priority with Gary Poster (low) (1): TODO
```

```
[jjohansen] env filtering stage 1, whole var matching - upstream - deps ??? (medium) (2): TODO
[jjohansen] env filtering stage 1, whole var matching - kernel - deps ??? (???) (3): TODO
[jjohansen] env filtering stage 1, whole var matching - parser - deps ??? (high) (1): TODO
[sbeattie] env filtering stage 1, whole var matching - parser tests - deps ??? (medium) (0.5): TODO
[sbeattie] env filtering stage 1, whole var matching - regression tests - deps ??? (high) (1): TODO
[jjohansen] env filtering stage 1, whole var matching - update aa-logparser, including tests - deps ??? (???) (1): TODO
[jjohansen] env filtering stage 1, whole var matching - userspace tools - deps ??? (???) (1): TODO
[jjohansen] env filtering stage 1, whole var matching - userspace tools unit tests - deps ??? (???) (1): TODO
[jdstrand] env filtering stage 1, whole var matching - update documentation/man pages - deps ??? (low) (0.5): TODO
```

```
[jjohansen] env filtering stage 2, whole var filtering - upstream (3): TODO
[jjohansen] env filtering stage 2, whole var filtering - kernel - deps ??? (???) (2): TODO
[jjohansen] env filtering stage 2, whole var filtering - parser - deps env matching, ??? (high) (1): TODO
[sbeattie] env filtering stage 2, whole var filtering - parser tests - deps ??? (medium) (0.5): TODO
[sbeattie] env filtering stage 2, whole var filtering - regression tests - deps ??? (high) (1): TODO
[jjohansen] env filtering stage 2, whole var filtering - update aa-logparser, including tests - deps ??? (???) (1): TODO
[jjohansen] env filtering stage 2, whole var filtering - userspace tools - deps ??? (???) (1): TODO
[jjohansen] env filtering stage 2, whole var filtering - userspace tools unit tests - deps ??? (???) (1): TODO
[jdstrand] env filtering stage 2, whole var filtering - update documentation/man pages -deps ??? (low) (0.5): TODO
```

```
[jjohansen] env filtering stage 3, sub element matching/filtering - upstream - deps ???? (???) (2): TODO
[jjohansen] env filtering stage 3, sub element matching/filtering - kernel - deps submatch extension (low) (2): TODO
[jjohansen] env filtering stage 3, sub element matching/filtering - parser - deps submatch, filter ??? (high) (1): TODO
[sbeattie] env filtering - stage 3, sub element matching/filtering - parser tests - deps ??? (medium) (0.5): TODO
[sbeattie] env filtering stage 3, sub element matching/filtering - regression tests - deps ??? (high) (1): TODO
[jjohansen] env filtering stage 3, sub element matching/filtering - update aa-logparser, including tests - deps ??? (???) (1): TODO
[jjohansen] env filtering stage 3, sub element matching/filtering - userspace tools - deps ??? (???) (1): TODO
[jjohansen] env filtering stage 3, sub element matching/filtering - userspace tools unit tests - deps ??? (???) (1): TODO
[jdstrand] env filtering stage 3, sub element matching/filtering - update documentation/man pages - deps ??? (low) (0.5): TODO
```

```
[jjohansen] env filtering stage 4, evaluate if exec helper or in kernel modification is best (assumes helper) - deps ??? (???) (2): TODO
```

```
[jjohansen] env filtering stage 5, set value based on kernel var - create exec helper daemon/callout - deps ??? (low) (5): TODO
[jjohansen] env filtering stage 5, set value based on kernel var - audit exec helper daemon/callout - deps ??? (low) (1): TODO
[jjohansen] env filtering stage 5, set value based on kernel var - upstream - deps kernel vars, ??? (???) (2): TODO
[jjohansen] env filtering stage 5, set value based on kernel var - kernel - deps kernel vars , exec helper (???) (5): TODO
[jjohansen] env filtering stage 5, set value based on kernel var - parser - deps kernel helper (low) (1): TODO
[sbeattie] env filtering stage 5, set value based on kernel var - parser tests - deps ??? (medium) (0.5): TODO
[sbeattie] env filtering stage 5, set value based on kernel var - regression tests/extend infrastructure for helper - deps ??? (high) (5): TODO
[jjohansen] env filtering stage 5, set value based on kernel var - update aa-logparser, including tests - deps ??? (???) (1): TODO
[jjohansen] env filtering stage 5, set value based on kernel var - userspace tools - deps ??? (???) (1): TODO
[jjohansen] env filtering stage 5, set value based on kernel var - userspace tools unit tests - deps ??? (???) (1): TODO
[jdstrand] env filtering stage 5, set value based on kernel var - update documentation/man pages - deps ??? (low) (0.5): TODO
```

```
[jjohansen] arg filtering stage 0, RFC/discussion, how far should it be taken (low) (5): TODO
[jjohansen] arg filtering stage 1, matching/conditional attach - upstream - deps ??? (low) (3): TODO
[jjohansen] arg filtering stage 1, matching/conditional attach - kernel - deps env match (low) (3): TODO
[jjohansen] arg filtering stage 1, matching/conditional attach - parser - deps ??? (low) (2): TODO
[jjohansen] arg filtering stage 1, matching/conditional attach - parser tests - deps ??? (low) (0.5): TODO
[jjohansen] arg filtering stage 1, matching/conditional attach - update exec helper - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 1, matching/conditional attach - regression tests - deps ??? (low) (3): TODO
[jjohansen] arg filtering stage 1, matching/conditional attach - update aa-logparser, including tests - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 1, matching/conditional attach - userspace tools - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 1, matching/conditional attach - userspace tools unit tests - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 1, matching/conditional attach - update documentation/man pages - deps ??? (low) (0.5): TODO
```

```
[jjohansen] arg filtering stage 2, whole arg filtering - upstream - deps ??? (low) (3): TODO
[jjohansen] arg filtering stage 2, whole arg filtering - kernel - deps env match (low) (1): TODO
[jjohansen] arg filtering stage 2, whole arg filtering - parser - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 2, whole arg filtering - parser tests - deps ??? (low) (0.5): TODO
[jjohansen] arg filtering stage 2, whole arg filtering - regression tests - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 2, whole arg filtering - update aa-logparser, including tests - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 2, whole arg filtering - userspace tools - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 2, whole arg filtering - userspace tools unit tests - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 2, whole arg filtering - update documentation/man pages - deps ??? (low) (0.5): TODO
```

```
[jjohansen] arg filtering stage 3, sub arg filtering - upstream - deps ??? (low) (3): TODO
[jjohansen] arg filtering stage 3, sub arg filtering - kernel - deps env match (low) (1): TODO
[jjohansen] arg filtering stage 3, sub arg filtering - parser - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 3, sub arg filtering - parser tests - deps ??? (low) (0.5): TODO
[jjohansen] arg filtering stage 3, sub arg filtering - regression tests - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 3, sub arg filtering - update aa-logparser, including tests - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 3, sub arg filtering - userspace tools - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 3, sub arg filtering - userspace tools unit tests - deps ??? (low) (1): TODO
[jjohansen] arg filtering stage 3, sub arg filtering - update documentation/man pages - deps ??? (low) (0.5): TODO
```

```
[jjohansen] ext. mediation, chroot, RFC/discuss chroot relative solution (high) (2): TODO
[jjohansen] ext. mediation, chroot, transition rules - upstream (???) (2): TODO
[jjohansen] ext. mediation, chroot, set kernel var at transition - kernel (???) (1): TODO
[jjohansen] ext. mediation, chroot, remove use of d_absolute_path for mediation - kernel - deps kernel vars, labeling (low) (1): TODO
[jjohansen] ext. mediation, chroot, auto add kernel var use to support old style profiles - parser (???) (2): TODO
[jjohansen] ext. mediation, chroot, - update documentation/man pages (???) (0.5): TODO
```

```
[jjohansen] ext. mediation, chroot, transition rules - upstream (???) (2): TODO
[jjohansen] ext. mediation, chroot, transition rules, to mirror pivot root - kernel - deps chroot relative resolved (high) (1): TODO
[jjohansen] ext. mediation, chroot, transition rules - parser (high) (1): TODO
[jjohansen] ext. mediation, chroot, transition rules - parser tests (???) (0.5): TODO
[jjohansen] ext. mediation, chroot, transition rules - regression tests (???) (1): TODO
[jjohansen] ext. mediation, chroot, transition rules - update aa-logparse, including tests (???) (1): TODO
[jjohansen] ext. mediation, chroot, transition rules - userspace tools (???) (1): TODO
[jjohansen] ext. mediation, chroot, transition rules - userspace tools unit tests (???) (1): TODO
[jjohansen] ext. mediation, chroot, transition rules - documentation/man pages (???) (0.5): TODO
```

```
[jjohansen] ext. mediation, signal - upstream (???) (2): TODO
[jjohansen] ext. mediation, signal, extend checks to kill hook - kernel (???) (1): TODO
[jjohansen] ext. mediation, signal, use sids for interrupts - kernel (???) (2): TODO
[jjohansen] ext. mediation, signal, extend policy language - parser (???) (2): TODO
[jjohansen] ext. mediation, signal - parser tests (???) (0.5): TODO
[jjohansen] ext. mediation, signal - regression tests (???) (2): TODO
[jjohansen] ext. mediation, signal - update aa-logparser (???) (1): TODO
[jjohansen] ext. mediation, signal - userspace tools (???) (2): TODO
[jjohansen] ext. mediation, signal - userspace tools unit tests (???) (1): TODO
[jjohansen] ext. mediation, signal - documentation/man page (0.5): TODO
```

```
[jjohansen] ext. mediation, alt ns unix domain socket, labeling - kernel - deps labeling (???) (1): TODO
[jjohansen] ext. mediation, alt ns unix domain socket, policy language - parser (???) (1): TODO
[jjohansen] ext. mediation, alt ns unix domain socket - parser tests (???) (0.5): TODO
[jjohansen] ext. mediation, alt ns unix domain socket - regressiont tests (???) (2): TODO
[jjohansen] ext. mediation, alt ns unix domain socket - update aa-logparse, including tests (???) (1): TODO
[jjohansen] ext. mediation, alt ns unix domain socket - userspace tools (???) (2): TODO
[jjohansen] ext. mediation, alt ns unix domain socket - userspace tools unit tests (???) (1): TODO
[jjohansen] ext. mediation, alt ns unix domain socket - documentation/man pages (0.5): TODO 
```

```
[jjohansen] ext. mediation, netlink, address matching - kernel (???) (2): TODO
[jjohansen] ext. mediation, netlink, profile language - parser (???) (2): TODO
[jjohansen] ext. mediation, netlink - parser tests (???) (0.5): TODO
[jjohansen] ext. mediation, netlink - regression tests (???) (2): TODO
[jjohansen] ext. mediation, netlink - update aa-logparser, including tests (???) (1): TODO
[jjohansen] ext. mediation, netlink - userspace tools (???) (2): TODO
[jjohansen] ext. mediation, netlink - userspace tools unit tests (???) (1): TODO
[jjohansen] ext. mediation, netlink - documentation/man pages (???) (0.5): TODO
```

```
[jjohansen] ext. mediation, ipc, RFC/discussion (???) (1): TODO
[jjohansen] ext. mediation, ipc - upstream (???) (1): TODO
[jjohansen] ext. mediation, ipc mediate - kernel (???) (5): TODO
[jjohansen] ext. mediation, ipc rules - parser (???) (2): TODO
[sbeattie] ext. mediation, ipc rules - parser tests (???) (1): TODO
[sbeattie] ext. mediation, ipc rules - regression tests (???) (2): TODO
[jjohansen] ext. mediation, ipc rules - update aa-logparser, including tests (???) (1): TODO
[jjohansen] ext. mediation, ipc rules - userspace tools (???) (2): TODO
[jjohansen] ext. mediation, ipc rules - userspace tools unit tests (???) (1): TODO
[jdstrand] ext. mediation, ipc rules - documentation/man pages (???) (1): TODO
```

```
[jjohansen] ext. mediation, anonymous ipc (pipes, sock pairs, ..) - RFC/discussion (???) (1): TODO
[jjohansen] ext. mediation, anonymous ipc (pipes, sock pairs, ..) - upstream (???) (1): TODO
[jjohansen] ext. mediation, anonymous ipc (pipes, sock pairs, ..) mediate - kernel (???) (5): TODO
[jjohansen] ext. mediation, anonymous ipc rules (pipes, sock pairs, ..) - parser (???) (2): TODO
[sbeattie] ext. mediation, anonymous ipc rules (pipes, sock pairs, ..) - parser tests (???) (1): TODO
[sbeattie] ext. mediation, anonymous ipc rules (pipes, sock pairs, ..) - regression tests (???) (2): TODO
[jjohansen] ext. mediation, anonymous ipc rules (pipes, sock pairs, ..) - update aa-logparser, including tests (???) (1): TODO
[jjohansen] ext. mediation, anonymous ipc rules (pipes, sock pairs, ..) - userspace tools (???) (2): TODO
[jjohansen] ext. mediation, anonymous ipc rules (pipes, sock pairs, ..) - userspace tools unit tests (???) (1): TODO
[jdstrand] ext. mediation, anonymoys ipc rules (pipes, sock pairs, ..) - documentation/man pages (???) (1): TODO
```

```
[jjohansen] ext. mediation, key ring - RFC/discussion (2): TODO
[jjohansen] ext. mediation, key ring - upstream - deps ??? (high) (2): TODO
[jjohansen] ext. mediation, key ring - kernel - deps labeling, conditionals (high) (3): TODO
[jjohansen] ext. mediation, key ring - parser - deps labeling, conditionals (high) (2): TODO
[jjohansen] ext. mediation, key ring - parser - deps ??? (high) (0.5): TODO
[jjohansen] ext. mediation, key ring - regression tests - deps ??? (high) (1): TODO
[jjohansen] ext. mediation, key ring - update aa-logparser, includes tests - deps ???? (high) (1): TODO
[jjohansen] ext. mediation, key ring - userspace tools - deps ??? (high) (2): TODO
[jjohansen] ext. mediation, key ring - userspace tools unit tests - deps ???? (?): TODO
[jjohansen] ext. mediation, key ring - update documentation/man pages - deps ??? (0.5): TODO
```

```
[jjohansen] ext. mediation, extended file perms - upstream (???) (1): TODO
[jjohansen] ext. mediation, extended file perms - kernel (???) (2): TODO
[jjohansen] ext. mediation, extended file perms - parser - deps: dfa set perms (???) (2): TODO
[jjohansen] ext. mediation, extended file perms - parser tests (???) (0.5): TODO
[jjohansen] ext. mediation, extended file perms - regression tests (???) (3): TODO
[jjohansen] ext. mediation, extended file perms - aa-logparser, including tests (???) (1): TODO
[jjohansen] ext. mediation, extended file perms - userspace tools (???) (2): TODO
[jjohansen] ext. mediation, extended file perms - userspace tools unit tests (???) (1): TODO
[jjohansen] ext. mediation, extended file perms - documentation/man pages (???) (0.5): TODO
```

```
[jjohansen] ext. mediation, file perms recursive tagging - upstream (???) (1): TODO
[jjohansen] ext. mediation, file perms recursive tagging - kernel (???) (1): TODO
[jjohansen] ext. mediation, file perms recursive tagging - parser - deps: dfa set perms, dfa dominance (???) (2): TODO
[jjohansen] ext. mediation, file perms recursive tagging - parser tests (???) (0.5): TODO
[jjohansen] ext. mediation, file perms recursive tagging - regression tests (???) (1): TODO
[jjohansen] ext. mediation, file perms recursive tagging - documentation/man pages (???) (1): TODO
```

```
[jjohansen] ext. mediation, memory controls. mmap min_addr, mmap w -> x, upstream (???) (2): TODO
[jjohansen] ext. mediation, memory controls. mmap min_addr, mmap w -> x, kernel (???) (1): TODO
[jjohansen] ext. mediation, memory controls. mmap min_addr, mmap w -> x, parser (???) (1): TODO
[jjohansen] ext. mediation, memory controls. mmap min_addr, mmap w -> x, parser tests (???) (0.5): TODO
[jjohansen] ext. mediation, memory controls. mmap min_addr, mmap w -> x, regression tests (???) (2): TODO
[jjohansen] ext. mediation, memory controls. mmap min_addr, mmap w -> x, aa-logparser, including tests (???) (1): TODO
[jjohansen] ext. mediation, memory controls. mmap min_addr, mmap w -> x, userspace tools (???) (2): TODO
[jjohansen] ext. mediation, memory controls. mmap min_addr, mmap w -> x, userspace tools unit tests (???) (1): TODO
[jjohansen] ext. mediation, memory controls. mmap min_addr, mmap w -> x, update documentation/man pages (???) (1): TODO
```

```
[jjohansen] ext. mediation, snapshots, upstream (???) (3): TODO
[jjohansen] ext. mediation, snapshots, kernel (???) (1): TODO
[jjohansen] ext. mediation, snapshots, parser - deps: recursive perm tagging (???) (1): TODO
[jjohansen] ext. mediation, snapshots, parser tests (???) (0.5): TODO
[jjohansen] ext. mediation, snapshots, regression tests (???) (2): TODO
[jjohansen] ext. mediation, snapshots, aa-logparser, including tests (???) (1): TODO
[jjohansen] ext. mediation, snapshots, userspace tools (???) (1): TODO
[jjohansen] ext. mediation, snapshots, userspace tools unit tests (???) (1): TODO
[jjohansen] ext. mediation, snapshots, update documentation/man pages (???) (0.5): TODO
```

```
[jjohansen] ext. mediation, setuid, upstream (???) (3): TODO
[jjohansen] ext. mediation, setuid, kernel (???) (2): TODO
[jjohansen] ext. mediation, setuid, parser (???) (1): TODO
[jjohansen] ext. mediation, setuid, parser tests (???) (0.5): TODO
[jjohansen] ext. mediation, setuid, regression tests (???) (2): TODO
[jjohansen] ext. mediation, setuid, userspace tools (???) (2): TODO
[jjohansen] ext. mediation, setuid, userspace tools unit tests (???) (1): TODO
[jjohansen] ext. mediation, setuid, update documentation/man pages (???) (0.5): TODO
```

```
[jjohansen] ext. mediation, ptrace - upstream (???) (1): TODO
[jjohansen] ext. mediation, ptrace - kernel (???) (0.5): TODO
[jjohansen] ext. mediation, ptrace - parser (???) (0.5): TODO
[jjohansen] ext. mediation, ptrace - parser tests (???) (0.5): TODO
[jjohansen] ext. mediation, ptrace - regression tests (???) (1): TODO
[jjohansen] ext. mediation, ptrace - aa-logparser, including tests (???) (1): TODO
[jjohansen] ext. mediation, ptrace - userspace tools (???) (1): TODO
[jjohansen] ext. mediation, ptrace - userspace tools unit tests (???) (1): TODO
[jjohansen] ext. mediation, ptrace - documentation/man pages (???) (0.5): TODO
```

```
[jjohansen] base extended conditionals - upstream (???) (2): TODO
[jjohansen] base extended conditionals - kernel - deps ??? (???) (2): TODO
[jjohansen] base extended conditionals - parser - deps ??? (???) (2): TODO
[jjohansen] base extended conditionals, file - parser (???) (0.5): TODO
[jjohansen] base extended conditionals, net - parser (???) (0.5): TODO
[jjohansen] base extended conditionals, mount - parser (???) (0.5): TODO
[jjohansen] base extended conditionals, caps - parser (???) (0.5): TODO
[jjohansen] base extended conditionals - parser tests (???) (0.5): TODO
[jjohansen] base extended conditionals - regression tests (???) (5): TODO
[jjohansen] base extended conditionals - userspace tools (???) (2): TODO
[jjohansen] base extended conditionals - userspace tools unit tests (???) (1): TODO
[jjohansen] base extended conditionals - documentation/man pages (???) (0.5): TODO
```

```
[jjohansen] file, mount conditionals - kernel - deps ??? (???) (1): TODO
[jjohansen] file, mount conditionals - parser - deps ??? (???) (1): TODO
[jjohansen] file, mount conditionals - parser tests (???) (0.5): TODO
[jjohansen] file, mount conditionals - regression tests (???) (1): TODO
```

```
[jjohansen] private mounts, syctl .. - upstream (???) (1): TODO
[jjohansen] private mounts, syctl .. - kernel - deps file mount conditionals, ??? (medium) (3): TODO
[jjohansen] private mounts, syctl .. - regression tests - deps apparmor private mounts sysctl (medium) (1): TODO
```

```
[jjohansen] private mounts labeling, overlayfs - upstream (???) (2): TODO
[jjohansen] private mounts labeling, overlayfs - kernel - deps labeling, conditionals (???) (2): TODO
[jjohansen] private mounts labeling, overlayfs - parser - deps labeling, conditionals (???) (2): TODO
[jjohansen] private mounts labeling, overlayfs - parser tests - deps ??? (???) (0.5): TODO
[jjohansen] private mounts labeling, overlayfs - regression tests - deps ??? (???) (1): TODO
[jjohansen] private mounts labeling, overlayfs - update aa-logparser, include tests - deps ??? (???) (1): TODO
[jjohansen] private mounts labeling, overlayfs - userspace tools - deps ??? (???) (1): TODO
[jjohansen] private mounts labeling, overlayfs - userspace tools unit tests - deps ??? (???) (1): TODO
[jjohansen] private mounts labeling, overlayfs - documentation/man pages (low) (0.5): TODO
```

```
[jjohansen] network stage 0, RFC/discussion (3): TODO
```

```
[jjohansen] network stage 1, wider permissions - upstream - deps ??? (???) (2): TODO
[jjohansen] network stage 1, wider permissions - kernel - deps ??? (???) (2): TODO
[jjohansen] network stage 1, wider permissions - parser - deps ??? (???) (1): TODO
[sbeattie] network stage 1, wider permissions - parser tests - deps ??? (medium) (1): TODO
[sbeattie] network stage 1, wider permissions - regression tests - deps ??? (medium) (2): TODO
[jjohansen] network stage 1, wider permissions - update aa-logparser, including tests - deps ??? (???) (1): TODO
[jjohansen] network stage 1, wider permissions - update userspace tools - deps ??? (???) (2): TODO
[jjohansen] network stage 1, wider permissions - unit tests for userspace tools - deps ??? (???) (2): TODO
[jjohansen] network stage 1, wider permissions - update documentation/manpages - deps ??? (???) (0.5): TODO
```

```
[jjohansen] network stage 2, socket address bind/send - upstream - deps ??? (???) (2): TODO
[jjohansen] network stage 2, socket address bind/send - kernel - deps ??? (???) (2): TODO
[jjohansen] network stage 2, socket address bind/send - parser - deps ??? (???) (1): TODO
[jjohansen] network stage 2, socket address bind/send - parser tests - deps ??? (???) (1): TODO
[jjohansen] network stage 2, socket address bind/send - regression tests - deps ??? (???) (2): TODO
[jjohansen] network stage 2, socket address bind/send - update aa-logparser, including tests - deps ??? (???) (1): TODO
[jjohansen] network stage 2, socket address bind/send - update userspace tools - deps ??? (???) (2): TODO
[jjohansen] network stage 2, socket address bind/send - unit tests for userspace tools - deps ??? (???) (2): TODO
[jjohansen] network stage 2, socket address bind/send - update documentation/man pages - deps ??? (???) (0.5): TODO
```

```
[jjohansen] network stage 3, socket labeling - upstream - deps ??? (???) (2): TODO
[jjohansen] network stage 3, socket labeling - kernel - deps ??? (???) (2): TODO
[jjohansen] network stage 3, socket labeling - kernel getcon, getpeercon - deps ??? (???) (1): TODO
[jjohansen] network stage 3, socket labeling - library getcon, getpeercon - deps ??? (???) (0.5): TODO
[jjohansen] network stage 3, socket labeling - regression tests for getcon, getpeercon - deps ??? (???) (0.5): TODO
[jjohansen] network stage 3, socket labeling - update network regression tests - deps ??? (???) (1): TODO
```

```
[jjohansen] network stage 4, network delegation - upstream - deps ??? (???) (2): TODO
[jjohansen] network stage 4, network delegation - kernel - deps ??? (???) (1): TODO
[jjohansen] network stage 4, network delegation - parser - deps ??? (???) (1): TODO
[jjohansen] network stage 4, network delegation - parser tests - deps ??? (???) (1): TODO
[jjohansen] network stage 4, network delegation - regression tests - deps ??? (???) (2): TODO
[jjohansen] network stage 4, network delegation - update aa-logparser, including tests - deps ??? (???) (1): TODO
[jjohansen] network stage 4, network delegation - update userpace tools - deps ??? (???) (2): TODO
[jjohansen] network stage 4, network delegation - unit tests for userspace tools - deps ??? (???) (2): TODO
[jjohansen] network stage 4, network delegation - update documentation/man pages - deps ??? (???) (0.5): TODO
```

```
[jjohansen] network stage 5, receive labeling - upstream - deps ??? (???) (4): TODO
[jjohansen] network stage 5, receive labeling - kernel - deps ??? (???) (4): TODO
[jjohansen] network stage 5, receive labeling - parser global computation- deps ??? (???) (10): TODO
[jjohansen] network stage 5, receive labeling - iptables interface - deps ??? (???) (5): TODO
[jjohansen] network stage 5, receive labeling - parser tests - deps ??? (???) (1): TODO
[jjohansen] network stage 5, receive labeling - regression tests - deps ??? (???) (4): TODO
[jjohansen] network stage 5, receive labeling - update aa-logparser, including tests - deps ??? (???) (1): TODO
[jjohansen] network stage 5, receive labeling - update userspace tools - deps ??? (???) (2): TODO
[jjohansen] network stage 5, receive labeling - unit tests for userspace tools - deps ??? (???) (2): TODO
[jjohansen] network stage 5, receive labeling - update documentation/man pages - deps ??? (???) (0.5): TODO
```

```
[jjohansen] network stage 6, CIPSO - upstream - deps ??? (???) (2): TODO
[jjohansen] network stage 6, CIPSO - kernel - deps ??? (???) (2): TODO
[jjohansen] network stage 6, CIPSO - parser - deps ??? (???) (1): TODO
[jjohansen] network stage 6, CIPSO - parser tests - deps ??? (???) (1): TODO
[jjohansen] network stage 6, CIPSO - regression tests - deps ??? (???) (2): TODO
[jjohansen] network stage 6, CIPSO - update aa-logparser, including tests - deps ??? (???) (1): TODO
[jjohansen] network stage 6, CIPSO - update userspace tools - deps ??? (???) (2): TODO
[jjohansen] network stage 6, CIPSO - unit tests for userspace tools - deps ??? (???) (2): TODO
[jjohansen] network stage 6, CIPSO - update documentation/man pages - deps ??? (???) (0.5): TODO
```

```
[jjohansen] cgroup based resource control - RFC/discussion (???) (4): TODO
[jjohansen] cgroup based resource control, refresh demo (???) (1): TODO
[jjohansen] cgroup based resource control, load policy - kernel (???) (3): TODO
[jjohansen] cgroup based resource control, transition with profile - kernel (???) (2): TODO
[jjohansen] cgroup based resource control, extend language - parser (high) (2): TODO
[jjohansen] cgroup based resource control, create cgroups - parser (high) (2): TODO
[sbeattie] cgroup based resource control - parser tests (medium) (0.5): TODO
[jjohansen] cgroup based resource control - experiment on interaction with stacking and containers (high) (5): TODO
[sbeattie] cgroup based resource control - regression tests (medium) (3): TODO
[jjohansen] cgroup based resource control - userspace tools (???) (2): TODO
[jjohansen] cgroup based resource control - userspace tools unit tests (???) (1): TODO
[jdstrand] cgroup based resource control - documentation (medium) (1): TODO
```

```
[jjohansen] default profile, so unconfined can be replaced - upstream (???) (1): TODO
[jjohansen] default profile, so unconfined can be replaced - kernel  (???) (2): TODO
[jjohansen] default profile, so unconfined can be replaced - regression tests (???) (1): TODO
[jjohansen] default profile, so unconfined can be replaced - documentation/man pages (???) (0.5): TODO
```

```
[jjohansen] policy language, extend to support pcre - parser (???) (2): TODO
[jjohansen] policy language, extend to support pcre - parser tests (???) (0.5): TODO
[jjohansen[ policy language, extend to support pcre - regression tests (???) (2): TODO
```

```
[jjohansen] dfa improvements, integrate cache line remapping - kernel (low) (1): TODO
[jjohansen] dfa improvements, mapping of loaded perms - kernel (low) (1): TODO
[jjohansen] dfa improvements, integrate differential encoding - parser (low) (0.5): TODO
[jjohansen] dfa improvements, unit tests - parser (low) (2): TODO
[jjohansen] dfa improvements, tree normalization and factoring - parser (low) (5): TODO
[jjohansen] dfa improvements, rework extraneous hash set during creation - parser (low) (2): TODO
[jjohansen] dfa improvements, minimization improve comparison - parser (low) (1): TODO
[jjohansen] dfa improvements, minimization investigate reverse mapping - parser (low) (2): TODO
[jjohansen] dfa improvements, rewrite compression routine - parser (low) (5): TODO
[jjohansen] dfa improvements, direct aare parsing - parser (low) (2): TODO
[jjohansen] dfa improvements, dominance calculation support (low) (3): TODO
```

```
[jjohansen] group policy, compile dfa to track multiple profiles - parser - deps set perms (low) (2): TODO
[jjohansen] group policy, compile extend perms to list profile sets - parser (low) (2): TODO
[jjohansen] group policy, compile sub profiles to use same dfa - parser (low) (2): TODO
[jjohansen] group policy, compile all provided profiles into same dfa - parser (low) (2): TODO
[jjohansen] group policy, atomic load - kernel (low) (2): TODO
[jjohansen] group policy, map loaded groups to SIDs (low) (2): TODO
[jjohansen] group policy, match based on loaded dfa - kernel (low) (3): TODO
[jjohansen] group policy, - unit testing (low) (2): TODO
[jjohansen] group policy, - regression testing (low) (2): TODO
```

```
[jjohansen] dfa, document kernel var matching algo (low) (5): TODO
[jjohansen] dfa kernel vars, matching extension - kernel, special @{proc} (low) (2): TODO
[jjohansen] dfa kernel vars, matching extension - kernel, ns (low) (3): TODO
[jjohansen] dfa kernel vars, matching extension - kernel, cred tree @{proc} (low) (4): TODO
[jjohansen] dfa kernel vars, libapparmor matching extension - libapparmor (low) (2): TODO
[jjohansen] dfa kernel vars, parser support to pass to backend - parser (low) (2): TODO
[jjohansen] dfa kernel vars, extend expr tree - parser (low) (3): TODO
[jjohansen] dfa kernel vars, extend dfa creation - paser (low) (3): TODO
[jjohansen] dfa kernel vars, extend minimization - parser (low) (2): TODO
[jjohansen] dfa kernel vars, extend diff encode - parser (low) (1): TODO
[jjohansen] dfa kernel vars, extend compression - parser (low) (2): TODO
[jjohansen] dfa kernel vars  - unit tests (low) (2): TODO
[jjohansen] dfa kernel vars - regression tests (low) (3): TODO
```

```
[jjohansen] dfa sub match, extend kernel match - kernel (low) (2): TODO
[jjohansen] dfa sub match, extend libapparmor match - libapparmor (low) (2): TODO
[jjohansen] dfa sub match, extend expr tree - parser (low) (1): TODO
[jjohansen] dfa sub match, extend dfa creation - parser (low) (1): TODO
[jjohansen] dfa sub match, extend minimize - parser (low) (1): TODO
[jjohansen] dfa sub match, unit tests (low) (2): TODO
[jjohansen] dfa sub match , regression tests (low) (3): TODO
```

```
[jjohansen] dfa backrefs, extend kernel match - kernel (low) (2): TODO
[jjohansen] dfa backrefs, extend libapparmor match - libapparmor (low) (2): TODO
[jjohansen] dfa backrefs, extend expr tree - parser (low) (1): TODO
[jjohansen] dfa backrefs, extend dfa creation - parser (low) (1): TODO
[jjohansen] dfa backrefs, extend minimize - parser (low) (1): TODO
[jjohansen] dfa backrefs, unit tests (low) (2): TODO
[jjohansen] dfa backrefs, regression tests (low) (3): TODO
```

```
[jjohansen] dfa counting constraints, extend kernel match - kernel (low) (2): TODO
[jjohansen] dfa counting constraints, extend libapparmor match - libapparmor (low) (2): TODO
[jjohansen] dfa counting constraints, extend expr tree - parser (low) (1): TODO
[jjohansen] dfa counting constraints, extend dfa creation - parser (low) (1): TODO
[jjohansen] dfa counting constraints, extend minimize - parser (low) (1): TODO
[jjohansen] dfa counting constraints, unit tests (low) (2): TODO
[jjohansen] dfa counting constraints, regression tests (low) (3): TODO
```

```
[jjohansen] dfa hfa, extend kernel match - kernel (low) (2): TODO
[jjohansen] dfa hfa, extend libapparmor match - libapparmor (low) (2): TODO
[jjohansen] dfa hfa, extend expr tree - parser (low) (1): TODO
[jjohansen] dfa hfa, extend dfa creation, border state selection - parser (low) (10): TODO
[jjohansen] dfa hfa, extend minimize - parser (low) (2): TODO
[jjohansen] dfa hfa, extend compression - parser (low) (5): TODO
[jjohansen] dfa hfa, unit tests (low) (2): TODO
[jjohansen] dfa hfa, regression tests (low) (3): TODO
```

```
[jjohansen] per kernel/features set cache files - parser (low) (1): TODO
[jjohansen] per kernel/features set cache files - init scripts (low) (1): TODO
[jjohansen] per kernel/features set cache files - tests (low) (1): TODO
```

```
[jjohansen] documentation, revised techdoc (low) (3): TODO
[jdstrand] documentation, edit revised techdoc (low) (1): TODO
[jjohansen] documentation, on handling disconnected paths in chroots and namespaces (low) (1): TODO
[jjohansen] documentation, RBAC (low) (3): TODO
[jjohansen] documentation, Intro (low) (?): TODO
[jjohansen] documentation, Tutorials, confining a user (low) (?): TODO
[jjohansen] documentation, Tutorials, setting up namespaces (low) (?): TODO
[jjohansen] documentation, policy reference manual (low) (10): TODO
```

```
[jjohansen] upstream, audit, make audit logging ns aware - upstream (low) (?): TODO
[jjohansen] upstream, audit, make audit logging ns aware - kernel (low) (?): TODO
[jjohansen] upstream, audit, make audit logging ns aware - regression tests (low) (?): TODO
[jjohansen] upstream, audit, make audit logging ns aware - documentation/man pages (low) (?): TODO
```

```
[jjohansen] Xace plugin (low) (?): TODO
[jjohansen] merge tool (low) (?): TODO
[jjohansen] aa-analysis tool (help make abstractions) (low) (?): TODO
[jjohansen] policy lint (low) (?): TODO
```

```
[jjohansen] GUI management tool (enable, disable, complain) (low) (?): TODO
[jjohansen] GUI profile creation/editing (low) (?): TODO
```

```
[jdstrand] stacking - update wiki documentation to include stacking information (low) (2): TODO
[jdstrand] labeling - pull hair out fixing documentation (low) (3): TODO
[jdstrand] aa-notify rate limiting (low) (1): TODO
[jdstrand] aa-profile-dump (low) (0.5): TODO
[jdstrand] aa-diff (low) (0.5): TODO
aa-notify - properly privilege separate: TODO
[jdstrand] verify/port python-libapparmor to python3 (low) (2): TODO
[jdstrand] base policy introspecition interface - documentation/man pages (low) (0.5): TODO
```

```
aa-notify - summary report/rate limit - instead of just eliminating dups if there are a lot of outstanding message output a summary message like
        firefox 10 - denials
aa-notify - allow filtering of which messages get sent to notifications (probably allow specifying filters in apparmor/notify.conf
```

```
[tyhicks] dbus - userspace tools (medium) (2)
[tyhicks] dbus - userspace tools unit tests (medium) (2)
```

Old Roadmap (needs to be examined and folded into new itemized list)
====================================================================

This is a list of extensions and new tools, it will at times have some overlap with [ work items](WorkItems "wikilink"), which are outstanding improvements to be made against the current implementation/feature set.

If you are looking for smaller, defined units of work see the [ work items](WorkItems "wikilink") list.

Towards AppArmor 3.0
--------------------

The development roadmap is working towards an AppArmor 3.0 release, which is a mythical version of AppArmor at which full functionality is achieved and we consider breaking things in order to fix it.

### AppArmor 2.5

current development, minor release bump

-   dfa optimizations
-   code cleanup (upstreaming)
-   ptrace
-   audit and control improvements
-   policy improvements
-   apparmor-notify improvements
-   pam\_apparmor (change\_profile)
-   move project <wiki, mailing list, hosting>

### AppArmor 2.6

[crowdsourcing](CollaborativeProfileDevSpec "wikilink")

no features targeted yet, possible features

-   support for extended permission set
    -   create, delete, chmod
-   improved conditionals
    -   full control of owner (and group), eg. owner=(foo,bar)
    -   conditionals based on file type ie. can make creat condition on node type
    -   kernel variable tables for conditionals
-   extended capabilities
    -   chroot
    -   chown
    -   ptrace
    -   setuid
-   in match rule dynamic variables /proc/@{PID}/foo
-   delegation
-   improvements to parser
-   improvements to user tools
    -   tunables
    -   aliases
-   profile namespace support
-   improve notification mechanisms
-   how to handle cap and network address family number mappings
-   testsuite
    -   log auditing
    -   kernel auditing
-   also search supplementary groups in pam\_apparmor

### AppArmor 2.7

Tools
-----

### Parser

-   replace/cleanup front end
-   split into separate tools
    -   common parsing library
    -   common computation library
    -   common interface (loader) library
    -   compiler
    -   loader
    -   verifier/analysis tool

### New tools

-   profile merge tool
-   gui profile editing tool
-   gui control tool (init script interface)
-   new learning tools (replace genprof/logprof)
    -   terminal (ncurses)
    -   gui
-   static binary analysis for profile generation
-   sandbox generation tool
    -   basic profile setup
        -   (autodep + default rule set)
        -   load profile
    -   setup jail
        -   chroot (no sharing)
        -   union fs/mount + bind mount
            -   share data (control reads with rules)
            -   capture writes
-   Notification
    -   OSD
        -   system bus
        -   direct to user session bus
        -   way to kick off profile update tool
    -   email, ...
-   aa-confine - run cmd confined by a <profile>. If <profile> does not contain permissions to exec <cmd> then the exec will fail. The profile used must already be loaded in the system unless a template is specified with -t. Eg:

` aa-confine guest_user ls -l /home`

-   aa-top - watch apparmor profiles
-   aa-ls - list file confinement (profiles/permissions)

Front End (User space)
----------------------

Features that are can be completely implemented in user space with current backend

-   x dominance analysis - allowing for overlapping x rules
-   pcre based rules (expose more matching ability)
-   rule subtraction or some such allowing for more expressive rules

Back End (Kernel)
-----------------

The back end (kernel module) exposes to the front end a set of features, that the front end then maps to and leverages. As such the back end doesn't generally have new features that can be implemented separate from the front end. There are however general cleanups that can be done in the [ work items](WorkItems "wikilink") list.

-   No blockers for moving upstream Linux. Have commitments it will go in once all issues are addressed. Work is ongoing and pushed upstream for (re)review. Inclusion is 2.6.36 is expected.
-   BSD kernel backend? Is it possible, is it desirable. Would be a new implementation.
-   verify/deal with btrfs snapshots

Language/features improvements
------------------------------

-   new language - possibly move to a new language?
    -   separate out x rules from others
    -   remove m?
    -   permissions come before filenames?
-   direct learning connection to kernel (netlink)
-   multiple names for a profile (extend upon regex profiles)
-   profile aliasing - share profile from different namespaces etc.
-   delegation
-   kill rules
-   extended conditionals rules
-   extended capability mediation
-   dynamic loadable variables
    -   per namespace
    -   per user
    -   per profile
-   user definable profiles
-   networking
    -   stage1 socket, outbound control
    -   table based rules
    -   labelled packets handled by ipc
-   ipc
    -   per ipc type control
    -   generic passing of info from/to domain
    -   labelled network communication
-   mount mediation
-   chroot (improvements to chroots are handled)
-   disconnected paths
-   filesystem namespaces (bind mounts)
-   keys
-   regexs in includes (e.g. include
    <dir/*.conf>
    )

Documentation
-------------

-   update techdoc
-   wiki cleanups
-   policy language document
-   bring man pages up to date

Resource usage improvements
---------------------------

-   Policy memory usage and computation time (high)
    -   Ideas
        -   could improve with table compression
            -   better memory usage, speed improvements
            -   need to evaluate different methods, potentially switching away from a strict DFA
        -   kernel memory usage multiple large compressed dfas
        -   user space memory usage, rules, expression trees, uncompressed dfas, compressed dfas
        -   dominant part of policy compilation is dfa compressions
    -   Possible DFA short term improvements
        -   improve compression speed (parser work)
        -   share dfa (parser work)
        -   improve dfa creation speed with set hashing
    -   Possible DFA medium term improvements
        -   DFA boolean operations and caching of partial compiled state
    -   Possible DFA long term improvements
        -   new compressions format
        -   hybrid dfas?
-   Parser caching (high)
    -   how to store files and time stamps in the cached files?
    -   need to cache the metadata about the on-disk profiles used to generate a cache
    -   sharing the parser's internal state when generating multiple profiles that share common abstractions
        -   cuts down on memory usage (~50%) and compile time
        -   needs some parser changes
-   atomic multiple profile load (low)
    -   currently doing 1 at a time, want to load multiple at once (isomorphs generated during multi-profile parsing)
    -   start with profiles with subpolicies like firefox and evince

Convert to default chroot relative profiles
-------------------------------------------

AppArmor 3.0 will move to using chroot relative profiles by
default. This means that profiles will resolve names relative to the
chroot rather than relative to the namespace.

### Goals

-   remove use of kernel ability that may be removed
-   Unify handling of chroot and fs namespaces from a policy pov
-   Deal with the disconnected path problem
-   provide backwards compatibility via flag etc while policy is migrated

### Rational

chroots in the past were handled in a namespace relative manner. That
is the chroot location is prepended to the path name. This is
problematic for several reasons.

-   The ability to find this value may be removed at some point. If this goes away we currently don't have a good fix because of how policy is structured.
-   The profile names are not what are visible to the application
-   profiles used within a chroot are tied to a specific location, and thus not reusable for other locations ie. the prefix that is the chroot location that is appended to every rule
-   profiles intermix pre and post chroot rules
    -   may give post chroot application more privilege than necessary
    -   may give post chroot application access to files outside of the chroot
-   chroots are handled differently than filesystem namespaces. Filesystem namespaces are similar to chroot in concept except there is no way to get the fs location like there is the chroot location. Thus they need a different solution. Ideally chroot and fs namespaces should share a common solution for profile sanity
-   namespace relative chroot profiles don't work for profiles loaded within a chroot (ie. the chroot is a container virtuallizing an OS)
-   Profile stacking means that multiple profiles maybe applied to a single access request. There is a need to minimize path lookup operations which means restructuring the path code, and sharing a single path between all profiles.

### Solution

-   Make profiles chroot relative - this mimics what happens with fs namespaces
-   Use labeling to handle files that are passed in and would result in a disconnected path
-   Use profile and namespace transitions to separate pre and post chroot policy. This structural changes allows profiles to contain just what is necessary pre and post chroot. It also allows better simulation of the current namespace relative behavior
-   Use namespace\_relative profile flag for backwards compatability where necessary
-   Use kernel variables on the security context to store chroot location to allow continuing to offer “namespace\_relative” compatibility

### Problems

-   With chroot relative paths, paths outside of the chroot but in the namespace are still disconnected. Many chrooted apps have files that are opened and then past into the chroot. This needs a labeling or other solution
-   breaks current applications using chroot and profiles
-   breaks current unconfined behavior. System profiles are applied to applications run from within the chroot.

### Work

-   Flag to support old behavior, namespace\_relative (done)
-   Parser patch to default profiles to use chroot relative profiles (done)
-   Kernel patch to default complain and unconfined profiles to chroot relative (done)
-   implicit labeling to support disconnected paths
-   chroot rules
-   kernel variables?
-   Design for how unconfined should handle chroot and namespace transitions
    -   parser implementation
    -   kernel implementation
    -   tools implementation
    -   restructure policy to the pre/post chroot split

Potential Features
==================

-   namespace controls (nice to have)
    -   number and size of profiles loaded
    -   masking off certain permissions for profiles
    -   user profile load?
-   kernel user interface
    -   netlink learning/logging interface -- to not flood logs (high)
        -   move “complain mode” to netlink effectively
    -   new sysfs style layout of supported features
        -   compatibility patch for the old information files (high)
        -   features directory (high)
        -   new layout of profiles and namespace information
            -   add md5s or shas of profile contents (low)
            -   names of profiles, with replaced character, something else? (low)
-   passing delegation info and cache full permissions of labelling profile (nice to have)
    -   deleted file labeling and exec
        -   passing the fd and deleting caused a problem
    -   deleted files
        -   deleted and not shared, we give a pass. could do something with it
    -   lazy unmount
        -   caching full perms issue
-   exec time revalidation and delegation (medium)
    -   file access time does a revalidation check (currently done)
    -   improves file access times if revalidation happens at exec
    -   slows down exec a bit
    -   loses the ability to pass a file through a child that doesn't have permission
-   networking
    -   pattern matching for labeling (medium)
    -   packet labeling (iptables and secmark rule generation) (low)
        -   has a prerequisite of multiple-profile atomic reload
    -   cipso (nice to have)
-   creds & references, indirection, hashing
-   caching of state on objects? Cache full looked up permission
-   mounts
-   investigate hybrid model
    -   directly integrate file labeling, when to label, when to allow relabeling
    -   mv vs. cp behavior
    -   single dfa, dfa intersection, Advantage single label model
        -   needs caching and merging
        -   needs to preserve previous labelings
-   pam\_apparmor
    -   change\_profile
    -   change\_onexec
