= Warning WIP =

= TOC =
- [policy management](Apparmorpolicymanagement)
- [policy caching](Apparmorpolicycache)
- [binary policy](Apparmorbinarypolicy)
- [early policy](Apparmorearlypolicy)
- [policy in the initrd/initramfs](Apparmorinitrd)

= Introduction =

initrd - allow setting profiles on init system and early processes


alternative early policy system
- less rigorous but often sufficient
- easier to update policy
- early processes start unconfined
- replace early default profile with new profile shared by all tasks in the default profile
- requires init system to load policy early, should be before compilation and text policy are available

default profile

first load replace default profile, with new profile, all tasks confined by default confined by the new profile


