# WARNING this doc is a WIP

# Intro

With LSM stacking it is possible to use multiple LSMs in conjunction with each other. With some care it is possible to set up a container with on LSM while the host is confined with another LSM. Unfortunately it is not entirely straight forward to do so and because LSM stacking support and support for namespacing an LSM have landed piecemeal there are many complications. This guide endeavours to document what is need to be able to successfully use run containers with apparmor and another LSM under stacking capable kernel.

At this time there are two [major and minor](how-to-setup-a-policy-namespace-for-containers#major-lsms-explained) LSMs. The following examples assume you are using a kernel that allows AppArmor to stack with another major LSM, the requirements for AppArmor to be able to stack with another major LSM are be different than say stacking smack with selinux, so what is outlined here may not work for other LSM combiniations.

LSM namespacing is another issue that must be considered. AppArmor support policy namespacing but smack and selinux do not currently which limits which LSMs can be used successfully within a container.

This document is split into sections outlining a how to/example for different container situations and followed by more generic detailed information that can be used to adapt the examples to other situations.

# Assumptions
- apparmor userspace
- kernel

# Docker

## apparmor host, apparmor container

 ```
lsm=apparmor
 ```

??? link anchor to setting config/lsm/grub default

### No host policy on container
 ```
  sudo mkdir /sys/kernel/security/apparmor/policy/namespaces/${NS_NAME}
  aa-exec -p ":${NS_NAME}:unconfined" -- ${CONTAINER_CMD}
 ```

and to cleanup after the container has exited
 ```
  sudo rmdir /sys/kernel/security/apparmor/policy/namespaces/${NS_NAME}
 ```

### Host policy applied to container
 ```
  sudo mkdir /sys/kernel/security/apparmor/policy/namespaces/${NS_NAME}
 ```

to add a bounding stack to the current host policy
```
  aa-exec -p "&:${NS_NAME}:unconfined" -- ${CONTAINER_CMD}
```

to specify the host policy and setup the bounding stack
```
  aa-exec -p "${HOST_PROFILE}//&:${NS_NAME}:unconfined" -- ${CONTAINER_CMD}
```

and to cleanup after the container has exited
 ```
  sudo rmdir /sys/kernel/security/apparmor/policy/namespaces/${NS_NAME}
 ```

## apparmor host, smack container

This is not currently possible due to smack not having namespacing support. Any smack policy in the container will also apply to the host.

```
  lsm="apparmor,smack"
 ```

??? link anchor to config...

## smack host, apparmor container

 ```
  lsm="smack,apparmor"
 ```

??? link anchor to config ...


### No host policy on container
 ```
  sudo mkdir /sys/kernel/security/apparmor/policy/namespaces/${NS_NAME}
  lsm-exec -l apparmor -- aa-exec -p ":${NS_NAME}:unconfined" -- ${CONTAINER_CMD}
 ```

and to cleanup after the container has exited
 ```
  sudo rmdir /sys/kernel/security/apparmor/policy/namespaces/${NS_NAME}
 ```

## smack & apparmor host, apparmor container

## smack & apparmor host, smack container

This is not currently possible due to smack not having namespacing support. Any smack policy in the container will also apply to the host.


# LXD

## apparmor host, apparmor container

LXD natively supports using apparmor policy namespaces and policy bounded stacking as of ???.

## apparmor host, smack container

## smack host, apparmor container

# snappy

snappy will use apparmor policy if it is available

## apparmor host, apparmor container

## smack host, apparmor container


# flatpak

????


# Base Requirements

* [A kernel that supports LSM stacking](how-to-setup-a-policy-namespace-for-containers#stacking-kernel-requirements)
* [Authority to create an apparmor policy namespace](how-to-setup-a-policy-namespace-for-containers#authority-to-create-a-policy-namespace)
* [ensure securityfs or apparmorfs is mounted (default location /sys/kernel/security/)](how-to-setup-a-policy-namespace-for-containers#mount-securityfs)

# Setting up the container

There is a basic pattern that can be used, modifications and per container notes document specifics needed for each.

## Assumption

The basic descriptions assume the AppArmor userspace is installed on the host, with or with out policy. Links to further information are provided detailing what needs to be done if this is not the case.

## Basic Pattern

1. Set up the container environment needed by apprmor.
2. [Create an apparmor namespace](how-to-setup-a-policy-namespace-for-containers#creating-an-apparmor-namespace)
3. [Switch the display LSM and put root container task into the apparmor namespace.](how-to-setup-a-policy-namespace-for-containers#starting-the-container-in-the-policy-namespace)

   AppArmor 2.x: ```lsm-exec -l apparmor -- aa-exec -p ":${NS_NAME}:unconfined" -- ${CONTAINER_CMD}```

   AppArmor 3.x: ```aa-exec --setlsm -p ":${NS_NAME}:unconfined" -- ${CONTAINER_CMD}```


# Setting up the environment

What needs to be done to setup the environment for AppArmor in the container will depend on the kernel version and how the container itself is setup. There are some configurations that may not be possible without help from the container.

The 

## loading policy in the container vs. loading policy from outside the container

### loading policy in the container
- requires apparmor enabled interface be available
- requires securityfs/apparmorfs available/mountable
- requires Authority to create policy in the container
- requires transition to policy namespace happen before policy load

### loading policy from outside the container

It is easier to load policy from outside the policy namespace and force the policy on the container. While loading policy on the host has similar requirements as loading policy from with in a container it is usually easier to achieve them on the host and does not require additional setup of the container environment.

- requires apparmor enabled interface be available
- requires securityfs/apparmorfs available/mountable
- requires Authority to create policy be available on the host during container setup

Load policy into the namespace

transition to profile in namespace


stacking vs. flat





## snappy

Is not required for snappy. Snappy applications don't load their own policy, snapd does it for them.

Can be used on distros that don't use apparmor as the default major lsm

need the display lsm set

## containers (docker ...)

????

## lxd

Lxd already supports creating apparmor child namespaces.
Requirement when used with user namespaces


# Authority to create a policy namespace

Depends on apparmor and kernel versions

* kernels up to ??? require capability MAC_ADMIN in the user namespace.

* kernels ??? relax this to apparmor policy admin capable due to interaction with other LSMs mediating capability MAC_ADMIN for control of their own policy. IF unconfined apparmor policy admin capable may require cap MAC_ADMIN depending on how the current policy namespace is configured.

* kernels ??? add the ability for users to create/admin their own policy.


# Stacking Kernel Requirements

Caveat: Audit subsystem is not namespaced


## User namespace requirements

if apparmor policy namespaces are used in conjunction with user namespaces. There is a nesting limit.

* 4.??? - single level of apparmor policy namespace (1 child policy namespace below the root) used with the user namespace.

* 4.???? - nesting level of policy namespace must match nesting level of the user namepace.

* 5.3 + apparmor 3.0 no need to set display LSM

## Upstream

There is not yet an upstream kernel capable of stacking apparmor and another major LSM (smack, selinux).

However the necessary LSM stacking configuration options are present in the 5.1 kernel.

 ```
  lsm="yama,loadpin,safesetid,integrity,smack,apparmor"
 ```

or config

 ```
  CONFIG_LSM="yama,loadpin,safesetid,integrity,smack,apparmor"
 ```

??? In 5.1 first major LSM wins

## Stacking Development kernels

??? multiple major LSMs

uses the same kernel params and config as 5.1

 ```
  lsm="yama,loadpin,safesetid,integrity,smack,apparmor"
 ```

or config

 ```
  CONFIG_LSM="yama,loadpin,safesetid,integrity,smack,apparmor"
  ```

or

Modify /etc/default/grub so it looks like the following and run update-grub to update the grub menu. Then reboot.

 ```
# grep GRUB_CMDLINE_LINUX_DEFAULT /etc/default/grub
###GRUB_CMDLINE_LINUX_DEFAULT=""
GRUB_CMDLINE_LINUX_DEFAULT="security=smack,apparmor"

# update-grub
 ```

After reboot,

## Ubuntu Kernels

Some Ubuntu kernels carry a version of the LSM stacking patch set and apparmor necessary to use apparmor stacked with another LSM.

### Ubuntu 19.04 (Disco Dingo)

The LSM stacking patches in 19.04's 5.0 based kernel is a combination of a backport of the 5.1 LSM stacking patch set, some LSM Dev branch patches, and reverts of secid support in apparmor so the full LSM dev branch did not need to be incorporated.

same as LSM Stacking development kernels

 ```
  lsm="yama,loadpin,safesetid,integrity,smack,apparmor"
 ```

or config

 ```
  CONFIG_LSM="yama,loadpin,safesetid,integrity,smack,apparmor"
 ```

### Ubuntu 18.04 (Bionic Badger)

 ```
  security=smack,apparmor
 ```

  CONFIG_????

or

Modify /etc/default/grub so it looks like the following and run update-grub to update the grub menu. Then reboot.

 ```
# grep GRUB_CMDLINE_LINUX_DEFAULT /etc/default/grub
###GRUB_CMDLINE_LINUX_DEFAULT=""
GRUB_CMDLINE_LINUX_DEFAULT="security=smack"

# update-grub
 ```

After reboot,


# Ensuring apparmor is enabled

## apparmor userspace

You can check that apparmor is enabled using the apparmor userspace utils
 ```
 $ aa-enabled
 Yes
 ```
or

 ```
 $ sudo aa-status
 apparmor module is loaded.
 0 profiles are loaded.
 0 profiles are in enforce mode.
 0 profiles are in complain mode.
 0 processes have profiles defined.
 0 processes are in enforce mode.
 0 processes are in complain mode.
 0 processes are unconfined but have a profile defined.
 ```

## low level interfaces

It is possible to check AppArmor's low level interfaces if the apparmor userspace package is not installed. You will need to establish that the module is enabled and that securityfs is mounted and has an apparmor directory or apparmorfs are mounted to be able to load policy.

 ```
  $ mount | grep securityfs
  securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
  $ ls /sys/kernel/security
  apparmor  evm  ima  lsm  tpm0
 ```

or
 ```
  $ mount | grep apparmorfs
  apparmorfs on /apparmor type apparmorfs (rw,nosuid,nodev,noexec,relatime)
 ```

securityfs is almost always mounted on ```/sys/kernel/security/```

 ```
   $ cat /sys/module/apparmor/parameters/enabled 
   Y
 ```
or
 ```
  $ cat $APPARMORFS/enabled
  Y
 ```
where $APPARMORFS is either the apparmor directory in securityfs ```$SECURITYFS/apparmor``` or where apparmorfs is mounted.

## ensuring the securityfs filesystem is enabled

For apparmor to be able to load policy its virtual filesystem interface needs to be enabled. This can be done through securityfs or apparmorfs depending on the system. If available apparmorfs might be a better solution inside a container as it can be mult-mounted and doesn't require sysfs or provide other lsm interfaces presented in securityfs.

### apparmor userspace
If the apparmor userspace package is installed the apparmor initscript or systemd unit will ensure that securityfs or apparmorfs is mounted if the apparmor kernel module is enabled.

### no apparmor userpace package on the host

 ```
  mount -t securityfs /sys/kernel/security
 ```

or if apparmorfs is available

 ```
  mount -t apparmorfs /apparmor
 ```

to make it permanent add the following mount rule to /etc/fstab

```
  securityfs /sys/kernel/security securityfs rw,nosuid,nodev,noexec,relatime
```

or if apparmorfs exists in /proc/filesystems you can do
```
  apparmorfs /apparmor apparmorfs rw,nosuid,nodev,noexec,relatime
```
where /apparmor can be replaced with any appropriate location.

# AppArmor Policy namespaces

To be able to use apparmor in the container you must be able to create a policy namespace which implies having the ability to load policy.

hierarchical

AppArmor actually provides two ways for this to happen. Through its
fs interface, and through policy.

## creating via the fs interface

if your task is unconfined by apparmor (it will be if you don't
have policy on the host) and it has cap mac_admin (root). Then
you can do

 ```
  mkdir /sys/kernel/security/apparmor/policy/namespaces/${NS_NAME}
 ```

where ${NS_NAME} is basically limited to alphanum with the first
character being alpha. And unfortunately there is no way to auto
reap apparmor policy namespaces so when your container dies.

 ```
  rmdir /sys/kernel/security/apparmor/policy/namespaces/${NS_NAME}
 ```

## policy

???? todo


# Failures and Issues

* Can't create policy ns
  - apparmor not enabled
  - policy interface not mounted/available
  - No authority to manage policy

* Policy fails to load
  - apparmor not enabled
  - policy interface not mounted/available
  - No authority to manage policy

* Can't transition to policy namespace
  - check that the policy namespace was successfully created
  - check for apparmor denials
    - exec
      - nnp
        - ensure you are using a 4.17+ kernel
      - safe exec
    - mmap failures
    - failure open executable
    - denials leading exec to immediately exit
    - task being killed
    - change_profile failures

* Can't set the display LSM
  - checkout that ```/proc/self/attr/display``` exists
    - if not you need a kernel with the display LSM patches
  - check that the specified LSM exists
  - check that you have permission to write ```/proc/self/attr/display```
  - check that you are not writing another task's ```/proc/self/attr/display```
  - check that if you are using a thread it is writing its own ```/proc/<pid>/attr/display```. Only the lead thread can write to ```/proc/self/attr/display```



AppArmor Enabled
- apparmor built into the kernel
- config param set
- kernel security param set

policy interface mounted

no-new-privs (nnp)

Tasks can the nnp flag through a prctl which prevents the task and its children from gaining new privileges. The nnp flag can prevent apparmor policy confining the task to transition to a new profile.

The restrictions nnp imposses on apparmor policy have changed over time
* 3.5 - nnp added. Profile transitions only allowed if task is unconfined
* 4.13 - nnp restrictions loosend, so profile transition that lead to a strict subset of the current confinement, or add a new element to the stack are allowed.
* 4.17 - nnp restrictions loosend. AppArmor confinement at the time the nnp flag is set is recorded and profile transitions are allowed as long as the resulting transition is a strict subset of the recorded nnp confinement.
* 5.3?? - override rules


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

# The display LSM

The display LSM is how the LSM virtualizes shared interfaces in userspace. The display LSM can be set per task and governs which LSM receives and displays information on shared interfaces. Unfortunately AppArmor, Smack and selinux all share a few user space interfaces.

*  /proc/self/attr/*
*  getsockopt SO_PEERCRED

## Setting the display LSM

Setting the display LSM will affect the task's view of the shared interfaces immediately. The display LSM value will be inherited by the task's children. The task may freely change its display LSM at any time but any changes made after a child is created will not affect the child's view of the shared LSM interfaces.

### apparmor userspace

 ```
  $ lsm-exec -l apparmor -- <cmd to exec>
 ```

 ```
  $ aa-exec --setlsm -p $PROFILE -- <cmd to exec>
 ```

### low level interface
 ```
  fd = open("/proc/self/attr/display", O_CLOEXEC | O_WRONLY);
  if (fd == -1)
      goto fail;
  if (write(fd, "apparmor", 7) != 7)
      goto fail;
  ...
 ```

## When setting the display LSM is needed

AppArmor and Smack have been migrating away from the shared interfaces to use private interfaces which will negate the need for setting the display LSM in the future but setting the display LSM is needed for legacy user space Applications that don't support the new interfaces.

AppArmor 2.x: requires the display LSM be set.

AppArmor 3.x: supports the new private interfaces, available on Kernel 5.3 or later.

Note: some applications (eg. LXD, snapd) use AppArmor's lowlevel interfaces directly instead of going through the libapparmor api. For these applications setting the display LSM may be required even if AppArmor 3 is installed on the system.





# starting the container in the policy namespace

??? Move to proper place


3. Put the root task into the apparmor namespace. You can either
   use aa-exec from the apparmor userspace project
   https://gitlab.com/apparmor/apparmor/blob/master/binutils/aa_exec.c

   with basic usage of
   aa-exec -p ":${NS_NAME}:unconfined" -- bash

   where again you can replace bash

   Alternately you can skip aa-exec by writing

      "exec :${NS_NAME}:unconfined" to /proc/self/attr/exec

   The profile transition to the new namespace will happen at the next
   exec and that task and its children will inherit confinement in
   the policy namespace.

The task is now in you apparmor policy namespace, and assuming it
has cap mac_admin can load the containers policy.

I should note apparmor audit messages go to the audit subsystem which
currently isn't namespaced.

# Major and Minor LSMs explained

The LSM infrastructure provides ???? . An LSM only needs to use the features that it needs to do its job.
* hooks
* security blobs
* mapping of secids
* interfaces

Because the ability to stack LSM modules has landed in the kernel in a piece meal fashion a distinction between which LSMs can be stacked was needed.

Minor LSMs are those that only take advantage of parts of the infrastructure that are fully shareable. Any number of minor LSMs can be stacked. When LSM stacking first landed minor LSMs were only allowed to use LSM hooks.

A major LSM is an LSM that can not stack with another major LSM because it is using parts of the infrastructure that are not shareable. A major LSM can stack with any number of minor LSMs but can not stack with another major LSM. When LSM stacking first landed any use of security blobs, secids, or shared interface would prohibit it from stacking from another LSM using those features.

- table of which features/blobs became shareable at which release.

??? A table of which LSMs are major/minor with which release (Casey slides)

