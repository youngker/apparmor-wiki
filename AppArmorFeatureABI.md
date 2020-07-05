# Introduction

The AppArmor kernel module exports the set of features it exports to userspace through the features directory hierarchy in apparmorfs (part of security fs). This information can then be used to determine what features are available in the kernel, and how policy should be compiled.

A flattened version of the feature abi directory is used by policy and the apparmor_parser policy compiler. This is used in text policy to specify the [policy feature abi](AppArmorpolicyfeaturesabi) the policy was developed under and to store which kernel feature abi binary policy was compiled for.

# Kernel

The kernel exports the feature abi via the ```features/``` directory in apparmorfs usually located at ```/sys/kernel/security/apparmor/features/```.

```
$ ls -R /sys/kernel/security/apparmor/features/
/sys/kernel/security/apparmor/features/:
capability  dbus    file   namespaces  policy  query   signal
caps        domain  mount  network     ptrace  rlimit

/sys/kernel/security/apparmor/features/caps:
mask

/sys/kernel/security/apparmor/features/dbus:
mask

/sys/kernel/security/apparmor/features/domain:
change_hat   change_onexec   fix_binfmt_elf_mmap  version
change_hatv  change_profile  stack

/sys/kernel/security/apparmor/features/file:
mask

/sys/kernel/security/apparmor/features/mount:
mask

/sys/kernel/security/apparmor/features/namespaces:
pivot_root  profile

/sys/kernel/security/apparmor/features/network:
af_mask  af_unix

/sys/kernel/security/apparmor/features/policy:
set_load  versions

/sys/kernel/security/apparmor/features/policy/versions:
v5  v6  v7

/sys/kernel/security/apparmor/features/ptrace:
mask

/sys/kernel/security/apparmor/features/query:
label

/sys/kernel/security/apparmor/features/query/label:
data  multi_transaction  perms

/sys/kernel/security/apparmor/features/rlimit:
mask

/sys/kernel/security/apparmor/features/signal:
mask
```

# User space flattened representation

User space flattens the directory into a single file that can be more easily processed by the tools. The above feature directory becomes a flattened file of

```
query {label {multi_transaction {yes
}
data {yes
}
perms {allow deny audit quiet
}
}
}
dbus {mask {acquire send receive
}
}
signal {mask {hup int quit ill trap abrt bus fpe kill usr1 segv usr2 pipe alrm term stkflt chld cont stop stp ttin ttou urg xcpu xfsz vtalrm prof winch io pwr sys emt lost
}
}
ptrace {mask {read trace
}
}
caps {mask {chown dac_override dac_read_search fowner fsetid kill setgid setuid setpcap linux_immutable net_bind_service net_broadcast net_admin net_raw ipc_lock ipc_owner sys_module sys_rawio sys_chroot sys_ptrace sys_pacct sys_admin sys_boot sys_nice sys_resource sys_time sys_tty_config mknod lease audit_write audit_control setfcap mac_override mac_admin syslog wake_alarm block_suspend audit_read
}
}
rlimit {mask {cpu fsize data stack core rss nproc nofile memlock as locks sigpending msgqueue nice rtprio rttime
}
}
capability {0xffffff
}
namespaces {pivot_root {no
}
profile {yes
}
}
mount {mask {mount umount pivot_root
}
}
network {af_unix {yes
}
af_mask {unspec unix inet ax25 ipx appletalk netrom bridge atmpvc x25 inet6 rose netbeui security key netlink packet ash econet atmsvc rds sna irda pppox wanpipe llc ib mpls can tipc bluetooth iucv rxrpc isdn phonet ieee802154 caif alg nfc vsock kcm qipcrtr smc
}
}
file {mask {create read write exec append mmap_exec link lock
}
}
domain {version {1.2
}
fix_binfmt_elf_mmap {yes
}
stack {yes
}
change_profile {yes
}
change_onexec {yes
}
change_hatv {yes
}
change_hat {yes
}
}
policy {set_load {yes
}
versions {v7 {yes
}
v6 {yes
}
v5 {yes
}
}
}
```

# Binary Policy

The full text feature ABI is not included in the binary (compiled) policy. Instead the version is encoded in the header and then additional information is encoded as needed within the policy encoding.

????

## Policy DB encoding of support for a rule class

The Policy DB state machine encodes binary support for a rule class in the target state of the class or subclass transition.

```mermaid
graph TD:
  A-->B;
  A-->C;
```

```mermaid
graph TD;
  "Start State" -- "Class File" --> File State (read - file supported);
  Start State -- Class Signal --> Signal State (no perms - Signal not supported);
  Start State -- Class Network --> Network State (read - Network supported);
  File State -- / --> File Internal State;
  Class Network -- subclass Unix --> Unix State (no perms - unix not supported);
  Class Network -- subclass IPv4 --> IPv4 State (read - IPv4 supported);
```

This allows the kernel to detect whether the policy had supported a given rule class. The policy can be encoded to have the state transition even if the kernel does not support a given class, in which case the kernel will not know to query the class.

While this encoding allows policy support multiple kernels (both those that do and don't support a given rule class) at a course level it does not provide a fine grained revisions of to policy rule encoding.

### Fine grained rule encoding flags

????
