# AppArmor Feature ABI Table of Contents
 - [Policy Feature ABI](AppArmorpolicyfeaturesabi)
 - [Feature ABI in AppArmor 2.x](AppArmor2FeatureABI) (This Document)
 - [Policy Feature ABI Interactions in Depth](AppArmorFeatureABIinteractions)
 - [Policy Feature ABI low level examination](AppArmorFeatureABI)
 - [AppArmor Versioning](Apparmorversioning)
 - [Policy Versioning](AppArmorpolicyversioning)

# AppArmor 2.x

AppArmor 2.x series used a simple global scheme for managing the policy feature abi. All policy regardless of where/when it was authored was compiled with a globally set feature abi. The abi was either set by:
- pinning: where the ```features-file``` option in the parser.conf file
- running kernel feature abi: if feature pinning was not specified the kernels feature abi was used.

Unfortunately not all policy was developed with a given kernel feature abi, resulting in denials and failures when booting into new kernels. Distros could manage this well enough by updating shipped policy during testing of their kernel updates, but for users running custom kernels or policy this could result in unwelcome failures.

In addition the AppArmor 2.x series with the exception of AppArmor 2.13 was limited by use of [policy caching](Apparmorpolicycache) which would only allow policy to be compiled for a single kernel at a time.

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
