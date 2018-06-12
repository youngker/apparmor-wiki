**TODO**

# Introduction
Trusted helpers are services that have elevated permission in some
domain, that AppArmor trusts to help enforce certain policy.


# Basics of a Trusted Helper

Factors to consider

Version of AppArmor to support

Whether to extend profile language or do custom policy
- keeping policy together 

PolicyDB

External Policy Blob

#Dealing with Dynamic Policy

Trusted helpers need to deal with dynamic policy

## Dynamic includes

## Stacking

## Delegation

### Object (FD) Delegation

### Rule Delegation


# Accessing policy

Query interface

Using libapparmor on locally saved policy

Pulling policy from the kernel for independent policy



# Responding to policy changes

policy notifications


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

