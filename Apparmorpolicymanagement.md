# Init systems

# Multiple policy locations

# Policy namespaces

# Responding to policy changes automatically

aa-policy daemon

# User defined policy

User defined policy requires that an apparmor namespace be setup for the user by the system. This can be done through the pam_apparmor plug-in. In addition the pam_apparmor plug-in can load the initial user defined policy if it is stored in the configured location.

- text policy: .apparmor.d/
- binary policy cache: ?????

In addition to using pam_apparmor to setup a users policy namespace. The user can configure the aa-policy daemon to run at session login. The daemon can be used to watch the user's policy for changes and automatically reload policy.

# Application defined policy

application can choose where to store policy and where to store its binary policy
