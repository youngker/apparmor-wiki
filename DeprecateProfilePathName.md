This document is a WIP

# Introduction

AppArmor 3.0 deprecates the use of pathnames as the profiles name. Eg.

Ex 1.
```
/usr/bin/firefox {
   ...
}
```

Instead the use of a simple profile name and a separate pathname based exec attachment should be used instead. Eg.

Ex 2.
```
profile firefox /usr/bin/firefox {
  ...
}
```

This does require the use of the ```profile``` keyword. The path name based attachment (/usr/bin/firefox in the above Ex 2) is required to replicate exec attachment of pathname based profiles (Ex 1).

# Why were pathname based profile names deprecated?

There are several reasons that these types of profile names were deprecated.

- Path based profile names could contain character that some tools (audit, top, ps, ..) could not properly handle. At best this would result in names being encoded (eg. by the audit subsystem), but in other cases the name would be cut off, dropped, or even cause the application to crash.

- Path based profile names can be long, especially if they used alternations or stacking was in use. This made them less than ideal for introspection tools (top, ps, ...) that would cut off 

Not cross distro compatible

Profile rules that reference profile names
- Encoded regexes
- Regexs in the rules
- Each rule referencing the profile name would need to be updated when the profile name was updated to different location
