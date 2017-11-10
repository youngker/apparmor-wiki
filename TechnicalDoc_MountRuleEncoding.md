Encoding of Path information for Mount rules
============================================

Related Documents
=================

-   [AppArmor Technical documentation](Documentation#In_Depth)
-   [Encoding of Rule Paths](TechnicalDoc_RulePathEncoding) - DRAFT
-   [Encoding of File Rule Paths](TechnicalDoc_FileRuleEncoding) - DRAFT
-   [Encoding of Network Rule Paths](TechnicalDoc_NetworkRuleEncoding) - DRAFT
-   [Encoding of DBus Rule Paths](TechnicalDoc_DBusRuleEncoding) - DRAFT
-   [Encoding of X Window Rule Paths](TechnicalDoc_XWindowsRuleEncoding) - DRAFT

Encoding of mount rule paths
============================

AppArmor mount rules are encoded into a set of tuples with mounts
being quad and quin null separated element tuple, unmount being a
single, and pivot\_root having two elements.

Each mount, umount and pivot\_root rule in the policydb begins with
the mount\_class which is defined as 0x07.
```C
 #define AA_CLASS_MOUNT        7
```

Followed immediately (no null separation) by its path. Mount rules,
umount and pivot\_root rules all follow the class with the mntpnt
match. Mount rules then follow that with the device path match,
the type match, and the mount flags match, finishing off the quad
tuple. If a match to the fs specific options is required a fifth
element for the data match portion is append to the quad tuple. While
umount rules hang their accept infomation off of the \<mntpnt\> match,
pivot\_rules follow it with an \<oldroot\> match.

This produces a rule path that looks like

-   mount

    ```
 \x07 <mntpnt> \x00 <device path> \x00 <fstype> \x00 <flags> [ \x00 <fs specific options> ]
                                                        |                      |
                                                     accept                  accept
    ```

-   umount

    ```
 \x07 <mntpnt>
         |
      accept
    ```

-   pivot\_root

    ```
 \x07 <mntpnt> \x00 <oldroot>
                        |
                     accept
    ```

All the individual match elements except \<flags\> are standard pattern
match expressions that are not allowed to match `[^\x00]` so that
a match can never pass the \x00 separator, until the mount matcher
explicitly transitions between the element. The \<flags\> element
has a special mapping that is documented in [Encoding of the mount
flags](TechnicalDoc_Mount_Flags).

accept information
------------------

Mount rules uses 4 different flags, one each for mount, umount,
and pivot\_root, with a special flag defined for data matches
```C
 #define AA_MAY_PIVOTROOT    1
 #define AA_MAY_MOUNT        2
 #define AA_MAY_UMOUNT       4
 #define AA_MATCH_CONT    0x40
```

The accept flag is hung off of the \<flags\> and \<fs specition option\>
matches. The \<fs specific options\> element is always optional and
will only be matched against if the accept flags on the \<flags\>
match contain the AA\_MATCH\_CONT flag.
