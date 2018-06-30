# Introduction

When working with AppArmor their are many independent or semi-independent components and it can be important to understand how they interact. Generally version numbering is used to provide a coarse indication of a components progression but does not usually provide???

Lots of different versioning to deal with

parser, kernel, policy, library, tools,

dependencies
- requires
- suggests
- breaks


profiles can be split into 2 main sets. Base application permissions, configuration permissions, dynamic runtime permission (changes based on inputs).

# Applications versions

AppArmor policy is not tied to an application version, however changes to an application or its libraries may necessitate changes to the apparmor policy.

package with application
system package separate from application
profile repository


[policy feature abi](AppArmorpolicyfeaturesabi)
