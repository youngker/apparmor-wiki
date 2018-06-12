**TODO**

Trusted helpers are services that have elevated permission in some
domain, that AppArmor trusts to help enforce certain policy.

# Example Trusted helpers

* External Mediation
  -   dbus
  -   [xapparmor xace plug-in](AppArmorXace)
  -   gsettings daemon
  -   databases
  -   ...
* Launchers/Domain Transitions
  -   aa-exec
* Containers
  -   snappyd
  -   libvirt
  -   lxd
* Privilege Separation
  -   media hub
  -   file dialogs
  -   contact management

