# Introduction

AppArmor is a long lived project and the set of features that are supported have evolved over time, and continues to evolve. Not only does the supported feature set change but there are independent parts: kernel, userspace tools, policy and trusted helpers each co-evolving but with independent changes. In addition to the components evolve partially independently but components from different versions may be used together so its important to understand their interactions and the limitations that this imparts when dealing with apparmor policy enforcement.

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

#AppArmor 2.x
- feature pinning
- use kernel features

#AppArmor 3.x
- policy features
- feature pinning
- lesser of kernel features or base 3.0 feature set



??? failing policy load

??? rule downgrades

??? Ignoring rules

# Version support matrix
## V(n-1) apparmor_parser

| Policy   | V(n-1) Kernel   | V(n) Kernel     | V(n+1) Kernel   |
|:--------:|:---------------:|:---------------:|:---------------:|
| V(n-1)  v| enforced        | enforced+       | enforced+       |
| V(n)     | fail to compile | fail to compile | fail to compile |
| V(n+1)   | fail to compile | fail to compile | fail to compile |

+ - as long as kernel supports abi being generated and loaded by V n-1 parser

## V(n) apparmor_parser

| Policy   | V(n-1) Kernel   | V(n) Kernel     | V(n+1) Kernel   |
|:--------:|:---------------:|:---------------:|:---------------:|
| V(n-1)   | enforced*       | enforced*       | enforced*+      |
| V(n)     | enforced        | enforced        | enforced+       |
| V(n+1)   | fail to compile | fail to compile | fail to compile |

* - kernel features not supported by policy feature abi not enforced 

+ - as long as kernel supports abi being loaded

## V.(n+1)

| Policy   | V(n-1) Kernel | V(n) Kernel | V(n+1) Kernel |
|:--------:|:-------------:|:-----------:|:-------------:|
| V(n-1)   | enforced*     | enforced*   | enforced*     |
| V(n)     | enforced*     | enforced*   | enforced*     |
| V(n+1)   | enforced*     | enforced*   | enforced      |

* - kernel features not supported by policy feature abi not enforced 
