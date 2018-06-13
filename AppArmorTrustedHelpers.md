**This is a wip**

# Introduction
Trusted helpers are userspace services that have elevated permission in some
domain, that AppArmor trusts to help enforce certain policy. Trusted helpers may themselves be untrusted and confined in other areas.

There is no one way to implement a trusted helper, they may be tightly integrated with rules integrated into policy and leveraging the apparmor query infrastructure. To a loosely coupled service that shares no information with AppArmor and only queries some AppArmor properties when doing its enforcement.

# Basics of a Trusted Helper

##Factors to consider

### Split or Consolidated policy
text policy in one place makes it easier to understand policy
- splitting text policy may make it easier to develop tool (no integration with apparmor parser needed)
- make it easier to write policy for admin of a traditional tool eg. iptables (note iptable rules can't take advantage of full apparmor mediation)

binary policy in one place makes check/point restore operation consistent
integrating into policydb allows for apparmor to handle dynamic policy computations


Version of AppArmor to support

Whether to extend profile language or do custom policy
- keeping policy together 

# Policy Backend

## Mediation Class
AppArmor uses a reserved number, the mediation class, for each type of mediation. The mediation class is used when encoding policy and making policy queries, and is needed for registering extensions. If policy extensions are going to be integrated into AppArmor they should reserve a class number.

???? link to class numbers

## PolicyDB

The [policydb](Technicaldoc_policydb) is the main ???.


Recommended to at least provide minimal policydb entry so that
aa_class_supported() can be used to determine if a mediation class is supported

???? link to minimal policydb and how to edit

## Private Trusted Helper Policy Blob

A trusted helper can decide that it does not want to use the AppArmor policydb/dfa, but still have some integration with AppArmor.

Trusted Helpers can use the profile "data" store to embed helper specific data into profiles. The data store provides a name value data store where the kernel does not care about the format of the data placed in the value field.

Accessing the data field
aa_query_label_data -

Direct access from file????



#Dealing with Dynamic Policy

Trusted helpers need to deal with dynamic policy

Handling policy external to apparmor means having to handle dynamic policy computations

## Dynamic Includes
dynamic includes can be used to extend a profile without having to replace an entire profile.
They allow for a Trusted helper a controlled way to provide partial policy for a profile without needing the authority to replace/update a profile.

## Stacking

Used by Launchers and containers [stacking](Apparmorstacking)

## Delegation

[delegation](Apparmordelegation)

### Object (FD) Delegation

### Rule Delegation


# Accessing policy

Query interface
- caching

Using libapparmor on locally saved policy

Pulling policy from the kernel for independent policy



# Responding to policy changes

policy notifications

# Namespace scopes and views
can be used to control access

# Example Trusted helpers

* External Mediation
  -   [dbus](AppArmorDBus)
  -   [xapparmor xace plug-in](AppArmorXace)
  -   [gsettings daemon](AppArmorGSettings)
  -   databases
  -   ...
* Launchers/Domain Transitions/Containers/Sandboxing
  -   aa-exec
  -   aa-sandbox
  -   [systemd](Apparmorinsystemd)
  -   snappyd
  -   libvirt
  -   lxd
* Privilege Separation
  -   media hub
  -   file dialogs
  -   contact management

