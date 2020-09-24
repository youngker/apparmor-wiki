WARNING
=======

This documentation is a wip, and currently is just a dumping ground
to collect bits and pieces of info in one place.

# Introduction

On systems where systemd is the init system it is responsible for loading apparmor policy, and can start services with a specified apparmor confinement.


# Loading apparmor policy

The loading of AppArmor policy is split into two phases.
- Early policy (systemd v246 and later)
- Late policy

Early policy loads are required to confine systemd, and other early services or services that can not depend on the apparmor unit. Late policy loads are more flexible and can be run in parallel with other systemd units on start up.

## Early policy loads

Early policy must be precompiled binary (cache) that matches the kernel being booted and it must be placed in

```
/etc/apparmor/earlypolicy
```

The cache placed in ```/etc/apparmor/earlypolicy``` is expected to to conform to the per kernel directory hierarchy of regular cache.



????
- Load is not parallel with other units
  - large loads can slow down boot

Does not have to be all of policy



Systemd v246 added the ability to load apparmor policy cache during early boot.
Requirements
- cache must be in a location available during early boot, eg. /etc/apparmor.d/cache or /lib/apparmor/cache.  Cache in /var/cache/apparmor/ can NOT be used.
- cache for the specific kernel being booted must be compiled before boot/reboot into said kernel
- ???? setup


The restart problem
===================

Restarting the apparmor unit should be avoided.

Systemd handles restart as a stop followed by a start. Unfortunately
this removes all apparmor policy from the kernel and results in all
tasks entering the unconfined state. The start will then load new
policy however all tasks in the system will remain unconfined, only
tasks start after the start operation will gain the new confinement.

Using systemd to set the apparmor profile/label
===============================================

It is possible to have systemd specify the apparmor confinement a
service should start with. To do this the following stanza is added
to the systemd unit file

```
 AppArmorProfile=<confinement>
```

where ```<confinement>``` is any profile name, or stack of profile names.

```
 AppArmorProfile=/bin/foo
```

or

```
 AppArmorProfile=:ns1://unconfined
```

or

```
 AppArmorProfile=/bin/foo//&bar
```

or

```
 AppArmorProfile=/bin/foo//&:ns1://unconfined
```

Care must be taken that the referenced profile(s) have already been
loaded into the kernel, and if a policy namespace is specified or
the unit will fail. The stanza can be proceeded by a **-** to ignore
failures in setting the profile confinement.

```
 AppArmorProfile=-bad_profile
```

If apparmor is not enabled the AppArmorProfile stanza will be ignored
and the unit will proceed as if AppArmorProfile was never specified.

For more information see the [systemd manual](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#)


