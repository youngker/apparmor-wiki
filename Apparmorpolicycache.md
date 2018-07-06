TOC
- [policy caching](Apparmorpolicycache)
- [binary policy](Apparmorbinarypolicy)

# AppArmor 2.x Binary policy cache

AppArmor compiles text policy into a binary binary policy that is then loaded into the kernel for enforcement. Before AppArmor 2.13 apparmor used a binary cache to store compiled policy so that the text policy would not have to be recompiled on every boot. However the cache had several limitations and issues.

single location
default to
managed in initscripts (ubuntu), parser.conf (upstream)


![image](/uploads/1e5a8be4dfebcf7e9566316c9d8651da/image.png)


policy cache

default location

setting its location

writing/updating the cache

kernel reboot limitations

AppArmor 2.x (except 2.13)

replaced by [binary policy](AppArmorbinarypolicy)

