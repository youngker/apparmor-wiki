# TOC
- [policy management](Apparmorpolicymanagement)
- [policy caching](Apparmorpolicycache)
- [binary policy](Apparmorbinarypolicy)



[Policy management in AppArmor 2.x](Apparmorpolicymanagement2x)

# Init systems

## Policy Location

The location of policy can be set by dropping a policy config file in ```$(config_location)/policy_conf.d/```. Where $(config_location) is either the default location ```/etc/apparmor/``` or set as a build option ???.

## Multiple policy locations

Policy can exist in multiple locations, for each location (even if the policy is not managed by AppArmor) a config file should be dropped in ```$(config_location)/policy_conf.d/```. This enables the AppArmor init system and tools find and function with the different policy locations. Even for policy not managed by AppArmor a policy config file should be added so the AppArmor tools can know not to touch the alternately managed policy.

## Policy config file
- policy location
- cache location
- namespace
- managed
- when should it be written
- watched for auto update
- additional parser flags


# Policy namespaces

Mixed policy

setting up a policy location

# Responding to policy changes automatically

aa-policy daemon

# Recommended Policy Locations

- Binary policy cache (locally updatable)
  /var/cache/apparmor/

- Immutable binary policy

- AppArmor 2.x default locations
    /etc/apparmor.d/
    /etc/apparmor.d/cache.d/

# User defined policy

- Requires
-- AppArmor 3.x
-- kernel with [scope & view support](AppArmorpolicyscope)
- Recommended
-- pam_apparmor2???

User defined policy requires that an apparmor namespace be setup for the user by the system. This can be done through the pam_apparmor plug-in. In addition the pam_apparmor plug-in can load the initial user defined policy if it is stored in the configured location.

- text policy: .apparmor.d/
- binary policy cache: ?????

In addition to using pam_apparmor to setup a users policy namespace. The user can configure the aa-policy daemon to run at session login. The daemon can be used to watch the user's policy for changes and automatically reload policy.

# Application defined policy

application can choose where to store policy and where to store its binary policy
