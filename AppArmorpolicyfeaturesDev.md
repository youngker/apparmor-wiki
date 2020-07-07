# AppArmor Feature ABI Table of Contents
 - [Policy Feature ABI](AppArmorpolicyfeaturesabi)
 - [Developing Profiles with the features ABI](AppArmorpolicyfeaturesDev) (This Document)
 - [Feature ABI in AppArmor 2.x](AppArmor2FeatureABI)
 - [Policy Feature ABI Interactions in Depth](AppArmorFeatureABIinteractions)
 - [Policy Feature ABI low level examination](AppArmorFeatureABI)
 - [AppArmor Versioning](Apparmorversioning)
 - [Policy Versioning](AppArmorpolicyversioning)

# Introduction

# Developing Policy

Feature ABI rules make policy development harder in that new kernel features are not used right away. Instead to use new features the abi rule must be updated.

AppArmor allows for a special abi rule for policy developers that allows for the old AppArmor 2.x development behavior.

```
   abi <kernel>,
```

Once the policy has been updated to the developer ABI rule, policy development can proceed as normal under AppArmor 2.x. The policy should be reloaded and the kernel will log violations for newly supported features. Once the policy has been updated with an new rules that are necessary. The abi rule must be updated to point to the new abi that is in use.

```WARNING: The AppArmor project will not accept profiles or policy patches that set the abi rule to <kernel>.``` This is to ensure policy works as expected for users and to ensure that the AppArmor project will be able to continue to land new feature support in the upstream kernel.

## How to find the Kernel feature ABI that is in use

???

### Extracting the new abi to an abi file

???

