Lots of different versioning to deal with

parser, kernel, policy, library, tools,


profiles can be split into 2 main sets. Base application permissions, configuration permissions, dynamic runtime permission (changes based on inputs).

# Applications versions

AppArmor policy is not tied to an application version, however changes to an application may necessitate changes to the apparmor policy.

- libs may require changes as well

package with application
system package separate from application
profile repository


[policy feature abi](AppArmorpolicyfeaturesabi)
