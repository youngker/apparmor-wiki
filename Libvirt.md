Introduction
============

With cloud computing and virtualization becoming more and
more prevalent, improving the security stance for libvirt
users is of primary concern. As of libvirt 0.7.2, AppArmor
support for confining virtual machines is available. This
tutorial is an updated version of [Jamie Strandboge's blog
entry](http://penguindroppings.wordpress.com/2009/11/03/apparmor-svirt-security-driver-for-libvirt/)
announcing the availability of an AppArmor-enabled libvirt in Ubuntu
9.10 (by permission). The libvirt AppArmor driver uses the sVirt
framework within libvirt, which is used to confine QEMU (and KVM)
virtual machines and as of libvirt 1.2.3, libvirt-lxc containers.

Implementation Overview
=======================

First, the libvirtd process is considered trusted and is therefore
confined with a lenient profile that allows the libvirt daemon to
launch VMs or containers, change into another AppArmor profile and
use virt-aa-helper to manipulate AppArmor profiles. virt-aa-helper
is a helper application that can add, remove, modify, load and
unload AppArmor profiles in a limited and restricted way. libvirtd
is not allowed to adjust anything in /sys/kernel/security directly,
or modify the profiles for the virtual machines directly. Instead,
libvirtd must use virt-aa-helper, which is itself run under a
very restrictive AppArmor profile. Using this architecture helps
prevent any opportunities for a subverted libvirtd to change its own
profile (especially useful if the libvirtd profile is adjusted to be
restrictive) or modify other AppArmor profiles on the system.

Next, there are several profiles that comprise the system:

```
 /etc/apparmor.d/usr.sbin.libvirtd
 /etc/apparmor.d/usr.lib.virt-aa-helper
 /etc/apparmor.d/abstractions/libvirt-qemu
 /etc/apparmor.d/abstractions/libvirt-lxc   # libvirt 1.2.9+
 /etc/apparmor.d/libvirt/TEMPLATE           # libvirt 1.2.2 and lower
 /etc/apparmor.d/libvirt/TEMPLATE.qemu      # libvirt 1.2.3+
 /etc/apparmor.d/libvirt/TEMPLATE.lxc       # libvirt 1.2.3+
 /etc/apparmor.d/libvirt/libvirt-<uuid>
 /etc/apparmor.d/libvirt/libvirt-<uuid>.files
```

**/etc/apparmor.d/usr.sbin.libvirtd** and
**/etc/apparmor.d/usr.lib.virt-aa-helper**
define the profiles for libvirtd and
virt-aa-helper. **/etc/apparmor.d/libvirt/TEMPLATE\[.qemu|.lxc\]**
is consulted when creating a new profile when one does not
already exist. **/etc/apparmor.d/abstractions/libvirt-qemu**
is the abstraction shared by all running VMs with
**/etc/apparmor.d/abstractions/libvirt-lxc** for libvirt-lxc
containers. **/etc/apparmor.d/libvirt/libvirt-<uuid>** is
the unique base profile for an individual VM/container, and
**/etc/apparmor.d/libvirt/libvirt-<uuid>.files** contains rules for
the guest-specific files required to run this individual VM/container.

The confinement process is as follows (assume the VM/container has
a libvirt UUID of 'a22e3930-d87a-584e-22b2-1d8950212bac'):

-   When libvirtd is started, it determines if it should use
    a security driver. If so, it checks which driver to use (eg
    SELinux or AppArmor). If libvirtd is both confined by AppArmor
    and configured to use it in /etc/libvirt/qemu.conf, it will use
    the AppArmor security driver

-   When a VM/container is started, libvirtd decides whether
    to ask virt-aa-helper to create a new profile or modify
    an existing one. If no profile exists, libvirtd asks
    virt-aa-helper to generate the new base profile, in this case
    /etc/apparmor.d/libvirt/libvirt-a22e3930-d87a-584e-22b2-1d8950212bac,
    which it does based on
    /etc/apparmor.d/libvirt/TEMPLATE\[.qemu|.lxc\]. Notice,
    the new profile has a profile name that is based on
    the guest’s UUID. Once the base profile is created,
    virt-aa-helper works the same for create and modify:
    virt-aa-helper will determine what files are required for
    the guest to run (eg kernel, initrd, disk, serial, etc), updates
    /etc/apparmor.d/libvirt/libvirt-a22e3930-d87a-584e-22b2-1d8950212bac.files,
    then loads the profile into the kernel.

-   libvirtd will proceed as normal at this point, until just
    before it forks a qemu/kvm/container process, it will
    call aa\_change\_profile() to transition into the profile
    ‘libvirt-a22e3930-d87a-584e-22b2-1d8950212bac’ (the one
    virt-aa-helper loaded into the kernel in the previous step)

-   When the VM/container is shutdown, libvirtd asks virt-aa-helper
    to remove the profile, and virt-aa-helper unloads the profile
    from the kernel

It should be noted that due to current limitations of AppArmor, only
qemu:///system and lxc:// are confined by AppArmor. In practice, this
is fine because qemu:///session is run as a normal user and does not
have privileged access to the system like qemu:///system does.

Basic Usage
===========

To see if libvirtd is using the AppArmor security driver, do:

```
    $ virsh capabilities
    Connecting to uri: qemu:///system
    <capabilities>
     <host>
      ...
      <secmodel>
        <model>apparmor</model>
        <doi>0</doi>
      </secmodel>
     </host>
     ...
    </capabilities>
```

Next, start a VM and see if it is confined:

```
    $ virsh start testqemu
    Connecting to uri: qemu:///system
    Domain testqemu started
    $ virsh domuuid testqemu
    Connecting to uri: qemu:///system
    a22e3930-d87a-584e-22b2-1d8950212bac
    $ sudo aa-status
    apparmor module is loaded.
    16 profiles are loaded.
    16 profiles are in enforce mode.
    ...
      /usr/bin/virt-aa-helper
      /usr/sbin/libvirtd
      libvirt-a22e3930-d87a-584e-22b2-1d8950212bac
    ...
    0 profiles are in complain mode.
    6 processes have profiles defined.
    6 processes are in enforce mode :
    ...
      libvirt-a22e3930-d87a-584e-22b2-1d8950212bac (6089)
    ...
    0 processes are in complain mode.
    0 processes are unconfined but have a profile defined.
    $ ps ww 6089
    PID TTY STAT TIME COMMAND
    6089 ? R 0:00 /usr/bin/qemu-system-x86_64 -S -M pc-0.11 -no-kvm -m 64 -smp 1 -name testqemu -uuid a22e3930-d87a-584e-22b2-1d8950212bac -monitor unix:/var/run/libvirt/qemu/testqemu.monitor,server,nowait -boot c -drive file=/var/lib/libvirt/images/testqemu.img,if=ide,index=0,boot=on -drive file=,if=ide,media=cdrom,index=2 -net nic,macaddr=52:54:00:86:5b:6e,vlan=0,model=virtio,name=virtio.0 -net tap,fd=17,vlan=0,name=tap.0 -serial none -parallel none -usb -vnc 127.0.0.1:1 -k en-us -vga cirrus
```

Here is the unique, restrictive profile for this VM:

```
    $ cat /etc/apparmor.d/libvirt/libvirt-a22e3930-d87a-584e-22b2-1d8950212bac
    #
    # This profile is for the domain whose UUID
    # matches this file.
    #
     
    #include <tunables/global>
     
    profile libvirt-a22e3930-d87a-584e-22b2-1d8950212bac {
       #include <abstractions/libvirt-qemu>
       #include <libvirt/libvirt-a22e3930-d87a-584e-22b2-1d8950212bac.files>
    }

    $ cat /etc/apparmor.d/libvirt/libvirt-a22e3930-d87a-584e-22b2-1d8950212bac.files
    # DO NOT EDIT THIS FILE DIRECTLY. IT IS MANAGED BY LIBVIRT.
      "/var/log/libvirt/**/testqemu.log" w,
      "/var/run/libvirt/**/testqemu.monitor" rw,
      "/var/run/libvirt/**/testqemu.pid" rwk,
      "/var/lib/libvirt/images/testqemu.img" rw,
```

Now shut it down:

```
    $ virsh shutdown testqemu
    Connecting to uri: qemu:///system
    Domain testqemu is being shutdown

    $ virsh domstate testqemu
    Connecting to uri: qemu:///system
    shut off

    $ sudo aa-status | grep 'a22e3930-d87a-584e-22b2-1d8950212bac'
    [1]
```

Advanced Usage
==============

In general, you can forget about AppArmor confinement and just use
libvirt like normal. The guests will be isolated from each other and
user-space protection for the host is provided. However, the design
allows for a lot of flexibility in the system. For example:

-   If you want to adjust the profile for all
    future, newly created VM/containers, adjust
    /etc/apparmor.d/libvirt/TEMPLATE\[.qemu|.lxc\]

-   If you need to adjust access controls for all VM/containers, new or
    existing, adjust /etc/apparmor.d/abstractions/libvirt{-qemu,-lxc}

-   If you need to adjust access controls for a single guest, adjust
    /etc/apparmor.d/libvirt-<uuid>, where <uuid> is the UUID of
    the guest

-   To disable the driver, either adjust /etc/libvirt/qemu.conf to have
    'security\_driver = “none”' or remove the AppArmor profile
    for libvirtd from the kernel and restart libvirtd

Of course, you can also adjust the profiles for libvirtd and
virt-aa-helper if desired. All the files are simple text files.

Limitations and the Future
==========================

While the sVirt framework provides good guest isolation and user-space
host protection, the framework does not provide protection against
in-kernel attacks (eg, where a guest process is able to access the host
kernel memory). Once AppArmor provides the ability for regular users
to define profiles, then qemu:///session can be properly supported.

Summary
=======

With cloud computing and virtualization becoming even more important
in the data center, leveraging technologies like libvirt and AppArmor
is a must. Virtualization removes the traditional barriers afforded
to stand-alone computers, thus increasing the attack surface for
hostile users and compromised guests. By using the sVirt framework
in libvirt administrators can better defend themselves against
virtualization-specific attacks.

References
==========

-   http://libvirt.org/
-   http://danwalsh.livejournal.com/30565.html
-   http://lwn.net/Articles/353970/
-   http://penguindroppings.wordpress.com/2009/11/03/apparmor-svirt-security-driver-for-libvirt/

