Encoding Mount options
======================

Mount options encompass two distinct sets of data, the mount flags,
and the fs options that are encoded in different ways.

Separating mount flags from fs data options
-------------------------------------------

Mount options are are parsed and any that match the a table of
predefined mount flags are removed to be encoded as mount flags,
the rest are treated as fs mount options.

Encoding of Mount flags
=======================

The flags of mount rules use a special encoding to allow for a trianary
match (present, not present, don't care) against a bit set.

The mnt flags set follows the kernel mount flag set, that is a 32
bit mask in the same numerical order as stored in the Linux kernel.

```c
 #define MS_RDONLY     (1 << 0)
 #define MS_RW         (0 << 0)
 #define MS_NOSUID     (1 << 1)
 #define MS_SUID       (0 << 1)
 #define MS_NODEV      (1 << 2)
 #define MS_DEV        (0 << 2)
 #define MS_NOEXEC     (1 << 3)
 #define MS_EXEC       (0 << 3)
 #define MS_SYNC       (1 << 4)
 #define MS_ASYNC      (0 << 4)
 #define MS_REMOUNT        (1 << 5)
 #define MS_MAND       (1 << 6)
 #define MS_NOMAND     (0 << 6)
 #define MS_DIRSYNC        (1 << 7)
 #define MS_NODIRSYNC      (0 << 7)
 #define MS_NOATIME        (1 << 10)
 #define MS_ATIME      (0 << 10)
 #define MS_NODIRATIME     (1 << 11)
 #define MS_DIRATIME       (0 << 11)
 #define MS_BIND       (1 << 12)
 #define MS_MOVE       (1 << 13)
 #define MS_REC        (1 << 14)
 #define MS_VERBOSE        (1 << 15)
 #define MS_SILENT     (1 << 15)
 #define MS_LOAD       (0 << 15)
 #define MS_ACL        (1 << 16)
 #define MS_NOACL      (0 << 16)
 #define MS_UNBINDABLE     (1 << 17)
 #define MS_PRIVATE        (1 << 18)
 #define MS_SLAVE      (1 << 19)
 #define MS_SHARED     (1 << 20)
 #define MS_RELATIME       (1 << 21)
 #define MS_NORELATIME     (0 << 21)
 #define MS_IVERSION       (1 << 23)
 #define MS_NOIVERSION     (0 << 23)
 #define MS_STRICTATIME    (1 << 24)
 #define MS_NOUSER     (1 << 31)
 #define MS_USER       (0 << 31)
```

Notice that some flags have a bit shift value while other flags
are defined as 0. The flags defined as 0 have no bit set, as these
flags they are represent by the absence of the flag, that is they
are represent by a 0 value in the noted bit position; e.g. MS\_RW is
true when MS\_RDONLY is not set.

Each flags is encoded as a single byte value equal to its bit shift +
1. So MS\_RDONLY is encoded as 0 + 1 = 1, and MS\_RW is not directly
encoded. With each bit that is encoded being written in bit order.

For example, the flags set (MS\_RDONLY | MS\_NODEV | MS\_NOACL | MS\_NOUSER) is encoded as the ordered match:

```
 (MS_RDONLY shift + 1)(MS_NODEV shift + 1)(MS_NOUSER shift + 1)
```

*Note*: MS\_NOACL is not encoded because it is represented by the
MS\_ACL bit not being set

A "don't care" flag is encoded as an alternation of the flag shift +
1, and the empty set (MS\_RDONLY shift + 1|).

For example, using the same flags from above as don't care values
(MS\_RDONLY | MS\_NODEV | MS\_NOACL | MS\_NOUSER) would be encoded as

```
 (MS_RDONLY shift + 1|)(MS_NODEV shift + 1|)(MS_ACL shift + 1|)(MS_NOUSER shift + 1|)
```

*Note*: MS\_NOACL is now represented by (MS\_ACL shift + 1|), this
is because declaring a flag as a don't care value means it can take
on either value.

To match against a mount rule's flag set. The requested flag set
is search in bit order and match attempt are made against the set
bits. That is for a bit set (MS\_RDONLY|MS\_NODEV) only the character
(MS\_RDONLY shift + 1) and (MS\_NODEV shift + 1) are matched against
in that order.

E.G. the dfa state matching for (MS\_RDONLY|MS\_NODEV) is

```c
    next_state = dfa_match(state, MS_RD_ONLY shift + 1)
    next_state = dfa_match(next_state, MS_NODEV + 1)
```

Why this works
--------------

A mount rule using = for its options (mount option=( )) encodes an
exact pattern of bits. Flags that are not specified are taken to be
their 0 value, and flags that have a 0 value are ignored. The string
of set flags makes a unique string much like the character string '1 3
7'. To match this string only those bits can be set in the flags mask,
an unset bit does not lead to progression through the match string,
a flag that is set that is not in the string results in match failure.

E.G. (MS\_RDONLY shift | MS\_NODEV shift | MS\_ATIME | MS\_ACL)
is encoded as the string '1 3 16', notice MS\_ATIME is dropped.

-   Trying to match (MS\_NOSUID) against this results in the string
    of '2', which clearly does not match (it fails in the first
    character), and can not progress, as 1 will never be matched as
    the string is always generated in numerically increasing order.

-   Trying to match (MS\_RDONLY shift | MS\_NODEV shift ) against
    it result in as string of '1 3' which will match the first 2
    characters but can not progress to the last character.

-   Trying to match (MS\_RDONLY shift | MS\_NODEV shift | MS\_ACL |
    MS\_NOUSER) results in a string of '1 3 16 32' which matches for
    the requisite 3 characters, but the MS_NOUSER bit forces the match
    to move out of an accepting state and fail. The accept permission
    is encoded only in the on the states that exactly match and moving
    out of them results in no permission.`

The DFA can efficiently encode multiple rules information, allowing
for a single pass match against all rules.

Encoding of flags when using **options in (set)**
-------------------------------------------------

When options are specified using the **in** keyword. The flags are
treated as don't care values because the **in** option defines a subset
of values that can be specified. Any flag that is not specified by
the **in** subset is treated as a 0 value unless there is also an
exact match **options=()** set.

This means that

```
 options in (ro)
```

is equivalent to

```
 options=(ro,rw)
```

and for allow rules is just a convenient short hand for expressing
a set of flags that may or may not be set. Matching of these values
works similar to the matches shown above except the string defines
a pattern match

Example: mount options in (ro,nouser):

-   defines the bitset (MS\_RDONLY | MS\_NOUSER), which makes the
    match string (1|) (32|) which is equivalent to the following set
    of strings

   -   '' - the empty string '1'
   -   '32' - '1 32'

Example: mount options in (ro,acl) options=(nodev,nouser)

-  defines the 2 bitsets (MS\_RDONLY | MS\_ACL) and (MS\_NODEV |
   MS\_NOUSER) which defines the match string (1|) 3 (16|) 32  which
   is equivalent to the following set of strings

   ```
   '3 32'
   '1 3 32'
   '1 3 16 32'
   '3 16 32'
   ```

### Short cut encoding for all flags

If all flags are optional the regular expression `[^\x00]*` is used

Encoding deny flag rules
------------------------

Because of the way flags are matched the strings used to match deny
rules need to be encoded differently in the don't care cases. The
exact match cases (options=()) follows the same encoding as the allow
rules. The reason two different string encodings are used is that
exact match cases defines exactly 1 string that can be matched but,
the don't care cases define a set of strings, and the semantics of
the set is inverted by the deny.

For example, deny options in (ro, acl), does NOT mean
```
   deny '' - emtpy string
   deny ro,
   deny acl,
   deny ro acl,
```
But rather means deny any match containing ro or acl, that is
```
   deny ro,
   deny ro nosuid,
   deny ro nodev,
   deny ro nosuid nodev,
   deny ro ...
```

Where deny options=(ro) means deny the case where ro is the only flag set.

To represent the don't care flags deny rule we use an alternation
with a pattern matching all flags

Example: options in (ro, acl), is represented as:
```
   deny ([^\x00]*1[^\x00]|[^\x00]*16[^\x00])
```

This allows the rule to intersect all mount rules that could contain
ro or acl flags.

### combining don't care and exact match

When options and exact match string are combined the alternation gets modified

????

### Short cut encoding for denying all flags

If a deny rule encodes both the set and unset forms of a flag
(eg. options=(rw,ro) or options in (ro)) this results in all flags
being denied because a positive flags match is not possible because
during a match there are only 2 possible values for each flag and
both values are denied.

This is match in this case is encoded with the regular expression
`[^\x00]*`

Encoding of FS specific options
===============================
