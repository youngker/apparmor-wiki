PolicyDB encoding policy into an HFA.

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

The PolicyDB is the state machine that matches various permission
requests to the permission permissions granted by a profile.

AppArmor 3.0 introduced the PolicyDB, which is extends the use of
the HFA beyond file rules into other mediation types. The PolicyDB
allows for generic queries to be made against AppArmor policy using
just the HFA. For backwards compatibility reasons masks and some
other structures are retained and used but all information is also
recorded in the PolicyDB.

The layout of the PolicyDB can be thought of as a tree, that begins
with the HFA start state. From here a single byte transition based
on the type of permission request, finds the rules governing that
type. Further transitions within a type lead to more specific sub-types
and eventually a match that can be used to determine permissions.

![](Policydb.png "Policydb.png")

Example: doing

```C
 file_rules_start_state = next_state(PolicyDB, start_state, AA_FILE_TYPE)
```

will find the file rules within the PolicyDB. Note that file rules
are stored in a backwards compatible manner so that, direct access
is possible by specifying an alternate start state for file rules.

Each kind of permission request has a defined types, with unknown
types reserved for future expansion. The Layout and ordering of
matching within a given type, is tailored to the input of the
types permission request, so each type has its own layout and high
level match routine.

The currently define classes for the policydb are

-   AA\_CLASS\_CAP
-   AA\_CLASS\_FILE
-   AA\_CLASS\_ATTACH
-   AA\_CLASS\_ENV
-   AA\_CLASS\_ARGV
-   AA\_CLASS\_MOUNT
-   AA\_CLASS\_NET
-   AA\_CLASS\_PIPE
-   AA\_CLASS\_SYSV\_MSGQ
-   AA\_CLASS\_SYSV\_SEM
-   AA\_CLASS\_SYSV\_SHMEM
-   AA\_CLASS\_SIGNAL
-   AA\_RLIMIT\_CLASS
-   AA\_MEMORY\_CLASS
-   AA\_CPU\_CLASS
-   AA\_CLASS\_AUDIT
-   AA\_CLASS\_KEY
-   AA\_CLASS\_DBUS
-   AA\_CLASS\_X

File type
=========

Link and snapshot rules
-----------------------

change\_profile
---------------

exec rules and environment filtering
------------------------------------

Capability type
===============

Mount type
==========

Network type
============
