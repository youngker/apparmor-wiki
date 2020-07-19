# AppArmor Feature ABI Table of Contents
 - [Policy Feature ABI](AppArmorpolicyfeaturesabi) (This Document)
 - [Developing Profiles with the features ABI](AppArmorpolicyfeaturesDev)
 - [Feature ABI in AppArmor 2.x](AppArmor2FeatureABI)
 - [Policy Feature ABI Interactions in Depth](AppArmorFeatureABIinteractions)
 - [Policy Feature ABI low level examination](AppArmorFeatureABI)
 - [AppArmor Versioning](Apparmorversioning)
 - [Policy Versioning](AppArmorpolicyversioning)

# Introduction

AppArmor 3.0 uses feature set ABIs as part of policy to help provide better compatibility across different systems. The feature ABI being tied to policy allows AppArmor to know which set of features the policy was developed under so that the kernel can be updated to newer versions without fear of breakage due to new features being introduced.


# How this affects Policy

Policy now needs to contain an abi rule to indicate which feature abi the policy was developed under.

```
  abi <abi/3.0>,
```

The abi rule needs to be in the preamble of the profile file before the first include file.

```
abi <abi/3.0>,

include <tunables/global>
profile example {
   ...
}
```

## What if policy is missing an abi rule

The AppArmor 3.0 policy compiler (apparmor_parser) will emit a warning

```
Warning from stdin (stdin line 1): apparmor_parser: File 'example' missing feature abi, falling back to default policy feature abi.
```

It will then set the profile to use a default feature ABI that is pre v8 networking feature compatible (This means network mediation will not work with upstream kernels but will work with kernel's patched with the out of tree v7 networking patches).

AppArmor 2.x policy should largely just continue to work under the default ABI rule, allowing for a gradual transition to AppArmor 3.x policy.

## ABI rules in include files

Include files can contain a feature ABI rule at their head. The affect of this ABI rule will depend on the apparmor_parser in use and compiler flags.

In AppArmor 3.0 the compiler will check that the include is the same as the feature ABI file declared in the policy (if no abi rule was declared the default ABI is used). If they differ the policy compile will fail.

In the future it may be possible for policy to switch between ABIs during compile but this is not currently possible.

# Developing Profiles in with the feature ABI

The addition of a feature ABI means that there are a few extra steps in profile development because new kernel features are not automatically used by policy. For more information please see [Developing Profiles with the features ABI](AppArmorpolicyfeaturesDev) for more information.

# Why were feature ABI rules added

Adding the feature ABI to policy allows AppArmor to better handle a couple of different situations.
* using kernel with support for new features is used on a userspace with policy that wasn't developed to support those features.
* profiles being developed separate from the system's policy and being shipped with an application.

Under AppArmor 2.x releases if a user upgraded their kernel they could find themselves in a situation where AppArmor policy that previously worked now results in denials and application failures. This then required the user to update the policy to work with the new kernel. Distros could deal with by testing and updating policy before shipping a kernel but for users who update or use none distro kernels this could result in frustration and an overall poor user experience.

The user experience is also improved for applications that ship profiles as part of their package instead of being part of the system policy. Under AppArmor 2.x if application profiles where not updated with the system profiles it could result in failures just as with changing the kernel. Even worse many devs were not in a position to update the applications profiles for the different distros the application ships on. With the feature ABI declared as part of the profile AppArmor can now support multiple feature ABIs, allowing application developers to update their profiles as works best for them.

# Does AppArmor 3 still support AppArmor 2.x feature pinning

Yes AppArmor 3 still allows for the features ABI to specified as part of the compiler command line or pinned in the configuration file. The new ordering of how pinning is applied is

1. Pinning of a feature ABI in the config file is applied
2. Command line option --feature-file and --policy-features directly passed to the apparmor_parser can be used to override any feature ABI pinned by the config file in 1.
3. Feature ABIs declared in a profile are applied overriding any Pinning declared in 1 or 2.
4. Command line option --policy-features-override is applied allowing developers to override the Feature ABI declared in policy.

This ordering allows AppArmor 2.x policy to be pinned to a given feature ABI while also allowing newer policy that declares its own feature ABI to co-exist.

AppArmor 3 also introduces the new compiler flag

```
--policy-features-override
```

in addition to the AppArmor 2 supported

```
--feature-file - set both the policy-features abi and the kernel-features-abi
--policy-features - this allows specifying what feature ABI the policy should use separate of what the kernel.

--kernel-features - this allows specifying what feature ABI the kernel will support and is useful when precompiling policy to be shipped as part of a package.
```
