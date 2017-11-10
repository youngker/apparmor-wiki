[AppArmor Kernel module interfaces presented to
userspace](Kernel_interfaces)

WARNING
=======

This document is unfinished, and contains information that is in
accurate in that it may have discussion of features that are planned
but not implemented.

AppArmor Technical Overview
===========================

This is a quick technical overview of AppArmor. This page covers a
basic overview of AppArmor technical details and provides a jump off
point for topic specific pages.

-   [Kernel Documentation](TechnicalDoc_Kernel)
-   [DFA documentation](TechnicalDoc_DFA)

AppArmor Basics
---------------

### versions

There are three main versions of the AppArmor security module that
saw any significant Deployment

-   AppArmor 2.0 series (not discussed), based on pcre (Perl Compatible
    Regular Expressions) and some small kernel hacks that could almost
    but not always get the pathname right

-   AppArmor 2.1 - 2.3 series - based on dfa (bespoke Deterministic
    Finite-state Automaton, replaced pcre) and vfs patching

-   AppArmor 2.4+ - based on dfa, creds and security path permission
    hooks

Many of the details of the 2.1 - 2.3 series are the same as the 2.4
series though the kernel code was significantly reworked.

### Components

AppArmor consists of several components

-   kernel module that enforces policy loaded from users space
-   Userspace
    -   initscripts that handle loading and stopping profiles as total policy
    -   apparmor\_parser - profile compiler and loader. It is responsible for converting text profiles into the binary policy, and also loading those binary blobs into the kernel.
    -   libraries - set of userspace libraries providing common functionality to AppArmor tools
        -   libaalogparse - parses auditd/syslogd messages, understands differences in formatting, presents common interface to events
        -   libapparmor - provides stable API for [change\_hat](change_hat), [aa\_change\_hat](aa_change_hat), [aa\_change\_profile](aa_change_profile). [parse\_record](parse_record), and [free\_record](free_record)
    -   AppArmor tools - user facing set of tools for interacting with the lower level bits of AppArmor.
        -   [aa-genprof](aa-genprof)
        -   [aa-logprof](aa-logprof)
        -   [aa-unconfined](aa-unconfined)
        -   [aa-status](aa-status)
        -   [aa-complain](aa-complain)
        -   [aa-enforce](aa-enforce)
        -   [aa-decode](aa-decode)
-   Profile repository - share profiles with other AppArmor users, could also share profiles within an organization

### Basics of Confinement

-   AppArmor only confines tasks for which there is policy defined and loaded into the kernel
    -   Any task that is not mediated by AppArmor is said to be unconfined, and should not be affected by AppArmor
    -   Profile names that are path based can automatically attach to executables with matching names
    -   Executables that are started before a matching profile is loaded will not be confined by AppArmor
    -   Profile names do not have to match executable names but often do, due to attaching to profiles from unconfined.
-   Tasks may be [confined by multiple profiles](Task_Confinement)
    -   When a tasks is confined by multiple profiles it permission set is the intersection of all profiles.
    -   Each profile confining an application behaves independently (ie. as though the other profiles were not also being applied).
-   unconfined profile
    -   AppArmor actually attaches a profile to every task in the system. Tasks that are unconfined have a special unconfined profile attached to them.
    -   Each profile namespace has its own unique unconfined profile.
    -   The unconfined profile is used to find the namespace unconfined tasks are in for profile attachment.
    -   The unconfined profile is explicitly tested for in the kernel so that AppArmor has minimal impact on unconfined tasks
    -   A namespaces default unconfined profile is created when a namespace is created
    -   The unconfined profile can not currently be overriden with profile replacement.
    -   The unconfined profile is equivalent to a profile defined as

        ```
 profile unconfined {
     capability *,
     /**        mrwlk,
     /** pix,

     network,
     ipc *,
 }
        ```

Mediation
---------

AppArmor mediation is based around the profile, which is a named
set of rules that are applied to a task (application). ??? variety
of rule types??? file, network, ipc, capability, rlimits. ??? Not a
set mathematical model???

AppArmor mediation is Applied in addition to DAC and can
not override DAC checks. When AppArmor is combined with
pam\_capability? capabilities can be granted which can override DAC
checks and then AppArmor further restricts.

### AppArmor internal permissions

AppArmor defines a set of internal permissions that is different than
represented in policy.

why no trunc? - minimal value - changing the size of the file,
would allow catching some behaviors but doesn't stop a task from
destroying data. If a task can write to a file it can overwrite the
file with garbage.

mv/cp are equivalent, mv doesn't result in a different labeling than
cp. ?? will this remain true for on disk labels??

why no mv/rename permission? Coould be used to enforce where files
can be renamed. Eg. rename /foo to /bar, - in reality it has minimum
value as it can not be generically enforced from the kernel. renames
within a fs/device/partition could be mediated this way but renames
across devices are handled as copies. Where the kernel can't “see”
the connection of reads and writes being done by user space. Thus a
copy could in many cases be used to circumvent a rename rule. A rename
rule could be useful in forcing the behavior that a file is created in
one part of the file system/device and only ever moved. Ie standard
creation and write are not allowed at the destination, but a rename
is allowed (but his would break copying to the location).

mapping policy based permissions to internal permission - this is
done is ideally done in the parser, but can be done in the loading
interface to provide backwards compatibility with older versions of
permission maps.

### Enforcing Permission Mapping

AppArmor kernel module is a policy engine, the LSM is considered as
the mechanism.

AppArmor does not provide a one to one mapping of DAC permissions to
AppArmor permissions

A permission request is done by mapping LSM functions and parameters
to an AppArmor permission set.

### File object mediation

AppArmor file object mediation is path based but has been slowly
evolving towards a hybrid of path and label mediation. Currently
AppArmor's uses DAC labeling of files (owner of file, ...) as a
secondary condition beyond path name to determine access rights. Using
xattr based labeling as another condition is possible and AppArmor
will be extended to use them in the future.

rules order independent To determine access permissions to a file ???

Once file access rights are determined opened file descriptors are
labeled with profile that they were opened under (dynamically labeled,
note this label is not written to disk). This labeling is then used
as the primary label for the lifetime of the file descriptor even if
the on disk file object is moved (AppArmor does not do any revocation
at this time). Any further access checks for the file descriptor are
done against the primary label and if this fails, then a secondary
revalidation check against the files current name can be done to
determine access rights.

revocation/revalidation - at exec time, reload of profile, change\_hat

AppArmor uses the path of file objects based mediation as part of its
controls. The path of the obPaths are used at open and to revalidate.

- label open files with profile that opened them - delegation

- revalidation and change\_hat

#### AppArmor path rules from a labeling perspective

There are two ways to think of AppArmor file rules as labels, either
the path as the label or unique dfa accept states as a label.

##### Path as a label

The path can be thought of as a label - multiple labels per file

##### Dfa state as label

The best equivalent AppArmor has to a label would be the unique accept
state in a dfa. - can intersect profiles to find shared states (labels)
- can analyze total policy this way

#### Revalidation

-   when a file object is accessed that has not been delegated and
    has not been labeled to match - Relooks up path, treated like
    opening file at that instant

#### Mediation without the path

-   network rules - labeling via profile (sid)

### Capabilities

AppArmor capability rules allow selective access to a tasks capability
set.

```
 capability dac_override,
```

##### Extended capability rules

The extended capability rules allow further restricting what is
granted by a capability. Each rule has its own syntax.

Conditionals on owner, user, profile, file type

Examples:

```
 # only allow dac_override on objects belonging to user1, or user2 this include file and ipc
 # conditional capabilities are scheduled for apparmor 3.0
 owner=(user1,user2) capability dac_override,
```

```
 # restricted dac_override
 capability dac_override {
    /file/bar rw,
 }
```

```
 capability chown {
   /file/bar (user1, user2),
 }
```

### Network

#### Network labeling CIPSO

setup network rules to label a socket/packet label is used by ipc rules

uses ipc

### IPC

can be done through file and network rules, and/or dynamic labeling
based (uses profile label on objects to determine if communication
is allowed

```
 ipc rw /profile,
```

Specific forms of ipc can be allowed/disallowed by using more specific ipc rules

```
 ipc signal w (child) /profile,   # only allow sending SIG_CHILD to /profile
 deny ipc signal w (kill) /profile,  # don't allow sending SIG_KILL to /profile
```

### RLIMITS

### Delegation ???

-  basic delegation

-  advanced delegation

Matching of UTF-8
-----------------

user space computation ? -\> filenames are just bytes... but with
e.g. JIS or BIG5, a byte-shift could have large consequences

Userspace Basics
----------------

### Policy

Important files /etc/apparmor.d/

### Initscripts

### Parser

Analyzing AppArmor policy
=========================

- discuss dfa state based analysis, intersecting profiles, etc. - {user,system}x{profile}-&gt;{user,system}x{profile} information flow
