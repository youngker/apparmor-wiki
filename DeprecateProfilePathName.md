This document is a WIP

# Introduction

AppArmor 3.0 deprecates the use of pathnames as the profiles name. Eg.

```
/usr/bin/firefox {
   ...
}
```

The use of a simple profile name and a separate pathname based exec attachment should be used instead. Eg.

```
profile firefox /usr/bin/firefox {
  ...
}
```

This does require the use of the ```profile``` keyword. The path name based attachment (/usr/bin/firefox) is optional .

# Why were pathname based profile names deprecated?

There are several reasons that these types of profile names were deprecated.

Could contain character that some tools could not properly handle or would result in names being encoded by the audit subsystem

Long

Not cross distro compatible

Profile rules that reference profile names
- Encoded regexes
- Regexs in the rules
- Each rule referencing the profile name would need to be updated when the profile name was updated to different location
