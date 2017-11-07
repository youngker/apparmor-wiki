Introduction
============

Can have more than one version of a profile.

The profile variant may have different permissions/rules.

Allows customizing profiles for different situations but still have
a single name.

Multiple variants can be loaded and applied at the same time.

Variants are based on the attachment conditional

```
 profile firefox attach=/usr/bin/firefox {

 }

 profile firefox attach=/usr/lib/firefox {

 }
```

If they have they same rules could do with a single attachment

```
 profile firefox attach=/usr/{bin/firefox,lib/firefox} {

 }
```

Variants do not change the label for the purposes of label
introspection or communication.

However they do change the type that needs to be used for caching

but sometimes installs at different locations need different
permissions.

variants can also be used to make choices based off of the user being attached to.

```
 profile firefox attach=(file=/usr/bin/firefox user=alice) {
 }

 profile firefox attach=(file/usr/bin/firefox user=bob) {
 }
```
