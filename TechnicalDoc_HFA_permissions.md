Related Documents
=================

-   [AppArmor Technical documentation](Documentation#In_Depth)
-   [Format of the DFA/HFA](TechnicalDoc_HFA_Layout)
-   [PolicyDB the encoding of policy rules into the HFA](TechnicalDoc_PolicyDB)
-   [Encoding permissions](TechnicalDoc_HFA_permissions)
-   [Putting it altogether relationship of Policy and DFA/HFA](TechnicalDoc_Policy_Layout)
-   [Steps in generating the DFA/HFA](TechnicalDoc_HFA)

Overview
========

The encoding of permissions in the DFA/HFA has evolved over time.

AppArmor 2.1 AppArmor 2.3 - 2.7

AppArmor 3.0

Reasons for the major permission rework
---------------------------------------

There are four basic reasons a permission layout was changed after
AppArmor 2.7

##### Efficiency

The layout of AppArmor 2.1 - 2.7 permissions was inefficient wasting
space, even for the limited set of permissions covered by AppArmor
2.7. Even though the full set of bits per entry where in use, if a
state shared the same permission with another state the information
was duplicated, 64bits per state. With a DFA/HFA with a size of 10,000
states typically having between 20 and 30 unique accept permissions,
the result was less than efficient, and would be even less efficient
if the accept information was widened.

```
 2.7 accept table size = 8*(states)
 3.0 accept table size = 2*(states) + (# unique perms) * (size of expanded permission block)
```

So assuming the accept block size was not expanded and encoded the same
64-bit permissions, and there where 30 unique permissions to encode.

```
 2.7 format = 8*10,000 = 80,000 bytes
 3.0 format = 2*10,000 + 30*8 = 20,240 bytes
```

And as the accept block is expanded the saving increase.

##### Future expansion

As documented in the AppArmor 2.7 layout all 64bits of the permission
structure where in use, an expansion was needed to be able to add new
abilities. The expansion could have just expanded the width of the
accept table, but that would not have been as efficient or flexible.

##### Flexibility

The older accept format was to ridged to be able to efficiently
represent, all the new abilities that where desired.

##### Speed

Reworking the layout provided for better cache efficiency, and no need
to unpack and transform the entries to determine the final permissions.

The permission rework also affected the the internals of the DFA/HFA
compilation engine. - rework to track information

- lost at tree to dfa state

- provide for full minimization - provide for per rule negation - provide for conditional tracking

AppArmor 2.1
============

AppArmor 2.1 used a single 32-bit accept table in DFA to store
permission.

Layout
------

Compiler internals
------------------

AppArmor 2.3 - 2.7
==================

File Permission Layout
----------------------

AppArmor 2.3 introduced a second accept 32 bit accept table to the
DFA making each accept state a total of 64 bits wide, and reworked
the bit layout, breaking compatibility with AppArmor 2.1.

The permissions where packed into the accept states as follows

| Bit                  | 31     | 30    | 29    | 28  | 27  | 26  | 25  | 24  | 23  | 22  | 21  | 20  | 19   | 18  | 17  | 16  | 15  | 14  | 13  | 12  | 11  | 10  | 9   | 8   | 7   | 6   | 5    | 4   | 3   | 2   | 1   | 0   |
|----------------------|--------|-------|-------|-----|-----|-----|-----|-----|-----|-----|-----|-----|------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|------|-----|-----|-----|-----|-----|
|                      | Global | Other | Owner |
| Table 1 - perms mask | cp     | onx   |       |     | x3  | x2  | x1  | x0  | i   | u   | pux | m   | k/ls | l   | a   | r   | w   | x   | x3  | x2  | x1  | x0  | i   | u   | pux | m   | k/ls | l   | a   | r   | w   | x   |
| Table 2 - audit mask | cp     | onx   |       |     | qm  | qk  | ql  | qa  | qr  | qw  | qx  | m   | k/ls | l   | a   | r   | w   | x   | qm  | qk  | ql  | qa  | qr  | qw  | qx  | m   | k/ls | l   | a   | r   | w   | x   |

The permissions stored in the **owner** mask are used if the objects
uid == to the tasks fsuid, other wise the other mask is used.

Execute permissions is determined by x, with the transition type determined by a combination of pux, u, i, and x0-x3.

-   *i* - fall back to inherit current profile
-   *u* - unsafe exec, have glibc scrub environment variables
-   *pux* - unconfined flag used in combination with x0-x3 to create
    pux. Was added after original layout to extend pux support
    explaining duplication of unconfined flag
-   *x0-x3* - index into transition table.
    -   0 - not defined (use i, u to determine transition)
    -   1 - unconfined (precludes px)
    -   2 - use executable name
    -   3 - use executable name, lookup child profile
    -   4-15 - index into named transition table, to find transition
        name. Note: named transition to children profiles where stored
        using compound name (ie parent//child), so they could share
        in a single global table entry.

cp - is the change\_profile permission
onx - change\_profile on exec permission

the link pair permission overlaid on the k permission on the second
pair.

ls - link subset permission, overlaps k permission, only happens in
second match of chain (see link below) where k is not possible.

The audit and quiet table (Table2). The permissions stored in table
2 are used to determine if a permission is forced audited, or if for
denied requests where the auditing is quieted. The upper 7 bits of
the Owner and Other sets store the quiet masks, which get shifted by
7 bits when applied to mask the x through m permissions.

Link rules are encoded requiring sequential matching and permissions in
two permission tables. First the link name is matched, and a permission
lookup for the l permission bit is done against the matched state. A
transition on the 0 byte from the current state is done and then the
target name is matched, and a second permission look up for the l
permission bit is done against the new match state. The link\_subset
permission (overlaps k in permission table) can also be looked up
against the second match.

change profile rules use cp and onx permissions and are overlayed
over top of file rule dfa entries. They are encoded in a multiphase
lookup, with the change\_profile permission bit being used to
determine permission. First a check is done against the profile
name for permission, if that doesn't succeed then a second lookup
is done against the namespace name followed by a colon followed by
a profile name.

Compiler internals
------------------

-   AppArmor 2.6 saw flattening of permissions at state creation time
    as part of internal optimizations to speed up the compiler.

AppArmor 3.0
============

Permission Layout
-----------------

AppArmor 3.0 extends and reworks the dfa and permissions structures
used in the AppArmor 2.x series. Unlike the AppArmor 2.x series a
single dfa can be shared by more than one profile, with each profile
having its own accept permissions for a give state. To accommodate
this the permission structure is split off from the dfa. The dfa
accept table now contains a single 16 or 32 bit index value (size
dependent on dfa16, or dfa32 format).

If the accept index value has its high bit clear (15 or 31) then it is
an index into a profiles permission table. If the accept index value
has its high bit set then it is an index into a profiles conditional
table. Each profile that references the dfa defines its own conditional
and permission tables allow each profile to have a different set of
permissions for any given state.

This split of the dfa and permissions was done to allow for dfa sharing
and to provide greater performance as when a dfa is shared multiple
profile matches can performed at once. It also allowed reducing the
amount of memory dedicated to storing permission data, by allowing
for duplicate elimination of permissions.

The split of the conditional information from the permissions structure
was done to allow for a much broader range of conditions that could be
easily extended in the future, and for better sharing of the extended
permission structure.

The permission structure for AppArmor 3.0 is extended to support a
much broader set of permissions, and information. Firstly as mention
with the dfa accept table value, it has been split into a permission
table and a conditional table. The conditional table contains entries
that encode conditional expressions to obtain a permission (eg. the
owner flag), the permission table contains the actual permissions
that the profile will grant.

The permission table has been expanded to support a broader set of
permissions, including file, network, ipc.

### File Permission Entry

### Network Permission Entry

### Capability Permission Entry

### ????

Compiler internals
------------------

-   rework of permission handling to find unique permissions during
    DFA/HFA creation and to carry full permission information through
    out the DFA/HFA creation pipe line. Carry the full information
    through the entire pipeline reduced performance slightly but was
    offset by improvements brought by reducing comparison time by
    separating out the accept states into unique sets.

The AppArmor DFA/HFA uses multiple unique accept states to encode
permissions, and related control information for any given match.

Why multiple distinct accept states
===================================

could encode in stream but

-   have to make multiple matches one for each permission
-   still have to store extra information for control that happens for the matched state (xtrans ..)

Encoding Conditional permissions
================================

AppArmor encodes conditional information both in the DFA/HFA and in
a separate conditional structure. The split is determined by how the
permission request is done. Permission queries done with information
external to the object (file path vs. inode) are done within the dfa
while, permissions conditional on object properties are done in the
conditional table.

Encoding within the DFA/HFA (parameter)
---------------------------------------

To encode conditionals within the DFA alternations and ordering
are used.

Encoding in the (object)
------------------------

When conditionals are encoded in stream
---------------------------------------

When conditionals are encoded out of band
-----------------------------------------
