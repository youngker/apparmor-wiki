# Introduction

AppArmor is a long lived project and the set of features that are supported have evolved over time, and continues to evolve. Not only does the supported feature set change but there are independent parts: kernel, userspace tools, policy and trusted helpers each co-evolving but with independent changes.

## Kernel
The kernel enforces most features, and supports the userspace.
- can't enforce what it doesn't know
- tries to remain backwards abi compatible

## Userspace tools
- compiles and builds policy that is loaded into the kernel
- glue between policy and kernel/trusted helpers

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
