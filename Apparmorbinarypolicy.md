TOC
- [policy caching](Apparmorpolicycache)
- [binary policy](Apparmorbinarypolicy)

# Introduction

In AppArmor 2.13, AppArmor moved from a [policy caching](Apparmorpolicycache) scheme to a binary policy scheme. The binary policy scheme is still a form of caching some times of text policy but the layout and semantics have changed allowing for improvements that where not possible under the old caching scheme. Specifically

- binary policy is assumed to exist at early kernel boot so the text policy does not need to be checked
- binary policy exists for each kernel that is booted so there is no clearing of the "cache" when switching kernels
- binary policy can be pre-shipped as immutable policy. With or without the corresponding text policy
- binary policy can still be a cached build of locally modifiable text policy
- binary policy allows for an overlay where for local updates again read-only images

With these changes policy can be shipped using traditional packaging or read-only images, and the init system can load policy from early boot with out having to do text policy compiles or consistency checks. 

# Management of binary policy

## Building of the binary policy cache

### Traditional packaging

### Read-only images

### Read-only images without a kernel

### Multiple policy directories

Some distributions may [split policy into different directories](Apparmorpolicymanagement). In this case different binary policy directory root must be defined for each policy location if there is any chance any of the text policy files in the different locations may have the same name.

If the distribution still wants to use the default recommended binary policy locations then an extra directory level can be added.

Eg.

  $(location)/system/...
  $(location)/snappy/...



Generally it is recommended that policy be compiled at installation time,. For traditional packaging this means building on the host, but for read-only images that include a kernel this means building policy as packages are being installed and the image is being build.


# Layout of binary policy

Binary policy is laid out as a forest of directories under a primary directory tree at ```$(location)```. Under $(location) directory is set of directories with their names based off of a hash of the kernel feature abi set the policy was compiled for followed by a collision number.

```$(location)/kernel_feature_hash.collison_number```

For example

![binarypolicy](/uploads/983cea25b0ebd22dc2eed9523096dbf4/binarypolicy.png)

In the above example ```7f01cf2e``` is the hash of a kernel features abi, and the ```.0``` collision number indicates that it is the first cache directory with the feature hash of ```7f01cf2e```. The ```7f01cf2e.1``` directory, is a directory with the same hash as ```7f01cf2e.0``` but a different kernel feature abi set, each directory contains the full feature set abi file to resolve any hash collisions.

Within each binary policy directory is laid out the same as the [policy cache](Apparmorpolicycache) directory with ```.features``` file for the kernel feature set the binary policy was compile for, and a file per for each compiled text policy file. In the above example ```7f01cf2e.0/bin.ping``` is the compiled binary policy file for the ```$(policy)/bing.ping``` text policy file compiled for a kernel who's feature abi hashes to ```7f01cf2e```. While ```a035ea11.0/bin.ping``` is a binary cache file for the same text policy file, $(policy)/bin/ping, but for a different kernel.

Note that binary policy cache files within the directory correspond to a given text policy file name.

Symlink files within the cache are used to map cache files that 

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


# Dealing with multiple policy locations and caches


# Managemnt

## What the parser handles

- default values
- --write-cache to update

## What the init scripts handle

## What packaging is supposed to handle

- refcounting cache dirs, shared between kernels
