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
