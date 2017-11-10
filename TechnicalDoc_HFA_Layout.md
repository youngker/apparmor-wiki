The AppArmor compressed DFA/HFA format is based off of the flex
DFA table format. The base format has remand the same since its
introduction but it has been tweaked and extend.

Related Documents
=================

-   [AppArmor Technical documentation](Documentation#In_Depth)
-   [Format of the DFA/HFA](TechnicalDoc_HFA_Layout)
-   [PolicyDB the encoding of policy rules into the HFA](TechnicalDoc_PolicyDB)
-   [Encoding permissions](TechnicalDoc_HFA_permissions)
-   [Putting it altogether relationship of Policy and DFA/HFA](TechnicalDoc_Policy_Layout)
-   [Steps in generating the DFA/HFA](TechnicalDoc_HFA)

Base Layout DFA
===============

The base layout consists of a header (at the start of the stream)
containing DFA generic information and then a set of tables, each
with their own headers with data following immediately after.

![](Base-dfa.png "Base-dfa.png")

The base dfa layout is common to all versions of AppArmor, it has
five tables: default, base, next, check, accept. The default, base,
and accept tables have the same length and are indexed by the state
number. The check, and accept tables are the same size and are index
by a states base value + input character.  The default table stores
the default transition for a given state, it is used when, when the
check value at base + input does not equal the current state. The
base value is the start position in the next/check tables, for the
states transitions. The next table provides the next state to go to
for the base + input if the corresponding check entry matches the
current state.

As shown above in states 2 transitions, they can be interleaved
with over states transitions. This comb compression is done to save
space, instead of requiring 256 transition entries for a given state
only the transitions not covered by the default transition need to
stored in the next check table. It is possible that some entries of
the next/check tables may be empty but comb compression will usually
fill the majority of entries.

The above diagram can not show the differential state compression,
because default state transition are used to represent both
differential state compression and simple default state transitions,
the only difference being a flag that is stored in the high bit of
the base table.

The accept table while being part of the DFA is not used by the DFA,
its values are defined externally to the DFA.

DFA16
-----

In the DFA16 format, the default, next and check tables are 16 bits
wide allowing for at most 65,536 states. It is used when possible to
reduce the amount of memory needed by a DFA at run time. The base
table is 32 bits wide with the upper byte reserved for flags. The
accept table size is defined by the application (see [Encoding
permissions](TechnicalDoc_HFA_permissions) for apparmor's use of it).

DFA32
-----

In the DFA32 format, the default, next, and check tables are 32
bits wide allowing for the DFA to be much larger at the cost of a
larger memory foot print. The base table remains 32 bits wide but a
new table is added to store that states flags, thus giving it a full
32bit index. The accept table size remains defined by the application
just like in the DFA16 format.

AppArmor 2.1
============

AppArmor 2.3 - 2.7
==================

A second accept table was added

AppArmor 3.0
============

eHFA/eDFA
=========

flags
-----

At this time the upper 8 bits are reserved for flags. Would like to
only use 4 or less for flags, and use the rest to extend the range

There are several flags used to track which states have special
conditions.

-   differential state compression
-   variable conditional accept
-   back ref - can this be tied to variable conds
-   counting constraing - can this be tied to variable conds
-   border state - could just do side lookup?
-   forward lookahead, and negative forward lookahead for match at this state? Or should this be encoded in a perm
    -   need to know what state(s) perms to hit on the look ahead so probably in perms

2-5 bits would like to get 28 bits for base value

eDFA - variable extension
-------------------------

In addition to the tables and transitions outlined for the base dfa the
variable extension add several new tables and transition relationships.

![](Edfa.png "Edfa.png")

The variable eDFA extends the base DFA with six new tables: caccept,
trans, inst, tcond, tbase, and cond. The caccept, and trans tables are
the same size and maybe less than or equal to the size of the accept,
default and base tables. The inst table maybe less than or equal to
the size of next check and the tcond, tbase and cond tables size is
independent of all other tables.

The cond table stores, logical variable combinations, like A&B,
A&B&C, etc.

The trans table stores an index into the tcond and tbase tables that
are used to find the transitions for a particular variable combination
that is possible for the state. The tcond table is an index into
cond for a particular variable condition, and tbase references into
next/check for the actual transition.

The inst table stores an index into the cond table for the set of
variables that a transition into next will instantiate.

If the state is flagged as conditionally accepting the accept table
entry takes on a new meaning, it becomes an index into the acond,
and aaccept tables. These provide the accept value that matches the
current variable match conditions.

The tcond/tbase and acond/accept tables, are stored in an ordered
set of perms that belong to the referencing state, that is the state
can have more than one entry. ?? does the state have another table
to list how many entries, or is a null entry used. Another table for
size is smaller. So add an extra table?

Note that states that have conditional accept, or conditional
transitions are sorted to be in the lower DFA state entries.

eDFA - back reference extension
-------------------------------

Back references are handled as an extension of the variables extension
with additional with additional tables that are used to set up variable
values, so that they can be matched against.

eDFA - counting constraints
---------------------------

Counting constraints while similar to the variable extension are
separate and see their own set of tables added (that are similar to
the variable tables) to set up the counts, and handle conditional
matching based off of the counts.

HFA - NFA border states
-----------------------

The addition of border states to the DFA changes it into an HFA by
adding introducing an NFA state. A border state marks the transition
from one sub DFA to a set of sub DFAs. It does this by storing a set
of transitions, one for each sub DFA it transitions too, which can
then be match by breadth or depth first match just has an NFA would do.
