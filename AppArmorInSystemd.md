WARNING
=======

This documentation is a wip, and currently is just a dumping ground
to collect bits and pieces of info in one place.

Introduction
============

Loading apparmor policy
=======================

???

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

where <confinement> is any profile name, or stack of profile names.

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
 -AppArmorProfile=bad_profile
```

If apparmor is not enabled the AppArmorProfile stanza will be ignored
and the unit will proceed as if AppArmorProfile was never specified.

[|systemd manual](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#)
