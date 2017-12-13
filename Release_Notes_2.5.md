AppArmor 2.5 Release Notes
==========================

This version of AppArmor continues with the limited development
of new features as was done for AppArmor 2.4. Focusing instead on
code cleanups needed for upstreaming. In general new features added
were to reinstate lost features or add flexibility of policy under
traditionally difficult situations (eg. deleted paths).

-   For information on other versions please see [AppArmor versions](AppArmor_versions)

NOTES
-----

-   This release consists of a kernel patch and a set of user space tools.
-   The kernel patch is against a 2.6.33 kernel, there are also larger patches for the 2.6.31 and 2.6.32 kernels.
-   The kernel provides a SECURITY\_APPARMOR\_COMPAT\_24 option which is required for the AppArmor 2.5 tools to work, it also allows the AppArmor 2.4 tools to be used.

Features removed
----------------

-   set capabilities - this feature is dangerous, has never been
    completed (could not drop caps, and proper checks not done and
    as such should not be used) and is not needed if AppArmor is
    combined with fscaps and the pam\_cap.so PAM module.

New Features
------------

-   Improved kernel module
    -   several code cleanups, bug fixes and general improvemenets
    -   improved policy and dfa and verification
-   file chmod, chown mediation reintroduced - write permission on the file is required
-   change\_hatv - The change\_hatv api allows up to 16 hats to be specified, in order of preference, at the same time. This reduces or eliminates the need of probing for hats using the old change\_hat api.
-   use of capability MAC\_ADMIN to determine ability to modify policy - AppArmor now checks againsts a task's capability set for MAC\_ADMIN to determine if policy modification is allowed. This allows trusted confined applications to modify policy. This replaces the old test of unconfined and root.
-   /proc/<pid>/attr/\* only mediated by DAC and profile rules (no additional restrictions). Previously AppArmor enforced a restriction that only unconfined tasks or the owning task could read from /proc/<pid>/attr/\*, this was an artificial restriction that was not necessary and has been removed.
-   hierarchical namespaces
-   alias allowing mapping to more than a single target
-   new profile flags controlling profile path lookup behavior
    -   chroot\_relative
    -   namespace\_relative
    -   attach\_disconnected
    -   no\_attach\_disconnected
    -   chroot\_attach
    -   chroot\_no\_attach
-   renaming replace - Profile replacements can now rename a profile at the same time as replacing it. This allows for null- learning profiles to be replaced be their new profile.
-   dfa minimization - A minimization stage has been added to policy compilation. This slows down dfa creation some but for larger policies is a net win as it reduces the dfa size and reduces the number of states processed in table compression (which is a much slower operation).
-   improved table compression - The table compression code was tweak so it does less unnecessary comparisons and orders the states such that usually compression is improved slightly.
-   new optimization and debugging flags for dfa creation
    -   -O
    -   -D
-   parallel profile load has been disabled (until the compilers memory use has been improved) as it could push some machines into using swap.
-   libvirt??
-   aa-decode utility - aa-decode is a new utility for decoding log messages, when values have been mangled
-   apparmor\_notifier - the apparmor notifier was rewritten to use the system notification infrastructure.

#### path name lookup and mediation of

Two new profile flags have been introduced to aid in mediation of
disconnected paths. AppArmor's default behavior is to reject new
accesses to disconnected paths reporting back the pathname without
a leadeding **/** Unfortunately this can break some applications,
if a profile must allow for mediation of disconnected paths then
the profile flag attach\_disconnected can be used. This prepend a
leading **/** to the reported name, however this may not result in
the original name of the file as AppArmor can only attach the file
to root, not to its original location.

```
 /some/profile (attach_disconnected) {
   ...
 }
```

### renaming replace

The kernel interface now allows for a renaming replace, allowing for
task to be set to a profile with a different name via replacement. At
this time renaming replace is not supported by the user space tools.

### caching improvements

flags

### dfa debugging flags

### dfa minimization

Changes
-------

-   Audit changes - The kernel records create a the 'c' permission and delete as the 'd' permission.
-   change\_hat - reintroduce ECHILD for when a profile that does not have hats attempts to change\_hat.

Features that did not make it
-----------------------------

Some of the features that were planned for AppArmor 2.5 did not make
it and have been postponed to AppArmor 2.6, though some of them may
be available in a 2.5 point release.

-   AppArmor 2.1 and 2.1+ compatability.
    Much of the work to support this was done but the kernel interface still doesn't do the necessary mapping to support AppArmor 2.1 and 2.1+ dfa permission mappings.
-   chroot rules - the extended rules controlling chroot have not been fully realized
-   Finer ptrace control - the improved control over ptrace did not make it
-   change\_profile pam\_apparmor
    The update of pam\_apparmor to use change\_profile was not available by the cut off so, it will be released as part of 2.5.1
-   caching improvements - the compiled profile cache still does not automatically regenerate when a source file has changed.

Bug Fixes
---------

-   previously alias mapping removed the original rule for from path,
    making it impossible to allow access to anything in the from path.

Ubuntu 10.04 (Lucid Lynx) Release Notes
=======================================

The initramfs based early profile load that was done in Ubuntu 9.10
has been removed.
