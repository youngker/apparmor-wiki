# AppArmor Feature ABI Table of Contents
 - [Policy Feature ABI](AppArmorpolicyfeaturesabi)
 - [Developing Profiles with the features ABI](AppArmorpolicyfeaturesDev)
 - [Feature ABI in AppArmor 2.x](AppArmor2FeatureABI)
 - [Policy Feature ABI Interactions in Depth](AppArmorFeatureABIinteractions) (This Document)
 - [Policy Feature ABI low level examination](AppArmorFeatureABI)
 - [AppArmor Versioning](Apparmorversioning)
 - [Policy Versioning](AppArmorpolicyversioning)

# Introduction

# Component ABI interaction

AppArmor is a long lived project and the set of features that are supported have evolved over time, and continues to evolve. Not only does the supported feature set change but there are independent parts: kernel, userspace tools, policy and trusted helpers each co-evolving but with independent changes. In addition the components evolve partially independently, but components from different versions may be used together so its important to understand their interactions and the limitations that this imparts when dealing with apparmor policy enforcement.

kernel feature abi
policy api exported by compiler to policy


# Version support matrix

The following set of matrixes provide a quick reference for how the policy, parser and kernel interact based on the feature abi used.

Where abi-old indicates an older feature abi, abi-current a different feature abi with more features than abi-old, and abi-future support for a newer feature set abi with even more features than abi-current.

---

## abi-old apparmor_parser

|               | abi-old Kernel  |abi-current Kernel | abi-future Kernel |
|:-------------:|:---------------:|:-----------------:|:-----------------:|
| abi-old Policy| enforced        | enforced[^1] [^3] | enforced[^1] [^3] |
|abi-current Policy | fail to compile | fail to compile   | fail to compile   |
|abi-future Policy | fail to compile | fail to compile   | fail to compile   |

---

## abi-current apparmor_parser

|               | abi-old Kernel   | abi-current Kernel     | abi-future Kernel     |
|:-------------:|:---------------:|:---------------:|:-----------------:|
| abi-old Policy | enforced        | enforced[^3]    | enforced[^1] [^3] |
| abi-current Policy | enforced[^2]    | enforced        | enforced[^1] [^3] |
| abi-future Policy | fail to compile | fail to compile | fail to compile   |

---

## abi-future apparmor_parser

|               | abi-old Kernel | abi-current Kernel | abi-future Kernel |
|:-------------:|:------------:|:------------:|:-------------:|
| abi-old Policy | enforced     | enforced[^3] | enforced[^3]  |
| abi-current Policy   | enforced[^2] | enforced     | enforced[^3]  |
| abi-future Policy | enforced[^2] | enforced[^2] | enforced      |

[^1]: as long as kernel supports abi being generated and loaded by the parser

[^2]: policy features not supported by kernel are down graded or not enforced 

[^3]: kernel features not supported by policy feature abi not enforced




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


