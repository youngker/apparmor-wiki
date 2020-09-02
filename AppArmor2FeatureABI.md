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

Pinning of the feature ABI is a way to lock policy down to a given stable feature set. This is desirable in stable deployments that may have different kernel versions installed through out the deployments life time.

Changes to the kernel and the feature ABI can lead to a few different problems
- Caches may need to be regenerated during boot. If the kernel has any feature changes and userspace only supports a single cache (apparmor 1 - 2.12) a recompile will be needed, which can slow down boot.
- Policy may not have been developed with a given feature in mind resulting in Denials that can break the system. Eg. if a new mediation feature is added to the kernel, and policy doesn't know about it, the policy will compile in such a way as to deny the new feature (default deny) but the system may need this new feature or it will break.

Distros can ensure their policy works with updated kernels that the distro deploys but they can not update out of distro policy, nor make sure their policy is updated for users that roll their own kernels. This can lead to failures and bug reports distro may not want from a stable system.

Pinning the feature ABI allows the admin to set the ABI that the policy was developed under to ensure that caching is not needless regenerated and that new mediation features are not used when the policy has not been updated to support them.

To pin a feature abi the parser.conf file is updated by adding a feature-file option line

eg.
```
  # pin policy to 4.0 kernel ABI
  feature-file=/etc/apparmor/features-ABI
```

Once this is set the apparmor_parser will compile all policy using the feature ABI in the /etc/apparmor/features-ABI file, unless it is explicitly overridded by passing the --features-file option directly to the apparmor_parser on the command line.

### issues

There is a down side to pinning the feature ABI. It prevents new features and mediation from being used in updated policy, and it prevents apparmor from logging potential violations so that policy can be updated. For this reason it is recommended that feature pinning is disabled during the development phase a release and only set before release.

### AppArmor 2.13

In AppArmor 2.13 it is preferred that the --policy-features option is used instead of the --features-file option. This allows apparmor to keep multiple cache of pre compiled policy, each tailored to a kernel.

AppArmor 2.13 also add support for multiple policy caches so that switching kernels does not force policy recompiles.

--
## Loading policy into the kernel

The kernel is often updated independent of the userspace. It is not uncommon to find new kernels being used on Linux distro userspaces that are several years old, because of this the apparmor kernel module supports multiple feature ABIs to remain backwards compatible with different userspaces.


enforce features policy wasn't designed for

fail to load policy

## Shipping prebuilt policy

To prebuild policy --features-file should be used. The feature abi should be for the target kernel or the lowest common denominator if there are multiple target kernels.


## Problems

### Policy not designed for the feature ABI
