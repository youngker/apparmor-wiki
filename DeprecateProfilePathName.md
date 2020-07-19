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

- Path based profile names can be long, especially if they used alternations or stacking was in use. This made them less than ideal for introspection tools (top, ps, ...) that could cut them off. It also could make applications fail if their buffers were too small.

- The use of regular expressions in path based profile names forced escaping of characters to distinguish them from regular expression characters. This made referencing profiles by their names cumbersome and again could cause problems with introspection tools. Switching to a simple profile name and attachment removes this problem by moving the regex into the attachment which is not used by system introspection tools.

- The use of path based profile names is not cross distro compatible, as applications may be stored in different locations on different distros. Updating a profile to a different location for a distro also required updating all profile's and rules that referenced the profile name, which could mean updating dozens of locations in policy. With splitting the location and attachment this means rules referencing the profile do not have to be updated, making porting policy between distros much easier.

- Simple profile names are easier for users to read, compare, and write.
