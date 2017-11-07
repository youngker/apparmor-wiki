AppArmor Sandbox
================

**NOTE**: this feature is under development and is not part of any
current release

A sandbox is a way of separating running untrusted applications or
doing testing in isolation.

-   Sandboxes are orthogonal
    -   AppArmor is controlling sharing
    -   Sandboxes creates isolated copies
-   jails (chroots) are traditional sandboxes

Sandboxes in AppArmor
---------------------

AppArmor provides a way to automatically create and control sandboxes
within the AppArmor framework.

Uses
----

-   running untrusted apps
-   testing
-   test install that can be reverted

<!-- -->

-   AppArmor sandboxes are created around union filesystems, or union
    mounts (it will be possible to substitute chroot). This allows
    for a lighter sandbox that has less cost to setup
-   Controlled sharing
-   copy on write behavior
    -   ability to see exactly what the applications has written
    -   ability to merge back into system as desired
-   ability to do controlled sandboxing of writes (only certain writes
    are sandboxed, others go straight to system).

Disadvantages of COW

-   slower
-   dependent on union filesystems or union mounts

Dynamic Sandbox
---------------

-   launch application with aa-jail (aa-sandbox)
-   create sandbox on-the-fly
    -   auto creates and loads a profile on-the-fly based on
        -   autodep
        -   binary analysis
        -   template
    -   creates mounts namespace/(chroot) on the fly
        -   file locations can be specified or set on the fly, this is where data will be written

Preconfigured Sandbox
---------------------

-   can be launched either by
    -   aa-jail
    -   Just executing the application --- profile name determined by profile attachment
        -   needs kernel call out to userspace + profile tag
        -   needs user side daemon to set up sandbox

Templates
---------

A sandbox template contains a partial profile and rules + other
information used to create a sandbox

Templates need

-   set of paths that are to be mapped to sand box
-   location of sandboxes (templated to user/app maybe)
-   partial profile that provide generic rules for the sandbox
-   other rules will be added by autodep etc

File location
-------------

-   how to determine where the program's data is
-   reusing an old sandbox when starting a program again
-   running multiple instances in different sandboxes

layout

```
 sandbox_dir_name/
    .config
    .profile
    data_dirs_1
    data_dirs_2
```

.config contains

-   program name (profile name)
-   user?
-   template used to instantiate
-   mapping of data\_dirs to what they are in sandbox
    -   could be dynamically recreated but if template has changed may fail

.profile - (optional)

-   profile that was created for the sandbox (including alias rules determined by mappings)
-   can be dynamically recreated

data\_dirs\_X - actual data that was written

Sandboxing and user profiles
----------------------------

how do sandboxes work for user profiles

how does a user use system sandbox templates?

Commands
--------

-   aa-sandbox - launch an application in a sandbox
-   aa-jail

<!-- -->

-   aa-unbox - take a sandbox and make it a regular part of the system, writes become shared etc
-   aa-flatten - same thing? which name

<!-- -->

-   aa-clone - clone a sandbox to a new location to run a new instance in

<!-- -->

-   aa-merge, aa-blend, aa-fuse, aa-meld, aa-join, aa-unite, aa-integrate? - fold sandbox back into system

