Work Items
==========

This is a list of outstanding work items that can be done against the current code base.

Some items are grouped by a targeted feature, others by the subsystem they are in.

For a list of improvements and extensions to AppArmor see the [development roadmap](DevelopmentRoadmap)
# wi list

# Release targets

- [ ] 2.14 - target: late Oct/early Nov 2019
  - [ ] finish review/merge of outstanding merge requests (attachment xattrs)
  - [ ] alpha
  - [ ] release

---

- [ ] 3.0 - target: early spring 2020
  - [ ] prompting and dependencies
  - [ ] policy hash
  - [ ] Build config of default locations for policy, configs, cache, ...
  - [ ] overlay of
    - [ ] policy
    - [ ] configs
  - [ ] cache
    - [ ] fallback if exact match not found
    - [ ] support warn if exact match not found
  - [ ] allow all,
  - [ ] compiler
    - [ ] intersection of kernel abi and specified abi
    - [ ] document abi priotity
  - [ ] when does policy abi override, compiler specified abi, vs defaulting to kernel abi
    - [ ] cleanup warnings
  - [ ] abi
  - [ ] fd interface
  - [ ] multi-query

---

- 3.1
  - tbd

# Notifications: Prompting

### Dependencies

{::comment} 

Use mermaid live editor to rerender dependency graph

https://mermaidjs.github.io/mermaid-live-editor/


graph TD
  Prompting[Prompting] --> KernelWork[Base Kernel Changes]
  Prompting --> ProfileFlags[Profile Flags]
  Prompting --> RulePrefixes[Prompt Rule Prefix]
  ProfileFlags --> KernelFlags[Profile Flags in Kernel]
  KernelFlags --> ParserFlags[Parser support for prompt flag]
  KernelFlags --> KernelPermsCheck[Rework file Perm check]
  ProfileFlags --> UserSpaceFlags[Profile Flags in Userspace]
  UserSpaceFlags --> ParserFlags
  UserSpaceFlags --> UtilsFlags[Utils support for prompt flag]
  Prompting --> ProfilePrefix[Rule Prefixes]
  KernelWork --> KernelLock[Locking Rework]
  KernelWork --> KernelBuffer[Buffer Rework]
  KernelWork --> ObjectDelegationBase[Internal Object Delegation]
  KernelWork --> TypeCache[Type Cache]
  KernelWork --> kernelInterface[kernel interface]
  KernelWork --> ioctluapi[ioctl uapi]
  KernelWork --> fdqueue[fd interface queues]
  KernelWork --> taskqueue[task queues]
  KernelWork --> AuditEventQueue[Queue for prompt audit events]
  Prompting --> UserAPI[libapparmor API]
  UserAPI --> ioctluapi
  UserAPI --> kernelInterface
  UserAPI --> filtering[filter generation]
  filtering --> filterparse[filter rule parsing]
  filtering --> libapparmorre[make re lib available to applications]
  Prompting --> NotifyPolicy[Notification Policy]
  NotifyPolicy --> KernNotifPolicy[Kernel check notify policy]
  NotifyPolicy --> ParserNotifyPolicy[Parser support Notify policy]
  NotifyPolicy --> UtilsNotifyPolicy[Utils support Notify policy]
  NotifyPolicy --> LibNotifyPolicy[Library parse notification audit]
  UtilsNotifyPolicy --> LibNotifyPolicy
  UtilsNotifyPolicy --> ParserNotifyPolicy
  TypeCache --> ObjType[Object Type]
  ObjType --> PermRemap[permission remap]
  KernelWork --> AuditRework[Rework AppArmor Audit]
  ObjectDelegationBase --> TypeCache
  ObjectDelegationBase --> LabelIt[Split Label Iterator]
  RulePrefixes --> ExtendedPerms[extended permissions]
  ExtendedPerms --> PermRemap
  ExtendedPerms --> PrefixKernel[Kernel prefix support]
  ExtendedPerms --> PrefixPolicy[Prefix Support in policy]
  ExtendedPerms --> PermsUnpack[Kernel Unpack extended perms]
  ExtendedPerms --> MovePermPack[Permission Mapping in Backend of Compiler]
  PrefixKernel --> PrefixPolicy
  PrefixPolicy --> ParserPrefix[Prefix support in Parser]
  PrefixPolicy --> UtilsPrefix[Prefix support in Utils]
  PrefixPolicy --> MovePermPack
  AuditEventQueue -->AuditRecordReroute[Reroute events from Audit to Prompt subsystem]
  KernelWork --> AuditDeDup[Dedup audit records]
  AuditRecordReroute --> AuditCache[Caching of Audit records]
  AuditCache --> AuditObject[Audit Record allocation]
  AuditObject --> AuditStack[AuditRecordOffStack]

{:/comment}



![Dependency Graph for Prompting WorkItem](img/prompting-dependencies.svg "Prompting Dependencies")



### Work Items
- [x] `JJ`: Rework Kernel locking to support prompting and realtime
- [x] `JJ`: Rework buffer allocation to support prompting and realtime
- [ ] `JJ`: permission remap work
- [ ] type cache (requires: permission remapping work
- [ ] split label iterator individual component iterators
- [ ] object delegation for prompting
- [x] `JJ`: interface file
- [x] `JJ`: ioctl interface control
- [x] `JJ`: ioctl uapi api
- [x] listener wait queue for tasks waiting on event
- [x] notification message for tasks waiting on reply
- [x] profile prompt flag (requires: profile flags)
  - [ ] use of in kernel permission checks <br>_requires: rework file mediation to use new code_
  - [ ] unpack
  - [ ] abi support flag
  - [ ] audit info for prompt
- [ ] prompt rule qualifiers _requires: extended permissions, profile prompt flag_
  - [ ] ???
  - [ ] unpack
  - [ ] abi support flag
- policy unpack
  - prompt (dendencies: extended permissions, profile flags, kernel: audit rework, object delegation, locking rework, buffer rework, type cache)
    - kernel
      - type cache
  - extended permissions (dependency: kernel permission remap work)
  - profile flags
    - prompt
    - kill + signal control
    - debug
  - audit rework
    - lib update to handle
    - kernel: audit caching dedup
    - kernel: mem off stack, cleanup reduce entries
    - kernel: share info/dedup
  - rule prefixes front end (accept in language but drop/ignore)
    - quiet
    - kill
    - prompt
    - access
    - complain
  - rule prefixes backend (requires: rule prefixes front end, extended permissions)



# By subsystem break down

Most work items cover more than one section of the stack, however there are several smaller items that affect just one area. Document those here so they can be opportunistically picked off.

### kernel
- [x] `Chris`: In kernel raw policy data compression `DONE`
- [ ] `Mike`: split apparmorfs and make it directly mountable <br>_required by: contextless container boot_
- [ ] `Chris`: make apparmorfs dynamic (see nsfs)
- [ ] `JJ`: nnp restrictions via stacking <br>_required by: nnp override rules_
- [ ] generic object delegation <br>_requires: object delegation for prompting_ <br>_required by: delegation_
- [ ] rule delegation <br>_requires: ?_ <br>_required by: delegation_
- [ ] support overlayfs
- [ ] Make label tree have lockless read side
- [ ] Remove profile list _requires: Make label tree have lockless read size_
- [ ] fuzz interfaces
- [ ] Fix mediation to do single path name lookup and share across label iterator
- [ ] task based debug flags
- [ ] rework debug messages into multiple classes, so we can selectively turn on/off debug output
- [ ] virtualize remaining interfaces to policy namespace <br> _requires: ? _<br>_required by: containers, application policy, unpriviliged user policy_
- [ ] split scope & view in code <br> _requires: ?_ <br>_required by: containers, application policy, unprivileged user policy_
  - [ ] rename fns etc to scope, view or ns
  - [ ] implement scope & view functionality
  - [ ] move children profiles to special child namespace
- [ ] Verify profile, ns, etc names meet specification
- [ ] on unpack create empty profiles/labels referenced by policy <br>required by: on unpack, prelink profile lookups, where possible
- [ ] on unpack, prelink profile lookups etc where possible <br>requires: on unpack, create empty profiles/labels referenced by policy
- [ ] support multiple policy namespace elements in single policy blob
- [ ] support removing multiple profiles in a single remove
- [ ] policy resource accounting <br>required by: unprivileged user policy, application policy


### library
  - [ ] remove overlay macro for fn

### tooling

### init
  - [ ] systemd link against libapparmor init, direct early load

### testing
  - [ ] convert test generators away from perl (python)?
  - [ ] convert regression tests to new infrastructure

### policy
  - [ ] refactor apparmor profiles
    - [ ] single upstream tree
      - [ ] distro dirs as branches
    - [ ] move apparmor.d out of userspace release

### infrastructure
  - [ ] `Joy`: top level build target to build whole project
  - [ ] `Joy`: fix --strip not being passed through correctly in build
  - [ ] remove deprecated perl from repo (not perl generated swig interface) (issue ??)
  - [ ] build flags default locations. Make part of `common/`
    - [ ] policy
    - [ ] cache
    - [ ] config
  - [ ] support USE_SYSTEM=1 in parts of build not currently supported)

### misc
  - [ ] update apparmor logo on cii best practices

### multiple elements of the stack (kernel, compiler, testing, utils, library, documentation, ...)
  - policy_hash (apparmor 3.0)
    - kernel:
      - ignore if present
      - export support of ignore
    - parser: compute and compare
    - library: load to strip hash if not supported by kernel
  - text policy
    - kernel: support loading text policy and compress it
    - parser: keep or regen text policy, load into kernel
    - library: provide easy access to text policy interface, support using decompile if text policy interface is not present
  - policy decompile
    - library
      - fns to grab binary policy
      - fns to decompile binary policy
    - tool to dump loaded policy as decompiled text
  - audit
    - make audit flags available in profile
    - new control flags quitet_allow, per class
  - extended conditionals
    - instruction stream for match
    - nnp
    - suid/guid
    - fs subtype for mount
    - uid
  - fine grained network
  - delegation
    - object
    - rule
  - dynamic include (dependency: rule delegation)
  - nnp override rules
  - application/task policy
    - create custom application profiles
      - in current ns vs. requiring unprivileged user ns
    - method to lock profile
      - no permission to load policy once profile is loaded
      - nnp
      - custom lock flag via apparmor similar to nnp but apparmor specific?
    - lib so application can compile and load policy
    - policy management rules
    - control of profile memory
  - early policy load
    - library routines: to setup, mount ...
    - tooling to build caches into initramfs/initrd
    - binary util (example) to load policy
    - init support (systemd)
      - early direct load
      - secondary late phase that can recompile/reload policy
  - criu for labeled and delegated objects








- a//b exec target bug


  


  - proc interface replacement in lib
  - aa-policy
    - default location, overlay support
    - config dir
    - lib interface
    - --load, --replace, --remove
    - option to build policy caches
  - aa-hash
    - hash policy, match to kernel hashes
  - semantic verification of
    - profile names
    - stacks
    - transitions
    - requires: aa_dfa in userspace
  - parser x dominance
• proc interface replacement
• unix upstream
∘ sock peercred conversion
∘ deal stored path
• suse socket mediation performance (tony)
• bigger xindex
• resurrect NS patches
• point releases 2.13.2, 2.12.2, ...
• dfa perm remap
• dfa update
• suid issues
• uid conditionals
∘ attachment conditional
∘ new hook to control and transition based on uid
• jann email about fixing something around ptrace???
• mount work
∘ fix compiler for current mount
∘ fix for new mount api
∘ fix to enable tracking and dealing with mounts and an ns level
• extended conditionals work
• net patches
∘ kernel_t
∘ act_as
∘ integrate with prompting
∘ ...
• sysbot interfaces/profiles
• Tetsuo
• ioctl
• user conditional
• user lsm hooks
• scope & view
∘ proper code division and annotations
∘ add view and scope to ns
∘ finish virtualizing kernel parameters
∘ children profiles become specialized namespace
∘ allow for multiple mounts of securityfs?
‣ maybe move entirely to apparmorfs?
• rbac/user policy
∘ pam_apparmor
• user defined policy
∘ pam_apparmor
∘ sysctl to disable
• application policy
∘ sysctl to disable
• documentation
∘ wi
∘ dev guides
‣ don't allow user writes to locations policy defined for
• could allow user to define scripts that change confinement behavior
• fuzz interfaces (Chris?)
• env filtering
• userspace exec helper (redirect)
∘ env rewriting
‣ add envs, change envs
‣ set LD library overrides so we can have our libraries hook calls
∘ arg processing
• unshare ns on clone
∘ anonymous random named ns
∘ needs: transition at clone
‣ needs lsm hook or abuse of clone hook
• policy author guide
∘ profile attachments, transitions and writable locations note
∘ mount, clone ns manipulation guidelines
∘ handling stacking crossing ns boundaries with different system namespaces
∘ handling user transitions
• unsafe/nnp flags should be separate for p and i in pix




# expanded wi




# kernel: policy blob compression `(DONE)`
  - dependencies: none
  - description: improve kernel memory usage by compressing the policy blobs which are used for dedup and check point and restore.
  - kernel: make transparent to userspace
    - after unpack succeeds, compress blob using gzip, or lz
    - decompress compressed blob when read

- kernel: policy blob: unpack deal with vmalloc limit
  - dependencies: none
  - description: currently policy is loaded as a single large blob which is then copied to kernel mem and unpacked. VMalloc picked up an 8 MB limit, so it is best for larger policy if we handle it in chunks instead of a single large blob.
  - kernel: make transparent to userspace
    - vmalloc picked up a limit, either switch to reading in chunks or method to make larger vmalloc work
      - chunks
        - convert single large buffer allocation into chunks of N size and do copy to user in chunk sizes
        - update unpack to work with chunks instead of single buffer
      - make vmalloc work with larger allocations
        - investigate

- libaa_re: dominance calculation of accept nodes
  - dependencies: permission rework
  - description: enable calculating dominance of a given node over another given node on all accept states
  - libaa_re: add method for calculating dominance

- parser: proper handling of overlapping x permissions
  - dependencies: dominance calculation in parser backend
  - description: make most specific x modifier win in an overlap as long as the overlap has full dominance. If there is a partial overlap it is not clear which modifier should be used so an error should be thrown.
  - parser: add x overlap computation

- parser, libaa_re: fixup exception code for C++11
  - dependencies: none
  - description: the parser throws exceptions for failure during class construction. Resulting in "warning: dynamic exception specifications are deprecated in C++11 [-Wdeprecated]"


- Late permission mapping
  - dependencies: none
  - description: move permission mapping to backend of compiler right before output format encoding
  - parser:
    - move use of native tree permission mapping into front end of the compiler
    - move mapping logic of tree permissions to backend of the compiler

- rule priority
  - dependencies: none
  - description: extend policy to support notion of priority so some rules can override others
  - parser, tools, policy, documentation, tests

- kernel: dfa rework
  - dependencies: none
  - description: rework dfa to an easier to work with format that does more checking
  - kernel:
    - rework dfa unpack

- kernel: map permissions on unpack (extended permission base)
  - dependencies: dfa rework
  - description: move permission remapping from run time to unpack
  - kernel:
    - create perm array 2 * number of accept entries
    - map accept entries to accept array, owner perm mapping to even entries?
    - create index array to use in place of accept entry arrays

- extended support for file permissins

- Move deny into permission set
  - dependencies: extended permission
  - description: current the explicit deny is not carried into the kernel. We need to carry explicit deny as a unique state permission for delegation.
  - kernel:
    - ?
  - parser:
    - ?

- extended x index
  - dependencies: none or extended permissions if doing more than 5 bits
  - description: The xindex is currently limited to 12 entries (4 bits minus 4). It is possible to extend this to 5 bits before extended permission set lands.
  - kernel:
    - update unpack to work with extra bit
  - parser:
    - detect kernel supports extra entries
    - extend tests and coding for extra entries
    - update verify backend/mapping of extra bits

- extend file permissions

- extend perm x mode for intersection rule delegation

- extended conditionals


- multiple: new audit modes
  - dependencies: none
  - description: update stack to support new audit modes
    - quiet_complain
    - ???

- annotate rule sets at compile time so that we can have proper error messages about where things fail

- parser: split out timestamp logic
  - dependencies: none
  - description: currently the timestamp logic for determinig if the cache or policy is good is internal to the parser. split it out to a library api

- parser: policy hashing
  - dependencies: none
  - description: use a fast hash (murmur hash 3) to help detect policy changes, in addition to the timestamp method currently used.

- parser: include caching
  - dependencies: none
  - description: cache include files so we don't have to reload cache files that have already been processed.
  - parser: create cache for include files etc. That can be used instead of accessing the file multiple times.

- parser:include dedup
  - dependencies: parser block context passed in as parameter, include caching
  - description: currently includes are expanded blindly causing the same include to be expanded multiple times. We can detect and cache which includes have been used within a given context
  - parser keep a cache of used includes on block context



- type cache

- lockless label tree lookup

- inode labeling

- mount labeling

- mount point tagging and improved name lookup

- dfa sharing

- precomputed labels A//&B, A//+B

- label aliases

- namespace mappings
  - ???

- policy ABI
  - compiler
  - utils

- audit cleanup 1
  - move audit struct defines early
  - what of struct sharing and multiple structs for different audits
  - audits need grouping

- audit cleanup 2
  - copy lsm audit specific fields into apparmor audit blob
  - move audit blobs to kmem_cache
  - per cpu buffer of allocated blobs
  - move blob allocation to early in hook
    - pull out of per cpu cache or fallback to kmem_cache if necessary
  - attach apparmor blob to lsm_audit at aa_audit time
  - move free to just before hook exit (outside of any aa locks)

- audit cache/message dedup
  - setup cache hash tree
  - copy buffer data to new string that can exist outside of locks
  - push message into tree, update time if already exists, and move to back
  - pop old message if full, or timed out
    - free contents if needed
    - push onto per cpu buffer or kmem_cache

- complain mode daemon
  - allow daemon to open fd to register interest in receiving messages
  - control to set mask for which messages to receive
  - allocate unique id for each message diverted to userspace
  - add messages to cache so we can dedup




- Dynamic Includes
  - kernel:
    - chase down include similar to delegation for permissions
  - compiler:

- Delegation
  - kernel:
    - rework domain label vector of profiles, to be vector of union { profile, vector profile, vector of label} + tag
    - rework label walk macros to check state, push/pop onto iterator
    - update permission routines to compute/carry accumulated perms
    - update parse to handle //+
    - update label print for //+
    - update label match
    - update label compute fns to build conjunctive normal form
    - update attachment to deal with delegation
    - update exec path
    - update fd handling to deal with delegation
    - update inheritance to deal with delegation
    - update fd passing to deal with delegation
    - add delegation api
  - compiler:
  - utils:
    - genprof/logprof
    - aa_status
      - update to handle new //+
  - libraries
    - logparsing
    - delegation apis
  - regression tests
  - documentation



- nested container support
  - kernel:
    - ????

- suid

- nonewprivileges override
  - kernel:
    - abi: export nnp override/conditional is supported
    - add nnp permission
    - check domain transitions using nnp conditional and nnp perm.
    - build new transition based on override
    - update tracked nnp for override so nnp subset test still works
  - parser:
    - update front end to parse
    - build layout for backend compile
    - tests:
      - add language parsing tests
  - utils:
    - update to handle policy updates
    - update to ask override question
  - policy:
    - update language design/documentation
  - library
    - update log parsing
  - regression tests:
    - tests around nnp override and subset behaviors

- mount
- keyring
- ioctl
- pipe
- sysV ipc
- mknod
- seccomp
- binder
- dmesg controls
- audit controls
- task priority, ...


- text policy in kernel
  - dependencies: none required, possibly make it dependent on text policy file cache so it is easier to get the unmodified text policy.
  - description: carry a compressed version of text policy along with raw data that can be introspected from the kernel. It should be optional so that it is not loaded/can be thrown away to reduce memory usage if the desired by the user.
  - kernel
    - export feature interface to indicate text policy is available.
    - potentially use key value store, and just grab a reference to a specific reserved key's data in the store.
    - provide interface to introspect the text policy
  - parser
    - check for support for text policy, or possibly just key/value store
    - keep policy text and push into key value policy storage.


  
- overlayfs
  - ???

extended permissions
extended conditionals



Parser
------

##### General

-   Convert parser to straight C++ compile and then strip
-   Convert parser into front end and library(ies)
    -   Front end, switch and input control
    -   Parsing Layer and semantic check - convert text to parse tree
    -   Mid Layer parse tree manipulation
    -   Back end
        -   rule to compiled struct - DFA/dDFa/XFA
        -   compiled struct matching
        -   compiled struct compression

##### Compatability

-   check for newer kernel version
-   output new split dfa, permission format
-   allow sharing dfa
-   network rules?

##### Caching

-   move cache out of /etc/apparmor.d/ into /lib/apparmor/
-   store off which files are parsed
-   read file list and stat to check for newer files against binary cache timestamp
-   fallback to parsed file stamps to validate cache before doing compute

##### Front end

-   cleanup internal flags and toggles, shouldn't have to special case -S to get output at backend
-   Cleanup iteration of files specified and front driver code
-   permission check update, should be using capable instead of checking for root
-   move profile control symlinks into /etc/apparmor/

##### Parsing

-   make parser reentrant
-   setup multiple entry points to parser, profile, general rule, file rule, network rule, variable, ...
-   Cleanup semantic parsing
-   Cleanup permission parsing of rules

##### Mid

-   convert rules, codomains to C++ classes
-   convert lists, to C++ list iterators
-   convert twalk to C++ container
-   Sort and rule merge cleanup
    -   use iterator sort
    -   cleanup merge comparisons
-   new stage doing front factoring for rules with the same permissions before expr parsing
-   Fix caching loading so its not split across front, and current backend loading of policy

##### EXPR

-   split expr from dfa
-   native expr parsing of AA rules
-   set up shared nodes struct for exprs
-   use single shared eps node, instead of doing allocations?
-   have normalization use single shared eps node so we don't have to do tail, alt direction check on factoring
-   have shared accept states that aren't ref counted but released when common shared struct is released
-   Add more debug to expr tree simplification
    -   number of iterations
    -   number of changes on a given iteration
    -   Find out why expr tree simplfication is taking so long
-   Add short cuts for expr tree simplification, need to reduce recursion and looping
    -   when pulling up alt nodes in normalization - maybe don't need to recurse on each child? Or can recurse less, currently done after each pull up
    -   hashing of node to speedup comparison
    -   character set conversion? Convert nodes into character sets?
    -   Inverted character set conversion? convert inverted sets into standard sets?
-   Fix dump of Nodes into standard C++ redirects
-   Split out as much dfa from expr trees as possible

##### DFA

-   hashing of sets for quicker construction comparison
-   store less dfa info in expr node, store as much intermediate information as possible in the DFA creation routine
-   pull cases into dfa node
-   get rid of dfa level map of state to transition table
-   add routine to clear, expr tree references from dfa, having references back to expr tree is good for debugging but should not be needed in normal use
-   dfa matching routines using computed dfa
-   DFA Set operations
-   Incremental DFA construction, instead of single large regex break
    into multiple, and do minimization on each and then merge. If the
    split is done correctly this should reduce the number of extra
    states created and eliminated

-   Cache precompiled DFA chunks
    -   save off chunk
    -   load chunk
    -   check time stamp
    -   combine chunk into accumulating policy

-   Accept States
    -   Dfa sharing, need to track perms per shared profile/hat and extract perms for each profile/hat after the fact
    -   dfa negative states for permission subtraction
        -   special states for per rule subtraction

-   Minimization
    -   better state comparison
    -   hash partitioning of non-accepting states
    -   For debugging purposes allow starting from 2 basic partitions
    -   For debugging purposes allow minimizing to a single accept state (ie different permissions are not a reason for partitions splitting

-   Compression
    -   Better stats
    -   Reduce overhead of tracking free position list
    -   more options on ordering of rules being inserted
    -   bitmap comparison
    -   hashing schemes to speedup compression?
    -   dfa matching routines using compressed dfa

##### Back End Binary Policy writing

    -   move to C++ stream

Genprof/Logprof
---------------

-   suggest abstractions with variable name matches. Eg, if
    /proc/filesystems is denied, aa-logprof will not suggest
    abstractions/base even though it has @{PROC}/filesystems

-   have aa-logprof only show entries after the 'Last Modified' date
    (which it writes to the profile)

Profile Repository
------------------

-   bleah current implementation is an eye sore

init scripts
------------

pam\_apparmor
-------------

-   update to use change\_profile
-   jdstrand: some form of testing (DONE via QRT)

tomcat\_apparmor
----------------

-   get change\_hat plugin for tomcat working with tomcat 6

mod\_apparmor
-------------

-   ???

kernel
------

-   remove unused dfa code
-   use lsm audit
-   use audit/complain masks instead of a flag
-   hierarchical namespaces
-   path improvements (flags to control)
    -   deleted file
    -   disconnected paths
    -   chroots
-   features interface as sysfs style dir
-   merge common of file\_perm path\_perm
-   multi profile load

Miscellaneous
-------------

-   add aa-reload (or similar) as a wrapper around 'apparmor\_parser -r' with perhaps some added functionality
-   verify fuse mounts with test cases

test suites
-----------

### parser

-   userside matching engine regression. Setup tests of matching against known rules, and strings.
-   userside matching of kernel regression tests (run known request traces against policy used in kernel test except against user side matching).
-   incorporate new 'error' tests from Makefile into testsuite proper
-   check binary output in some way
-   check dump output against what is loaded
-   use more unit testing
-   add capability to run certain tests as root

### kernel

#### regression tests

-   way to mark expected profile on program
-   way to mark supported version on test (error code, pass/fail for each)
-   better/more flexible profile description support (profile generation)
-   better way to share/reuse test for conditionals
-   load/remove/replace tests
-   namespace create/remove tests
-   break tests into different catagories and directories
-   provide easier access to different sets of tests to run subsets
-   procattr read expected value tests
-   supported interface version tests
    -   do basic loads for all supported versions
    -   run tests for supported versions

-   failed name lookup tests (lazy unmounts, deleted files, exec through proc fds, etc)
-   namespace tests
-   named transition tests
-   regex profile name tests
-   change\_profile onexec
-   expand open permissions tests with different flags passed
-   alias tests
-   variable and conditional tests
-   children profiles
-   audit tests
    -   audit rules
    -   quiet
    -   global flags
-   conditional tests
    -   owner
    -   extended owner
    -   group
    -   inode ...
-   expand link tests
-   chroots
    -   flags affecting path resolution
-   mount - extend mount tests, tests for new mount rules
-   bind mounts
-   pivot roots
-   union mounts
-   stacked filesystems
    -   unionfs - tests
    -   aufs
    -   ecryptfs
-   network tests
-   logging tests
-   ipc tests
-   eventually reorganize so this is more like the other suites where the tests are drop in

#### stress tests

-   fix existing broken tests and reenable
-   leak tests
-   add structure to tests

### libapparmor

-   rename so that the test files give an indication of what they do
-   create regression tests for bindings with following priority: first perl, then python, then java and ruby

### mod\_apparmor

-   add upstream tests, but have them disabled by default (since a running apache will be needed)
-   enable the upstream tests in QRT

Documentation
=============

### wiki

-   Release Notes
    -   update and collate into single location

-   overview
    -   walk throughs

-   Technical documentation
    -   10000' view (overview) see above
    -   1000' view course explanation of what happens at walk through level
    -   100' view detail of what is done and why for each part

-   Roadmap

### Man pages

-   \[jdstrand\] write pam\_apparmor
-   write change\_hatv(2)
-   write change\_onexec(2)
-   genprof logprof

### Tech documents

-   update and fold into wiki versions

### Presentations

-   add to wiki

### comparisions

-   comparisons to other security projects
    -   weaknesses and advantages

### Tutorials to write

Once written, these need to be integrated into [Documentation](Documentation)

-   [Extended\_profiling\_example](Extended_profiling_example)
-   [Revising\_and\_fine\_tuning\_policy\_and\_abstractions](Revising_and_fine_tuning_policy_and_abstractions)

### Misc

Once written, these need to be integrated into [Documentation](Documentation)

-   Update [Raise task capabilities](Raising_Task_Capabilities)
-   Write [Confining\_all\_tasks](Confining_all_tasks)
-   Update [AppArmorSandboxing](AppArmorSandboxing) as sandboxing capabilities come online
-   Write [Binary\_Profile\_Language](Binary_Profile_Language)
-   Write [AppArmorLog](AppArmorLog)
-   Write [Controlling\_Profile\_State](Controlling_Profile_State)
-   Write [ExampleProfiles](ExampleProfiles)

