# AppArmor 2.x ABIs

The AppArmor series used a single feature ABI to determine what features where supported. When policy was compiled the ABI was checked for support of a given feature.

By default the compiler would use the feature ABI presented by the kernel, but the feature ABI could be changed by either specifying a specific ABI to the policy compiler (apparmor_parser) or Pinning the feature ABI.

## Specifying the feature ABI to the policy compiler

## Pinning

## Loading policy into the kernel

The kernel is often updated independent of the userspace. It is not uncommon to find new kernels being used on Linux distro userspaces that are several years old, because of this the apparmor kernel module supports multiple feature ABIs to remain backwards compatible with different userspaces.


enforce features policy wasn't designed for

fail to load policy


## Problems

### Policy not designed for the feature ABI
