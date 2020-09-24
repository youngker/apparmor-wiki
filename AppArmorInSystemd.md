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

Early policy must be pre-compiled binary (cache) that matches the kernel being booted and it must be placed in

```
/etc/apparmor/earlypolicy
```

The cache placed in ```/etc/apparmor/earlypolicy``` is expected to conform to the per kernel [directory hierarchy of regular cache](https://gitlab.com/apparmor/apparmor/-/wikis/Apparmorbinarypolicy#layout-of-binary-policy).

The early policy cache does not have to contain all of the system policy, only the policy that should be loaded in early boot. The early policy load does not run in parallel with other units so loading a large policy may slow down boot some. However AppArmor will not do a compile of policy during early boot so the load will be as fast as the kernel can load and verify the policy.

When using early policy the user is responsible for either copying cache files into ```/etc/apparmor/earlypolicy/``` or setting up apparmor to write its cache directly to earlypolicy.

### Early policy and boot

If early policy is configured systemd will load policy from ```/etc/apparmor/earlypolicy/``` and then try to set its confinement to the ```systemd``` profile.

If the ```systemd``` profile is not present in the loaded policy systemd will report an error.

```
Failed to change to AppArmor profile 'systemd'. Please ensure that one of the binary profile files in policy cache directory /etc/apparmor/earlypolicy/XXXXX contains a profile with that name."
```

This failure does not mean that early policy is not loaded, just that systemd is not confined by AppArmor policy.

WARNING: Confining systemd with AppArmor policy can break your system. Make sure your policy is good before enforcing any policy on systemd.

### Configuring AppArmor to write cache to ```/etc/apparmor/earlypolicy/```

To have AppArmor write its cache into ```/etc/apparmor/earlypolicy/``` the ```/etc/apparmor/parser.conf``` file needs to be edited. The following lines can be added to the file to turn on caching and writing of policy cache to the desired location.

```
# enable writing the cache
write-cache
# set the cache location
cache-loc /etc/apparmor/earlypolicy/
```

After this is done policy needs to be recompiled. Reloading policy should do this for the current kernel

```
systemctl reload apparmor.service
```

For kernels that are different than the current kernel

```
apparmor_parser -QT --kernel-features=<kernel_features_file> <policy_location>
```

where ```<kernel_features_file>``` is the features file for the kernel that policy should be compiled for and <policy_location> is the location of text policy on your system (eg. ```/etc/apparmor.d/```


## Late policy Loads

Late policy loads is provided by systemd apparmor unit. This unit calls out to the ```apparmor_parser``` to handle loads and policy compiles. This enables the late load to recompile policy if the cache is stale or if the kernel is not one that apparmor has generated policy for before.

The late policy load can run in parallel with other systemd units unless they depend on it.

Late policy loads can not confine services that start before the policy is loaded nor systemd.


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


