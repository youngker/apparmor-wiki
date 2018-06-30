# Introduction

When working with AppArmor their are many independent or semi-independent components whos version numbers will not match. Generally version numbering is used to provide a coarse indication of a components progression but does not usually provide an indication of compatibility.

The main components when dealing with apparmor are the kernel module, user space tools (parser, libapparmor, apparmor utils), policy (abstractions, and applications), and applications (both the application and its dependent libraries).

# AppArmor kernel module

The AppArmor kernel module does not have its own version information. It is tied to kernel releases and instead exports a feature abi, and api to userspace.

This largely decouples the kernel version from userspace. A specific kernel version may be patch to include additional features (eg. Ubuntu kernels carried additional patches for several releases), and userspace can take advantage of those features if it knows how to deal with it.

# AppArmor userspace

## apparmor_parser (policy compiler)

## library

## tools and utils

## policy


dependencies
- requires
- suggests
- breaks


profiles can be split into 2 main sets. Base application permissions, configuration permissions, dynamic runtime permission (changes based on inputs).

# Applications versions

AppArmor policy is not tied to an application version, however changes to an application or its libraries may necessitate changes to the apparmor policy.

package with application
system package separate from application
profile repository


[policy feature abi](AppArmorpolicyfeaturesabi)
