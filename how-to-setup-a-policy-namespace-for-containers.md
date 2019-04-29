# Intro

bla bla bla

# snappy

Is not required for snappy. Snappy applications don't load their own policy, snapd does it for them.

Can be used on distros that don't use apparmor as the default major lsm

need the display lsm set

# containers (docker ...)

????

# lxd

Lxd already supports creating apparmor child namespaces.
Nesting requirement with user namespaces

# Stacking Kernel Requirements

## Nesting Requirement

if apparmor policy namespaces are used in conjunction with user namespaces. There is a nesting limit.

* 4.??? - single level of apparmor policy namespace (1 child policy namespace below the root) used with the user namespace.

* 4.???? - nesting level of policy namespace must match nesting level of the user namepace.

## Upstream

There is not yet an upstream kernel capable of stacking apparmor and another major LSM (smack, selinux).

However the necessary LSM stacking configuration options are present in the 5.1 kernel.

  lsm="yama,loadpin,safesetid,integrity,smack,apparmor"

or config

  CONFIG_LSM="yama,loadpin,safesetid,integrity,smack,apparmor"


??? In 5.1 first major LSM wins

## Stacking Development kernels

??? multiple major LSMs

uses the same kernel params and config as 5.1

lsm="yama,loadpin,safesetid,integrity,smack,apparmor"

or config

CONFIG_LSM="yama,loadpin,safesetid,integrity,smack,apparmor"


## Ubuntu Kernels

Some Ubuntu kernels carry a version of the LSM stacking patch set and apparmor necessary to use apparmor stacked with another LSM.

### Ubuntu 19.04 (Disco Dingo)

The LSM stacking patches in 19.04's 5.0 based kernel is a combination of a backport of the 5.1 LSM stacking patch set, some LSM Dev branch patches, and reverts of secid support in apparmor so the full LSM dev branch did not need to be incorporated.

same as LSM Stacking development kernels

  lsm="yama,loadpin,safesetid,integrity,smack,apparmor"

or config

  CONFIG_LSM="yama,loadpin,safesetid,integrity,smack,apparmor"


### Ubuntu 18.04 (Bionic Badger)

  security=smack,apparmor

  CONFIG_????


# AppArmor Policy namespaces

## ensuring apparmor is enabled

## ensuring the securityfs filesystem is enabled

## creating via the fs interface

## policy


# AppArmor Policy on the Host

# Alternate LSM Policy on the Host

# Simultaneous AppArmor & Alternate LSM Policy on the Host


# single level of apparmor policy enforcement


# bounding apparmor policy via apparmor policy stacking





hrmmm, add more specific details about doing this with docker and snappy

Assumes this is being done on host that isn't using apparmor. But could document that situation as well

also worth throwing in some lxd mention, as it has support for this


As Casey says not possible with the current upstream kernel.
You do have a few options currently.

You can use the patchset Casey pointed you at or you can
cherry-pick a reduced size stacking variant from the Ubuntu 18.04
or 19.04 kernels. The patchsets are quite different but they are
effectively equivalent. They cutout patches that are not needed
to stack smack/selinux with apparmor.


Once you have a kernel that supports the stacking patchset. The
setup is fairly easy.

You need to boot with both smack and apparmor enabled. Under the
current LSM stacking and 19.04 kernels your kernel parameter is

lsm="yama,loadpin,safesetid,integrity,smack,apparmor"

or config

CONFIG_LSM="yama,loadpin,safesetid,integrity,smack,apparmor"


and the 18.04 version

security=smack,apparmor

there is a config option as well, but I don't remember it off the
top of my head.


Once you have this you can achieve what you want. You don't need
apparmor policy on the host, and you might not even need an
apparmor userspace, as long as each docker container has its own
apparmor userspace and policy.


For each container you are going to do three things.
0. Make sure securityfs is mounted (default location would be
   /sys/kernel/security/)
1. Create an apparmor namespace
2. Switch the display LSM (this does not exist in current upstream
LSM stacking, but hopefully 5.3)
3. Put the root container task into the apparmor namespace.

there is flexibility in the ordering but if you stick to the above
ordering you avoid some of the potential problems.

1. Creating an apparmor namespace.

AppArmor actually provides two ways for this to happen. Through its
fs interface, and through policy. I am going to assume you want to
skip policy on the host.

if your task is unconfined by apparmor (it will be if you don't
have policy on the host) and it has cap mac_admin (root). Then
you can do

  mkdir /sys/kernel/security/apparmor/policy/namespaces/$(NS_NAME)

where $(NS_NAME) is basically limited to alphanum with the first
character being alpha. And unfortunately there is no way to auto
reap apparmor policy namespaces so when your container dies.

  rmdir /sys/kernel/security/apparmor/policy/namespaces/$(NS_NAME)


2. Switch the display LSM, you basically have to write
  "apparmor" to /proc/current/attr/display

   I've attached a basic utility program (lsm-exec), I use to do this.
   You can rip the code you need from it. Basic usage is

   lsm-exec -l apparmor -- bash

   where bash can be replaced with any executable

3. Put the root task into the apparmor namespace. You can either
   use aa-exec from the apparmor userspace project
   https://gitlab.com/apparmor/apparmor/blob/master/binutils/aa_exec.c

   with basic usage of
   aa-exec -p ":$(NS_NAME):unconfined" -- bash

   where again you can replace bash

   Alternately you can skip aa-exec by writing

      "exec :$(NS_NAME):unconfined" to /proc/self/attr/exec

   The profile transition to the new namespace will happen at the next
   exec and that task and its children will inherit confinement in
   the policy namespace.

The task is now in you apparmor policy namespace, and assuming it
has cap mac_admin can load the containers policy.

I should note apparmor audit messages go to the audit subsystem which
currently isn't namespaced.
