# AppArmor Feature ABI Table of Contents
 - [Policy Feature ABI](AppArmorpolicyfeaturesabi) (This Document)
 - [Feature ABI in AppArmor 2.x](AppArmor2FeatureABI)
 - [Policy Feature ABI Interactions in Depth](AppArmorFeatureABIinteractions)
 - [Policy Feature ABI low level examination](AppArmorFeatureABI)
 - [AppArmor Versioning](Apparmorversioning)
 - [Policy Versioning](AppArmorpolicyversioning)

# Introduction

AppArmor uses features set ABIs to track what is supported???.

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
Warning from stdin (stdin line 1): ./apparmor_parser: File 'example' missing feature abi, falling back to default policy feature abi.
```

It will then set the profile to use a default feature ABI that is pre v8 networking feature compatible (This means network mediation will not work with upstream kernels but will work with kernel's patched with the out of tree v7 networking patches).

AppArmor 2.x policy should largely just continue to work under the default ABI rule, allowing for a gradual transition to AppArmor 3.x policy.

## ABI rules in include files

Include files can contain a feature ABI rule at their head. The affect of this ABI rule will depend on the apparmor_parser in use and compiler flags.

In AppArmor 3.0 the compiler will check that the include is the same as the feature ABI file declared in the policy (if no abi rule was declared the default ABI is used). If they differ the policy compile will fail.

In the future it may be possible for policy to switch between ABIs during compile but this is not currently possible.


# Developing Policy

Feature ABI rules make policy development harder in that new kernel features are not used right away. Instead to use new features the abi rule must be updated.

AppArmor allows for a special abi rule for policy developers that allows for the old AppArmor 2.x development behavior.

```
   abi <kernel>,
```

Once the policy has been updated to the developer ABI rule, policy development can proceed as normal under AppArmor 2.x. The policy should be reloaded and the kernel will log violations for newly supported features. Once the policy has been updated with an new rules that are necessary. The abi rule must be updated to point to the new abi that is in use.

```WARNING: The AppArmor project will not accept profiles or policy patches that set the abi rule to <kernel>.``` This is to ensure policy works as expected for users and to ensure that the AppArmor project will be able to continue to land new feature support in the upstream kernel.

## How to find the Kernel feature ABI that is in use

???

### Extracting the new abi to an abi file

???

# Why were feature ABI rules added

# Kernel
- The kernel presents a set of features that it can enforce to userspace. This is known as the kernel feature set or abi.
- The kernel can not enforce features that are not compiled into it, but its policy support is flexible and it can do partial policy enforcement. As long as the loaded policy passes the kernels policy consistency checks it can be loaded and enforced to the best of the kernels ability.
- policy using the policydb format can specify rule types the kernel may not be able to enforce.
- the kernel can support older policy that does not support all the features that the kernel supports. In this case the kernel will not enforce features that the policy was not authored for.

## Userspace tools

### Policy compiler (aka apparmor_parser)
The policy compiler is the only part of the userspace tool chain required enable apparmor it converts text policy into the binary policy format and can load the binary policy into the kernel for enforcement.

It is the glue between the policy and the kernel and trusted helpers. For a policy rule to be compiled it must be understood by the parser. The parser however can down grade or ignore policy rules that a kernel does not understand allowing for newer policy to work on older kernel versions.

- supports older feature abis and kernels
- will fail on policy it doesn't understand.
-- policy using new features needs newer versions of the apparmor_parser

### Init scripts/systemd

TODO

### Libapparmor

TODO

### Other userspace tools
The remaining userspace tools are not required for the enforcement of policy but can make an admin or policy authors life much easier. If one of the tools is used with a policy feature it must understand the feature enough to handle parsing the text, so generally newer tools will work with older policy but not visa versa.

## Policy

TODO see policy links

- limits applications
- often dependent on application versions, OS release versions
- dependent on supported features of userspace and kernel

## Trusted helpers
- similar to kernel for policy, but rely on kernel and userspace support for to help with their policy enforcement.

# Packaging

TODO



# AppArmor 3.x
- policy features
- feature pinning
- lesser of kernel features or base 3.0 feature set

[binary policy](Apparmorbinarypolicy)

packaging and distro recommendations
- install time build of policy
- preshipping policy

# Dealing with different feature abis in policy

conditionals and supports
supports
- kernel_supports
- parser_supports
- policy_supports
- supports


downgrading and ignoring unsupported rules

# Future AppArmor
- allow defining new features in policy
- as long as new feature only uses the exported compiler abi, new features can be supported on older compilers.

??? failing policy load

??? rule downgrades

??? Ignoring rules

