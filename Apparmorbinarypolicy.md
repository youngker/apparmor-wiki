TOC
- [policy caching](Apparmorpolicycache)
- [binary policy](Apparmorbinarypolicy)

# Introduction

In AppArmor 2.13, AppArmor moved from a [policy caching](Apparmorpolicycache) scheme to a binary policy scheme. The binary policy scheme is still a form of caching of text policy but the layout and semantics have changed allowing for improvements that where not possible under the old caching scheme.


2.13 & AppArmor 3.x

binary policy

# Layout of binary policy

Binary policy is laid out as a forest of directories under a primary directory tree at ```$(location)```. 

![binarypolicy](/uploads/983cea25b0ebd22dc2eed9523096dbf4/binarypolicy.png)

binary policy layout

dealing with collisions

# Overlaying of binary policy

![cache-overlay](/uploads/03d11d02d539af5135084d9f1aafc5b9/cache-overlay.png)


