AppArmor 2.4 Release Notes
==========================

Kerenel version: 2.6.31

In this version of AppArmor development of new features was
largely halted and the kernel module was rewritten to use the new
path\_permission hooks provided by the LSM. This necessitated some
changes to user space as well and some features were lost.

-   For information on other versions please see [AppArmor versions](AppArmor_versions)

Features removed
----------------

### Set profile interface

The ability for an unconfined process to arbitrarily set a tasks
profile via the /proc/\<pid\>/attr/current interface was removed. This
was done due to changes necessitated by the kernels move to creds,
which no longer allows another task from modifying a given tasks creds.

This interface was used by the profiling tools to change running tasks
from the null-complain-profile to newly created profiles. The move to
unique null-XXXX profiles was designed to circumvent this limitation
but the full support needed to handle this was not implemented in
AppArmor 2.4. To work around this problem applications must stopped
and started during profiling.

### chmod, chown mediation

Path based mediation of chmod and chown was lost due to limitations
in the security\_path hooks

### xattr mediation

Path based mediation of xattrs was lost due to limitations in the
security\_path hooks.

### path mediation of unix domain socket

Path based mediation of unix domain sockets was lost due to limitations
in the security\_path hooks.

New Features
------------

### Regex based profile names

Profile names can now contain regular expressions allowing all profile
to match against multiple binaries.

### pux transition

profile transitions so that x transitions can fall back to unconfined
if a profile is not present

### Better support of profile namespaces

### Basic caching of compiled profiles

### kill mode

### audit control

### /proc/\<pid\>/attr/prev

### change profile on exec

The basics of the ability to delay change\_profile until exec time was
added. This allows change\_profile to be used without stub profiles
for immediate transitions.

change\_profile \<profile\_name\> -&gt; executable?

/proc/\<pid\>/attr/prev

Buggy

Changes
-------

### null-complain-profile replaced by null-XXXX

The use of a single shared null-complain profile to do learning was
replaced with unique null-XXX profiles.

Bugs
----

### Alias rules

Alias rules have the same bug as in AppArmor 2.3 where the alias
rule removes the original path making it impossible to express any
permission for the replaced path.

Ubuntu 9.10 specific Notes
==========================

The init of AppArmor was split into an early profile load done out
of initramfs and a later load as part of system boot. This was done
due to limitations on how dependencies were being handled by upstart
for the start up of some of the networking services which would cause
them to be loaded before the profile was loaded.

-   libvirt???

