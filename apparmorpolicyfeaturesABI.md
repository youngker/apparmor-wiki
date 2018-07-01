# Versioning in AppArmor Table of Contents
- [AppArmor Versioning](Apparmorversioning)
 - [Policy Feature ABI](AppArmorpolicyfeaturesabi)
 - [Policy Versioning](AppArmorpolicyversioning)

# Introduction

AppArmor is a long lived project and the set of features that are supported have evolved over time, and continues to evolve. Not only does the supported feature set change but there are independent parts: kernel, userspace tools, policy and trusted helpers each co-evolving but with independent changes. In addition to the components evolve partially independently but components from different versions may be used together so its important to understand their interactions and the limitations that this imparts when dealing with apparmor policy enforcement.

kernel feature abi
policy api exported by compiler to policy

## Kernel
- The kernel presents a set of features that it can enforce to userspace. This is known as the kernel feature set or abi.
- The kernel can not enforce features that are not compiled into it, but its policy support is flexible and it can do partial policy enforcement. As long as the loaded policy passes the kernels policy consistency checks it can be loaded and enforced to the best of the kernels ability.
- policy using the policydb format can specify rule types the kernel may not be able to enforce.
- the kernel can support older policy that does not support all the features that the kernel supports. In this case the kernel will not enforce features that the policy was not authored for.

## Userspace tools

### Policy compiler (aka apparmor_parser)
The policy compiler is the only part of the userspace tool chain required enable apparmor it converts text policy into the binary policy format and can load the binary policy into the kernel for enforcement.

It is the glue between the policy and the kernel and trusted helpers. For a policy rule to be compiled it must be understood by the parser. The parser however can down grade or ignore policy rules that a kernel does not understand allowing for newer policy to work on older kernel versions.

### Init scripts/systemd

### Libapparmor

### Other userspace tools
The remaining userspace tools are not required for the enforcement of policy but can make an admin or policy authors life much easier. If one of the tools is used with a policy feature it must understand the feature enough to handle parsing the text, so generally newer tools will work with older policy but not visa versa.

## Policy
- limits applications
- often dependent on application versions, OS release versions
- dependent on supported features of userspace and kernel

## Trusted helpers
- similar to kernel for policy, but rely on kernel and userspace support for to help with their policy enforcement.

# AppArmor 2.x

AppArmor 2.x series used a simple global scheme for managing the policy feature abi. All policy regardless of where/when it was authored was compiled with a globally set feature abi. The abi was either set by:
- pinning: where the ```features-file``` option in the parser.conf file
- running kernel feature abi: if feature pinning was not specified the kernels feature abi was used.

Unfortunately not all policy was developed with a given kernel feature abi, resulting in denials and failures when booting into new kernels. Distros could manage this well enough by updating shipped policy during testing of their kernel updates, but for users running custom kernels or policy this could result in unwelcome failures.

In addition the AppArmor 2.x series with the exception of AppArmor 2.13 was limited by use of policy caching which would only allow policy to be compiled for a single kernel at a time.

# AppArmor 3.x
- policy features
- feature pinning
- lesser of kernel features or base 3.0 feature set


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

# Version support matrix

The following set of matrixes provide a quick reference for how the policy, parser and kernel interact based on the feature abi used.

Where V(n-1) indicates an older feature abi, V(n) a different feature abi with more features than V(n-1), and V(n+1) support for a newer feature set abi with even more features than V(n).

---

## V(n-1) apparmor_parser

| Policy   | V(n-1) Kernel   | V(n) Kernel       | V(n+1) Kernel     |
|:--------:|:---------------:|:-----------------:|:-----------------:|
| V(n-1)   | enforced        | enforced[^1] [^3] | enforced[^1] [^3] |
| V(n)     | fail to compile | fail to compile   | fail to compile   |
| V(n+1)   | fail to compile | fail to compile   | fail to compile   |

---

## V(n) apparmor_parser

| Policy   | V(n-1) Kernel   | V(n) Kernel     | V(n+1) Kernel     |
|:--------:|:---------------:|:---------------:|:-----------------:|
| V(n-1)   | enforced        | enforced[^3]    | enforced[^1] [^3] |
| V(n)     | enforced[^2]    | enforced        | enforced[^1] [^3] |
| V(n+1)   | fail to compile | fail to compile | fail to compile   |

---

## V.(n+1) apparmor_parser

| Policy   | V(n-1) Kernel | V(n) Kernel | V(n+1) Kernel |
|:--------:|:------------:|:------------:|:-------------:|
| V(n-1)   | enforced     | enforced[^3] | enforced[^3]  |
| V(n)     | enforced[^2] | enforced     | enforced[^3]  |
| V(n+1)   | enforced[^2] | enforced[^2] | enforced      |

[^1]: as long as kernel supports abi being generated and loaded by the parser

[^2]: policy features not supported by kernel down graded or not enforced 

[^3]: kernel features not supported by policy feature abi not enforced


