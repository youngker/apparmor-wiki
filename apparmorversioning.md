# Versioning in AppArmor Table of Contents
- [AppArmor Versioning](Apparmorversioning)
- [Policy Feature ABI](AppArmorpolicyfeaturesabi)
- [Policy Versioning](AppArmorpolicyversioning)

# Introduction

When working with AppArmor their are many independent or semi-independent components whos version numbers will not match. Generally version numbering is used to provide a coarse indication of a components progression but does not usually provide an indication of compatibility.

The main components when dealing with apparmor are the kernel module, user space tools (parser, libapparmor, apparmor utils), policy (abstractions, and applications), and applications (both the application and its dependent libraries).

# AppArmor kernel module

The AppArmor kernel module does not have its own version information. It is tied to kernel releases and instead exports a [feature abi](AppArmorFeatureABI), and [api to userspace](AppArmorinterfaces).

This largely decouples the kernel version from userspace. A specific kernel version may be patch to include additional features (eg. Ubuntu kernels carried additional patches for several releases), and userspace can take advantage of those features if it knows how to deal with it.

# AppArmor userspace

Gerneally the AppArmor userspace will be built and packaged as a whole under a single version number. This is however not required and at times the apparmor_parser or tools maybe built and deployed on an older version of the libraries. Generally it is not recommended that distro do this as the tools can become out of sync, where the apparmor_parser may support features that the profile development tools don't, etc.

However when distros have long term support cycles new kernels and Policy may require updating the apparmor_parser, and the distro maybe reluctant to upgrade the other components. In this case it is best for the distro to decide what is best for their users.

# Applications versions

AppArmor policy is not tied to an application version, however changes to an application or its libraries may necessitate changes to the apparmor policy.


# Packaging


# Policy

The discussion of policy versioning has been left to last as it depends on all of the above factors.

Use of packaging Versioning

Dealing with the [feature abi in policy](AppArmorpolicyfeaturesabi)

[Versioning of Policy](AppArmorpolicyversioning)

Not all policy shipped in packaging

dependencies
- requires
- suggests
- breaks


profiles can be split into 2 main sets. Base application permissions, configuration permissions, dynamic runtime permission (changes based on inputs).


package with application
system package separate from application
profile repository



