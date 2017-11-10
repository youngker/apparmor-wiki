Kernel Security Basics
----------------------

-   The basic unix security model is discretionary access control (DAC) and capabilities.

-   The [Linux Security Modules (LSM)](http://en.wikipedia.org/wiki/Linux_Security_Modules) is a
    framework (set of hooks) that allow extending access control to
    the linux kernel via a security module.

#### vfs patching vs. path permission hooks

-   vfs patching used the existing hooks and passed addition
    information `(vfsmnt, file *)` that was available in the vfs down
    into the LSM. The goal being that LSM should have all available
    information when making its decision. Patching the vfs in this
    way was controversial for several reason.
    -   it passed information through vfs routines that did not need it, making them not as clean
    -   it added an extra parameters that didn't have a valid value in some situations
    -   it was a large and invasive patch set

-   path permissions hooks is the solution for path based mediation
    that went upstream. It adds a new hooks that are called with the
    extra information.
    -   to avoid patching the vfs the hooks are added at different locations than the older inode hooks
    -   the new hooks means often two hooks are called to make a security decision for an object
        -   there is no state passed between the hooks to determine if one has already been called
    -   the path hooks do not provide as much coverage as the inode hooks

### LSM

The LSM provides a set of hooks (upcalls) inserted throughout the
kernel to provide extended permission mediation.

Important files

-   security/security.c
-   security/capability.c
-   security/commoncap.c
-   include/linux/security.h

General points to consider

-   The LSM is configured by choosing **CONFIG\_SECURITY=y**
    Security options ---\> Enable different security modules
-   If no security module built in to kernel or registers DAC permissions are used
    -   most DAC checks are hard coded into the kernel
-   Security modules source resides in the linux kernel under the security/ directory
-   Security modules can not be loaded, they are built into the kernel (as 2.6.28? **TODO** double check git commit \#)
    -   Multiple security modules can be built into the kernel
-   An LSM must register with the security framework using **security\_module\_enable**
    -   If multiple security modules are builtin then the first to register wins
        -   **security=kernel** parameter can be used to set which security module to use on boot.
            e.g. `security=apparmor`
        -   AppArmor and SELinux support a parameter to turn them off at boot regardless of other settings
            e.g. `apparmor=0` or `selinux=0`
    -   As of 2.6.33 the default security module can be set via config option (Ubuntu and SuSE have carried patches to due this for several revisions)
    -   Tag module init fn with **security\_initcall** if module needs to start during early boot.
        AppArmor does this in more
    -   There is no generic stacking of security modules. In practice this means that AppArmor and SELinux can not be used at the same time.
        -   There can be module controlled stacking if the modules cooperaterecent versions Ubuntu 9.10 (Karmic Koala)
-   There is a securityfs file system for LSMs to build their own dir fs/dir layout under
    -   Usually mounted at /sys/kernel/security/
        apparmor shows up as /sys/kernel/security/apparmor/
-   In general DAC checks are applied before calling into the LSM
    -   The security modules does not see failures caused by DAC so it can not log them. The audit frame work can be used for this.
    -   some security\_path\_hook come before DAC checks
-   LSM hooks are setup in a vector (include/linux/security.h)
    -   default (capabilities) security module security/capability.c
    -   if a hook entry in the vector is not set by an LSM it is set to match the default provided by capabilities (security\_fixup\_ops)
    -   Generally the LSM is stateless so that most calls to security hooks must be consider individually instead of in the context of which hooks were called before.
-   The LSM has different parts that are conditionally compiled. If AppArmor is selected it will select the components it needs.
-   Different kernel objects have security pointers or SIDs that are used by an LSM to store security info.
-   AppArmor's interface to the LSM is in the file **security/apparmor/lsm.c**


```C
    static struct security_operations apparmor_ops = {
        .name =              "apparmor",

        .ptrace_may_access = apparmor_ptrace_may_access,
        .ptrace_traceme =    apparmor_ptrace_traceme,
        .capget =            apparmor_capget,
        .sysctl =            apparmor_sysctl,
        .capable =           apparmor_capable,
```

### DAC

Discretionary access control (DAC) is the standard unix permission
model. An LSM module can not directly disable DAC security checks,
which means the LSM is not well suited to implementing some potential
security models.

Most DAC checks can be worked around or disabled via the LSMs control
of capabilities.

### Security Pointers & SIDs

Many kernel objects have a `void *security` field which is used
by LSM modules to store security context information. It is up to an
LSM to determine how and when to use them.

On some objects a security id (SID) instead of a `void *` security
pointer is used. This is a unique id that the LSM should be able to
map back to its policy and determine confinement.

AppArmor uses the security field in the tasks cred, files, and will
use the inode security field for none file objects.

AppArmor currently has minimal support for SIDs, and its main current
use is in identifying unique learning profiles.

### Creds

The way the kernel handles a tasks security credentials was changed
in 2.6.29? (**TODO** get commit number). All task credentials got
coalesced into a single shared security structure. This necessitated
a significant change on how AppArmor did locking and managed profiles
associated with tasks.

Important files

-   Documentation/credentials.txt
-   include/linux/cred.h
-   security/apparmor/context.c
-   security/apparmor/context.h

Basics

-   The cred struct is ref counted
-   The task that the cred is assigned to can access the cred without any locking or RCU
-   The cred struct can only be updated by the task it is assigned to
    -   The task must do the update RCU style. ie. no direct modification of any value with in the struct
-   If another task A wishes to access another tasks B cred it must do all access within
    -   An RCU block
    -   with a reference count to the struct held

AppArmor Basics

-   the `cred->security` field points to the AppArmor task\_context
-   the code for dealing with AppArmor interaction with creds is in security/apparmor/context.c
-   AppArmor task\_context is NOT reference counted, it relies on the cred reference count
-   AppArmor does not get the cred reference count except where necessary (ie when task being examined is not current)
-   generally only the code in lsm.c deals with the cred, as AppArmor uses the profile as its unit of confinement

### AppArmor confinement

-   namespaces
-   confined
-   unconfined
    -   unconfined profile
    -   NULL pointer

Path Based mediation
--------------------

### ?? Path discussion

path being valid at that instance of lookup, doesn't matter if racing
as long as pathname generated is consitent and valid for instant
in time.

### Path Generation

Important files

-   security/apparmor/path.c

Basics - vfsmnt, dentry pair needed. Calls into d\_path

d\_namespace\_path

reasons not to do forward lookup

#### Deleted files

#### chroots

#### Disconnected paths

### DFA

Important files

-   security/apparmor/match.c
-   security/apparmor/include/match.h

The dfa is used to perform all path name matching. It does a linear
search transitioning through 1 state per character, when it is done
with the input string it returns the state it finished in. This state
can then be used to start another match or to lookup the permissions
associated with the state.

For matches that require pairs (link pairs) a null transition between
the two matches is used.
