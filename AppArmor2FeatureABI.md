# AppArmor Feature ABI Table of Contents
 - [Policy Feature ABI](AppArmorpolicyfeaturesabi)
 - [Developing Profiles with the features ABI](AppArmorpolicyfeaturesDev)
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

The features ABI can be specified to the apparmor_parser using the command option. 

```
  --features-file=<abi file>
```

Where <abi file> is a file containing the feature abi that should be used. This option tells the apparmor_parser to compile policy for that set of features and assumes that the kernel can and will support it. If the kernel does not support the compiled policy it will reject it with a message logged when the policy is loaded.

### AppArmor 2.13

AppArmor 2.13 added two new flags to help with policy compilation

```
  --policy-features=<abi file>
  --kernel-features=<abi file>
```

This allowed the apparmor_parser to provide better controller over how policy features are compiled and caching. The policy-features are primarily used to determine how policy is compiled while the kernel-features are used to control cache generation (hashing) and to catch incompatibilities between the kernel and the policy-features at compile time, instead of load time.

Generally only the --policy-features option should be specified as the apparmor_parser will automatically use the running kernels feature abi. However when compiling policy for a different kernel or building precompiled caches the --kernel-features option maybe used.

Using the older

```
  --features-file=<abi file>
```
is equivalent to specifying the same file to both --policy-features and --kernel-features.


## Pinning

When deploying policy


### AppArmor 2.13

In AppArmor 2.13 it is preferred that the --policy-features option is used instead of the --features-file option. This allows apparmor to keep multiple cache of pre compiled policy, each tailored to a kernel.


--
## Loading policy into the kernel

The kernel is often updated independent of the userspace. It is not uncommon to find new kernels being used on Linux distro userspaces that are several years old, because of this the apparmor kernel module supports multiple feature ABIs to remain backwards compatible with different userspaces.


enforce features policy wasn't designed for

fail to load policy


## Problems

### Policy not designed for the feature ABI
