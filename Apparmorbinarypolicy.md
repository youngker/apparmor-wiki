TOC
- [policy caching](Apparmorpolicycache)
- [binary policy](Apparmorbinarypolicy)

# Introduction

In AppArmor 2.13, AppArmor moved from a [policy caching](Apparmorpolicycache) scheme to a binary policy scheme. The binary policy scheme is still a form of caching of text policy but the layout and semantics have changed allowing for improvements that where not possible under the old caching scheme.

# Layout of binary policy

Binary policy is laid out as a forest of directories under a primary directory tree at ```$(location)```. Under $(location) is set of directories with their names based off of a hash of the kernel feature abi set the policy was compiled for followed by a collision number.

```$(location)/hash.collison_number```

For example

![binarypolicy](/uploads/983cea25b0ebd22dc2eed9523096dbf4/binarypolicy.png)

In the above example ```7f01cf2e``` is the hash of the kernel features abi, and ```.0``` is indicates that it is the first cache directory with the feature hash of ```7f01cf2e```. The ```7f01cf2e.1``` is a directory with the same hash as ```7f01cf2e.0``` but a different kernel feature abi set.

With in each binary policy directory is laid out the same as the [policy cache](Apparmorpolicycache) directory with ```.features``` file for the kernel feature set the binary policy was compile for and a file per text policy file that was compiled.


# Binary Policy Validity check

Binary policy is guaranteed to have at a minimum the same checks for validity done for the [policy cache](Apparmorpolicycache). That is the parser will check each files time stamp against the text policy time stamps during a compile.

In addition to the time stamp check, a feature comparison check is done, however unlike [policy caching](Apparmorpolicycache) binary policy will not clear the cache if the features do not match, it will create a new subdirectory with an incremented revision number.
???? move to first check???

With the introduction of AppArmor 3.0 an addition policy hash check was added to ensure that the binary policy is valid.

# Management of binary policy directories
- directory is not removed/cleared when it doesn't match. New dir is created
- left to package management to manage cleanup of dirs
- Binary policy cache should be created before rebooting into new kernel

# Loading policy

Because binary policy is no longer treated as a cache that is cleared it is assumed that a binary policy cache will exist at boot. This means that the init system can load from the binary policy cache directly without doing cache validity checks. This allows for earlier policy loads.

A second phase of boot can be introduced to do validity checks and recompile policy.


# Overlaying of binary policy

![cache-overlay](/uploads/03d11d02d539af5135084d9f1aafc5b9/cache-overlay.png)


- ro images


# Managemnt

## What the parser handles

- default values
- --write-cache to update

## What the init scripts handle

## What packaging is supposed to handle

- refcounting cache dirs, shared between kernels
