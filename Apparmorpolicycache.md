# Warning this is a WIP

# TOC
- [policy management](Apparmorpolicymanagement)
- [policy caching](Apparmorpolicycache)
- [binary policy](Apparmorbinarypolicy)

# AppArmor 2.x Binary policy cache

AppArmor compiles text policy into a binary binary policy that is then loaded into the kernel for enforcement. Before AppArmor 2.13 apparmor used a binary cache to store compiled policy so that the text policy would not have to be recompiled on every boot. However the cache had several limitations and issues. The cache was replaced by a [binary policy](AppArmorbinarypolicy) scheme in apparmor 2.13.

The policy cache was defined by the apparmor_parsers ```--cache-loc``` flag which defaulted to ```/etc/apparmor.d/cache/``` if no value was supplied. The cache consisted of a directory of files, one for each text policy file, and ```.features``` file that stored the [feature abi](Apparmorfeatureabi) the cache was compiled with.

![image](/uploads/1e5a8be4dfebcf7e9566316c9d8651da/image.png)


names same as text policy
time stamp and features check

no way to define multiple policy locations
managed in initscripts (ubuntu), parser.conf (upstream)




file/time stamps

policy cache

default location

setting its location

writing/updating the cache

kernel reboot limitations



