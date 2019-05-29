Related Documents
=================

-   [AppArmor Technical documentation](Documentation#In_Depth)
-   [Format of the DFA/HFA](TechnicalDoc_HFA_Layout)
-   [PolicyDB the encoding of policy rules into the HFA](TechnicalDoc_PolicyDB)
-   [Encoding permissions](TechnicalDoc_HFA_permissions)
-   [Putting it altogether relationship of Policy and DFA/HFA](TechnicalDoc_Policy_Layout)
-   [Steps in generating the DFA/HFA](TechnicalDoc_HFA)

A Brief explanation of eHFAs in AppArmor
========================================

AppArmor uses an extened Hybrid Finite Automata
(eHFA) as its matching engine. An HFA is merely a [finite
automata](http://en.wikipedia.org/wiki/Nondeterministic_finite-state_machine%7Cnondeterministic)
(NFA) that has been specially
constructed so that it is made up of [finite
automatas](http://en.wikipedia.org/wiki/Deterministic_finite-state_machine%7Cdeterministic)
(dfas) that are joined by special transitions or border states. In
its simpliest form an HFA can be reduced to a single DFA. The
“extended” prefix indicates that the HFA has been extended beyond
traditional nfas/dfas with additional functionality (currently limited
to dynamic runtime variable matching).

AppArmor 2.1 was the first iteration of apparmor to use a dfa to
perform its pattern matching as they provide a fast matching that is
linear to the input stream. It has since been extended to be an eHFA.

AppArmor is capable of generating dfa graphs that can help understand
what is happening in the apparmor\_parser back end.

<image:example-dfa.png>

-   circles represent a state
-   { } within a circle are the set of expression tree nodes that
    make the state
-   ( ) within a circle are optional
    and indicate an accepting state. The number is the permission set for the state
-   characters along a given edge indicate valid transitions. If
    there is input that doesn't match a valid transition then the dfa
    transitions to a null non-accepting state that can not be exited
    (ie it will consume all input).

Issues with AppArmor's DFA
==========================

The dfa that apparmor uses, is a simple direct representation with
the most basic of table compression schemes. It also does not allow
for in line variable matching or other advances that are desirable.

Specific issues with the current implementation

-   it generates large tables that need to be loaded into kernel consuming memory that could be used else where
-   it takes a relatively long time to compute
    -   the generation of the dfa without the factoring steps can be impractical even on modern hardware
    -   the dfa is subject to exponential state explosion.
    -   equivalence classes (if used) are applied post generation
-   the table compression algorithm is a simple and straight forward
    with O(n^2) running time (slow). The compression achieved is not
    perfect but is fairly good given the limits of the compression
    format.
-   the table compression format is simple and direct and does not
    leverage any state or transition redundancies, resulting in poor
    compression compared to what could be achieved
-   The implementation needs to be cleaned up

Overall the dfa engine needs to be updated and replaced with a better
atomata representation that keeps many of the dfas properties while
being flexible, faster to compute (control state explosion), and
leverages a better compression format. Better alternative exist so
it is only a matter of examining the alternatives and implementing.

Rule to DFA conversion
======================

AppArmor file rules are compiled into a dfa that is loaded into the
kernel to do path matching. To do this AppArmor transforms the rules
in the profiles, going through several steps to create the final
dfa. The pipeline is as follows

      profile rules
          |
          v
      sort profile rules
          |
          v
      merge duplicate rules
          |
          v
      convert rules to expression tree
          |
          v
      expression tree simplification
          |
          v
      dfa creation
          |
          v
      dfa minimization
          |
          v
      dfa unreachable state removal
          |
          v
      creation of equivalence
          |
          v
      diff encode
          |
          v
      transition table compression

To illustrate the steps of the conversion pipeline the following simple profile is used through out the following discussion

```
 /usr/bin/example {
   /etc/passwd r,
   /home/*/** rl,
   /home/*/bin/ ix,
   /home/likewise/*/*/** rwl,
   /{usr,}/bin/** px,
   /etc/passwd r,   # duplicate
   /home/*/** w,    # duplicate

 }
```

rule sorting
------------

The first step is sorting path based rules this allows for simple
merging can take place, and also aids in regular expression factoring
as it put similar expressions close to each other in the tree.

```
 /usr/bin/example {
   /etc/passwd r,
   /etc/passwd r,   # duplicate
   /home/*/** rl,
   /home/*/** w,    # duplicate
   /home/*/bin/ ix,
   /home/likewise/*/*/** rwl,
   /{usr,}/bin/** px,
 }
```

merge duplicates rules
----------------------

Rules with the exact same path description are merged, merging
permission as well. There are exceptions in rule merging x
permission and rules that have a pair, like link rules may actually
be separated. This step reduces the number of rules going through pcre
conversion and also the amount of rules that expression simplifications
needs to consider. The permissions merging done in this step results
in smaller expression trees (tree simplification can not do permission
merging as it does evaluate at the rule level), and hence smaller
dfas being constructed.

```
 /usr/bin/example {
   /etc/passwd r,
   /home/*/** rwl,
   /home/*/bin/ ix,
   /home/likewise/*/*/** rwl,
   /{usr,}/bin/** px,
 }
```

rule to expression tree conversion
----------------------------------

AppArmor globbing rules are converted into a single pcre expression
tree, that represents the rules for entire profile. This is done
by first converting an individual rule into and expression tree and
then using an alternation to merge it into the main expression tree
(this is equivalent to combining two rules with an alternation).

Early versions of AppArmor this step is split into two parts, a text
conversion to pcre syntax, and then a pcre parse tree is generated
from that; e.g.:

> `rule1 r, rule2 w,` can be combined as `(rule1 r|rule2 w)`

The handling of rule permissions in tree conversion ???

Note: each parse tree includes a fake node containing the permission
of the rule.

This step can be examined in AppArmor 2.4 and later by doing

```
 apparmor_parser -QT -D expr-tree <profile>
```

E.G., for a single rule from a profile do:

```
  echo "profile example { /home/*/** rl,} " | ./apparmor_parser -QT -D expr-tree

  DFA: Expression Tree
  (/home/[^\0000/]([^\0000/])*/[^\0000/]([^\0000])*(((<4>|<16>)|<65536>)|<262144>)|/home/[^\0000/]([^\0000/])*/[^\0000/]([^\0000])*\0000/[^/](.)*((<16>|<32>)|<262144>))
```

E.G.; for the example profile above:

```
  ./apparmor_parser -QT -D expr-tree usr.bin.example

  DFA: Expression Tree
 ((((((/etc/passwd(<4>|<65536>)|/home/[^\0000/]([^\0000/])*/[^\0000/]([^\0000])*(((((((<2>|<4>)|<8>)|<16>)|<32768>)|<65536>)|<131072>)|<262144>))|/home/[^\0000/]([^\0000/])*/[^\0000/]([^\0000])*\0000/[^/](.)*((<16>|<32>)|<262144>))|/home/[^\0000/]([^\0000/])*/bin/(((<513>|<64>)|<8404992>)|<1048576>))|/home/likewise/[^\0000/]([^\0000/])*/[^\0000/]([^\0000/])*/[^\0000/]([^\0000])*(((((((<2>|<4>)|<8>)|<16>)|<32768>)|<65536>)|<131072>)|<262144>))|/home/likewise/[^\0000/]([^\0000/])*/[^\0000/]([^\0000/])*/[^\0000/]([^\0000])*\0000/[^/](.)*((<16>|<32>)|<262144>))|/(usr|[])/bin/[^\0000/]([^\0000])*(<2305>|<37765120>))
```

### understanding the tree output

The tree is output as as a regular expression where ( ) - groups and expression precedence
| - alternation that separates two alternatives eg. A|B
\* - means repeat the previous expression 0 or more times
+ - means repeat the previous expression 1 or more times
. - match any character
\[\] - character class which can contain multiple alternative characters
\[^\] - an inverted character class
\\xxxx - a non-printable character

expression tree simplification (AppArmor 2.3 and later)
-------------------------------------------------------

This stage does expression tree factoring which can remove common
subexpressions that cancel out. To do this the tree is first normalized
into a left or right normal form, and then a factoring pass is made
finding common nodes. The normalization is then flipped and the
process is repeated as necessary until the tree stabilizes and no
more simplification is possible.

> For example, `(ab|ac)` would be factored into `a(b|c)`

This step was introduced because the dfa conversion step can cause
exponential state explosion. The factoring is biased towards minimizing
the cases that will cause state explosion (right normalization
first). This stage is critical for large or complicated policies as
it can make them possible to compute in reasonable amounts of time
and memory.

### Normalization

Normalization rewrites the expression tree so that alternations and
concatenations always have the same consistent form, which simplifies
the the Factoring stage.

left normalization rules (right normalization is opposite)
```math
(\epsilon | a) \to a | \epsilon
```
```math
(a | b) | c  \to  a | (b | c)
```
```math
[b] | a \to a | [b]
```

```math
 \epsilon a \to a \epsilon
```
```math
 (ab)c \to a(bc)
```

**TODO**: add actual tree diagrams

### Factoring Patterns

```math
a\epsilon \to a
```
```math
(a | a) \to a
```
```math
a | (a | b) \to (a | b)
```
```math
a | (ab) \to a (\epsilon | b) \to a (b | \epsilon)
```
```math
(ab) | (ac) \to a(b|c)
```

To see the tree after the apparmor\_parser has done simplification do

```
 ./apparmor\_parser -D expr-simple -QT <profile_name>

 DFA: Simplified Expression Tree

 /((usr|\[\])/bin/\[^\\0000/\](\[^\\0000\])\*(<2305>|<37765120>)|(etc/passwd(<4>|<65536>)|home(/\[^\\0000/\](\[^\\0000/\])\*/bin/(<513>|(<8404992>|(<1048576>|<64>)))|(/likewise/\[^\\0000/\](\[^\\0000/\])\*|\[\])/\[^\\0000/\](\[^\\0000/\])\*/\[^\\0000/\](\[^\\0000\])\*(<4>|(<16>|(<32768>|(<65536>|(<131072>|(<262144>|(<2>|(<8>|\\0000/\[^/\](.)\*(<16>|(<32>|<262144>)))))))))))))
```

dfa creation
------------

AppArmor does direct single regex to dfa creation
as described in [Compilers - Principles, Techniques, and
Tools](http://www.amazon.co.uk/Compilers-Principles-Techniques-Alfred-Aho/dp/0201100886)
(aka the Dragon Book). This combines the traditional two step
conversion of regex to NFA and then subset construction, and should
general be faster and consume less memory.

At this stage of the dfa lots of information is available to do
analysis on.

To see the dfa statistics and compare how previous optimization have do:

##### tree stats without simplification

```
 apparmor_parser -O no-minimize -O no-remove-unreachable -O no-expr-simplify -D dfa-stats -QT example.txt
 Created dfa: states 58    matching 69 nonmatching 56
```

##### tree with simplifications applied

```
 apparmor_parser -O no-minimize -O no-remove-unreachable -D dfa-stats -QT example.txt
 Created dfa: states 54    matching 67 nonmatching 52
```

The important output from dumping dfa-stats is the number of states
created. The matching entry indicates the number of expression node
comparisons that were folded into existing states while nomatching
is the number of expression nodes that resulted in the creation of
new states.

At this stage you can also obtain a visual graph of the generated
dfa. This is done by dumping the dfa in graphviz format and using
graphviz to generate a diagram. Warning it is easy to create graphs
that are to big to be viewed.

To generate a graph do

```
 apparmor_parser -O no-minimize -O no-remove-unreachable -D dfa-graph -QT <profile_name> 2>profile.gv
 dot -Tpng -o profile.png profile.gv
```

| no tree simplification                                      | tree simplification                                  |
|-------------------------------------------------------------|------------------------------------------------------|
| [400px](image:example-dfa-no-simplification.png) | [400px](image:example-dfa-simplified.png) |

dfa minimization (AppArmor 2.5 and later)
-----------------------------------------

AppArmor uses Hopcroft's dfa minimization algorithm to reduce the
number of states in the dfa to the smallest dfa possible. This step
was added to help reduce kernel memory consumption by the dfa and
compilation times.

**TODO**: insert dfa state example pic

### hashing partition setup

Traditionally minimization starts with 2 partitions (1 accepting,
1 non-accepting) and the partitions are iteratively split until all
states in a partition are indistinguishable. A single representative
state is then chosen from each partition, removing the other states
and a minimized dfa is created. Instead of splitting the dfa states
into 2 partitions, and repartitioning, a linear pass is made through
the states setting up an initial set of partitions using hashing to
separate states that can never be equivalent.

#### transition based hashing

By default transition based hashing is used.

The hashing algorithm takes advantage of the fact that states are
stored in a compressed form with a default transition and then a set
of N non-default transitions.

The rules for the hash are

-   states can not be used as part of the hash as different states may be reduced to the same Partition. Only transitions and known unique partitions can be considered
-   states will only be equivalent if they have the same set of non-default transitions.
    -   number of none default transitions are the same
    -   the set of characters used for non-default transitions are the same

#### permission based hashing

The permissions of the accept state can be used as part of the initial
partition setup (it is defaulted on in AppArmor 2.4 - 2.6). This can
lead to even more initial partitions (speeding up minimization) but it
can result in a non-minimum dfa as some states that would have been
merged end up in separate partitions, which will increase the final
size and can slow down total creation time as state minimization has
more states to compress.

### partition merging

The current algorithm for state comparison within a partition is not
the most efficient form, but since the partition setup does a very
good job, it is adequate for the number of states currently present
in partitions.

dfa unreachable state removal (AppArmor 2.5 and later)
------------------------------------------------------

The creation and or minimization of a dfa can result in unreachable
states. This phase walks the dfa from the start state finding all
reachable states and deletes any that can not be reached.

**TODO**: insert dfa state example pic

Default state differential encoding (compression)
-------------------------------------------------

To reduce the number of transitions a state has to encode states can be
made to encode their transitions as a differential to a “default”
state. If a transition is not represented in the current state
the default state is entered and the process is repeated until the
transition is found or the state is not marked to be encoded relative
to it default.

This type of state encoding can be quite effective in reducing the
number of states but it can increase both encoding and matching time
(as multiple states must be traversed for a single match). However
if the states to encode against are chosen carefully, then both the
encoding time and matching time can be bounded, and differential
encoding can even result in a faster HFA as it can reduce the memory
bandwidth required by the HFA.

The requirements AppArmor uses to choose the states to encode
against are:

-   The state must have been previously matched while walking the dfa (it will be hot in the cache then)
-   or the state must be at the same level in a DAG decomposition of the dfa, sharing a common ancestor (more on this below)

The first requirement was primarily for performance concerns but
in practice works out well for compression too, as states that are
close to each other often have similar transitions. The second allows
expanding the reach of the compression to a few more likely options
while keeping a potentially common hot path, and without breaking
other properties of only referencing previously matched states.

In practice requirement 1 can not be met as each match string takes
a different path through the dfa. It can however be approximated
by converting the dfa into a directed acyclic graph (DAG) with the
start states as the root. The DAG provides a good approximation for
requirement 1 and at the same time limits how many states have to
be considered for compression (only backwards in the DAG). It also
provides guarentees on how many states will be walked at run time
(at most 2n).

Converting the HFA into DAG for compression does have a limitation
in that it removes many of a states immediate neighbours from
consideration. In a DAG a states neighbours can be broken into
five classes, immediate predecessor, predecessor on another branch,
sibling on another branch, immediate successor, successor on another
branch. The immediate predecessor and immediate successor cases
are covered by the predecessor differential compression scheme
described above (successor as the current state is the successor
state predecessor, and thus will be considered when the successor
is differentially encoded). However the successor and predecessor on
another branch and sibling cases are not covered, and they maybe the
more optimal path for encoding, and may be the hot path the match
came through.

To help account for this AppArmor also compares to the immediate
successors of the state being consider if there are transitions between
the states. Sibling states are also considered if there are transitions
between the state and the sibling is differentially encoded against a
predecessor (not another sibling), or not differentially encoded. This
broadens the set of states considered but limits it to states that
were potentially matched against and thus in the cache. It also has
the property of looking backwards in the DAG thus keeping the maximum
number of states that are required to be transitioned to in a match
to a linear constant. If only branch predecessor where used then
the limit could be kept at 2n but because immediate siblings can be
used iff they transition to a predecessor the limit is bounded to a
slightly higher value of 5/2n.

When considering which state to differentially encode against AppArmor
computes a weighted value and chooses the best one. The value is
computed as follows.

-   For each defined transition in the state
    -   +0 to candidate state weight

        if the transition is undefined in the candidate state (the
        transition must be represented in the current state)

    -   +1 to candidate state weight

        if the candidate state has the same transition to the same
        state (the transition can be eliminated from the current state)

    -   -1 to candidate state weight

	if the transition is defined in the candidate state and it is
	not the same transition (current state must add a transition
	entry to override candidate transition)

-   For each undefined transition in the state

    -   +0 to candidate state weight

        if the transition is undefined in the candidate state

    -   -1 to candidate state weight

        if the transition is defined in candidate state (current
        state must add an entry to override the candidate transition)

The current state will be differentially encoded against the candidate
state with the largest weight \> 0. If there is no weighting \>
0 then no differential encoding for the state will be done as their
is no benefit to doing so.

**Note**: differential encoding can in one special case reduce a state to 0
stored transitions. This can happen when two states have the exact same
transitions but belong in different partitions when minimized. This
would happen for example when one state was an accept state and the
other a none accepting state. Other wise if states have the same
transitions they are redundant and removed during state minimization.

creation of equivalence classes
-------------------------------

AppArmor can use (they are optional) equivalence classes to further
compress a states transitions. AppArmor uses a single HFA table to
encode equivalence classes at the byte level (ie 256 entries). The
equivalence class can be used in two ways to improve compression.

-   map multiple characters to a single transition
-   remap characters to reduce the distance between characters allowing the better comb compression

The mapping of multiple characters to a single character transition
is the traditional use of equivalence classes. Each equivalence class
in the HFA consists of the set of characters that have the exact
same transitions for every state in the HFA. That is 'a' and 'A'
are in the same equivalence class if and only if for every state that
'a' has a defined transition, 'A' also has a defined transition and
'a' and 'A' transition to the same state. This allows 'a' and 'A'
to represented by a single transition, and for an equivalence class
table to be used to remap 'a' and 'A' to the equivalence class.

Even if there are no equivalence classes to be found within the HFA the
equivalence class table can be used to remap characters, so that they
can be better compressed by the following comb compression step. In
this case the distance between common transitions is reduced (ideally
to zero). For example assume that 'a' and 'A' appear as transition
with in states frequently but are not equivalence classes, and that
no characters between 'a' and 'A' are used in transitions. When the
transitions for 'a' and 'A' are encoded in the transition table there
is a gap between the two transitions as the transition table is index
by character. Comb compression is a technique to try and fill the gap
between those characters so that the transition table is fully packed,
but this is a hard problem (np complete) and so comb compression often
does a less than optimal job. The equivalence class can be used to map
these two characters to be adjacent so that there is no gap to fill in
the transition table. This can result in higher and faster compression
as the comb compressor then has less combinations to consider.

Implementation wise the equivalence class is a 256 byte table that
is index by the input character to find the equivalence class that
is then used as the index for the state transition. The equivalence
class transformation is one way as multiple characters can map
to a single equivalence class (which is just a character). While
equivalence classes require and extra lookup they can actually increase
performances as they reduce the memory bandwidth required to traverse
the HFA and the equivalence class table is small and cache hot.

The equivalence class computation is performed after state differential
transition encoding as that reduces the number of transitions that
must be considered. This in turn can improve the chance of finding
equivalence classes, and even if it doesn't can result character
transitions being mapped tighter together for comb compression.

dfa table compression
---------------------

Currently there are is a single compression scheme with some
options that can affect compression times, match times and size. The
compression format is based on the split tables and comb (interleaved
column) compression from the Dragon book. The current compression
schemes are designed to be used at dfa runtime such that the the data
is kept in an immediately accessible form using naturally accessible
word entries with no bit compression. A consequence is that the
tables could be further compressed using a gzip style compression
for on disk storage.

The simplest form of the compression is straight comb compression
with non-differential entries. Other compression variants modify the
base compression by making entries differential to other states as
discussed above.

### Compression Efficiency

The base compression format of split tables + comb compression
works well as long as the average number of transitions per state
is low. The use of differential encoding does not change the base
compression format but serves to further reduce the average number
of transitions that need to represented.

The effectiveness is determined by comparing the compressed format to
an uncompressed dfa representation which is a table indexing the rows
by state, the columns by input character, and the cells containing
the next state and accept information. Ignoring any extended HFA
information, this would be
```math
 size = (sizeof(next state)) * (# of input characters) * (# of states) + sizeof(accept index) * (# of states)
      = (2) * 256 * s + 2 * x
      = 512s + 2s
      = 514s
```

Therefore AppArmor's uncompressed encoding (2 bytes for next state,
2 bytes for accept index) takes up 514bytes of memory for each
state. The compressed table size is represented by:
```math
 size = ((sizeof(base index) + sizeof(default) + sizeof(accept index)) + ((sizeof(next) + sizeof(check)) * (Average transitions per state) * (Packing factor))) * (# of states)
      = ((4 + 2 + 2) + ((2 + 2) * Ave * Pf)) * s
      = 8s + 4(Ave * Pf)s
```

The packing factor (Pf) expresses the efficiency of the packing done
by comb compression. A perfect packing of 1000 transitions would be
a table with 1000 entries. When the packing isn't perfect the comb
compression leaves unused entries (gaps) in the table so a Pf of
1.1x for a 1000 transitions would result in a table with a size of
1100 entries.

The theoretical maximum compression of the split table + comb
compression scheme is 64.25x (514s/8s) and only occurs when there
is only a default transition for every state in the DFA. Since this
makes the average number of transition per state 0, the second term
of the table size equation can be eliminated.

In practice, 64.25x compression is not achievable but good
compression is still achieved. For a given profile, the average
number of transitions per state ranges from 3 up to approximately 16
(see apparmor\_parser -D stats) and the packing factor ranging from
1.1 to 1.4.

Compression efficiency for different transition averages and packing factors:

| Ave | 1.0x  | 1.1x  | 1.2x  | 1.3x  | 1.4x  | 1.5x  |
|-----|-------|-------|-------|-------|-------|-------|
| 1   | 42.83 | 41.45 | 40.16 | 38.94 | 37.79 | 36.71 |
| 2   | 32.13 | 30.60 | 29.20 | 27.93 | 26.77 | 25.70 |
| 3   | 25.70 | 24.25 | 22.95 | 21.78 | 20.73 | 19.77 |
| 4   | 21.42 | 20.08 | 18.90 | 17.85 | 16.91 | 16.06 |
| 6   | 16.06 | 14.94 | 13.97 | 13.11 | 12.36 | 11.68 |
| 8   | 12.85 | 11.90 | 11.08 | 10.36 | 9.73  | 9.18  |
| 12  | 9.18  | 8.45  | 7.84  | 7.30  | 6.84  | 6.43  |
| 16  | 7.14  | 6.56  | 6.06  | 5.64  | 5.27  | 4.94  |
| 32  | 3.78  | 3.45  | 3.18  | 2.95  | 2.75  | 2.57  |
| 64  | 1.95  | 1.77  | 1.63  | 1.51  | 1.40  | 1.31  |
| 128 | 0.99  | 0.90  | 0.83  | 0.76  | 0.71  | 0.66  |
| 240 | 0.53  | 0.48  | 0.44  | 0.41  | 0.38  | 0.35  |
| 250 | 0.51  | 0.46  | 0.43  | 0.39  | 0.37  | 0.34  |
| 256 | 0.50  | 0.45  | 0.42  | 0.38  | 0.36  | 0.33  |

When differential state compression is used, the average number of
transitions stays closer to the low end (2 and 3) and the packing
factor approaches 1. This is because as the number of transitions
approach the upper and lower bounds (1 and 256), the vector are denser
and tend to pack better.

### Effect various options have on the size of the dfa

add no tree-expr, no min

old style accept

An example of the effectiveness of the compression scheme is the
evince profile, which compiles into 3 profiles with DFAs.

| Profile 1 | options                 | DFA \# of states | no compression | 2 table (no comb) | 2 table + comb          | 2 table + comb + differential |     |
|-----------|-------------------------|------------------|----------------|-------------------|-------------------------|-------------------------------|-----|
| 1         | no minimize, no sharing | 11977            | 5.87 MB        |                   | 563KB ave=7.54, Pf=1.33 |                               |     |
| 2         | no minimize, no sharing | 8172             | 4.01 MB        |                   | 306KB ave=6.22, Pf=1.22 |                               |     |
| 3         | no minimize, no sharing | 5954             | 2.92 MB        |                   | 230KB ave=6.40, Pf=1.23 |                               |     |
| 1         | minimize, no sharing    | 8852             | 4.34 MB        |                   | 425KB ave=7.74, Pf=1.33 |                               |     |
| 2         | minimize, no sharing    | 6016             | 2.99 MB        |                   | 237KB ave=6.61, Pf=1.22 |                               |     |
| 3         | minimize, no sharing    | 4086             | 2.95 MB        |                   | 168KB ave=6.93, Pf=1.23 |                               |     |
| 1,2,3     | minimize + sharing      | ???              |                |                   | Ave=, Pf=               |                               |     |

### The Basic format

The HFA is split into multiple tables, for a basic DFA (subset of
the EHFA) there are 5 table types used, default, base, next, check,
and accept. The default, base, and accept tables store the basic
information about a state and are indexed by the state number. While
the next and check tables store state transitions are index by the
base value (obtained from base table) + input character.

The accept table provides the information to determine the permission
associated with that state, whether directly stored in the table or
an index into another structure.

The default table stores the default transition that should be taken
for the state, if the check entry does not match.

The base table stores the start position value into the next check
tables for the state.

The next check, tables store the non-default transitions for the
dfa. Each state begins in the table with a location determined by its
base value and then it is index from that position by the character
(byte) being checked. The next table stores the state to transition
to for the character being checked, and the check table stores whether
the entry being checked belongs to the current state.

Each state with in the table is treated as a sparse matrix column
only storing non default entries. The states are then interleaved so
that one states entries are mapped into the holes of another state.

Comb compression affects the base value which determines the position
of the states transitions in the next, check tables. State differential
encoding affects the default state and number of transition that need
to be stored for the state.

performance optimization, grouping tables together so they are the
same cache line.

### Basic default entry compression

| State     | \\000 | \\001 | \\002 | ..  | \\254 | \\255 |
|-----------|-------|-------|-------|-----|-------|-------|
| 0 (dead)  | 0     | 0     | 0     | ..  | 0     | 0     |
| 1 (start) |       |       |       | ..  |       |       |
| 2         |       |       |       | ..  |       |       |
| . . . |   |
| 53        |       |       |       | ..  |       |       |
| 54        |       |       |       | ..  |       |       |

| State     | Accept | Default | Base |
|-----------|--------|---------|------|
| 0 (dead)  | 0      | 0       | 0    |
| 1 (start) |        |         |      |
| 2         |        |         |      |
| 3         |        |         |      |
| 4         |        |         |      |

| Base | Next | Check |
|------|------|-------|
| 0    |      |       |
| 1    |      |       |
| 2    |      |       |
| 3    |      |       |
| 4    |      |       |

### Recursive default entry compression

Computing State Permissions from Rules
======================================

accept nodes
------------

- unique per permission

deny
----

x permission
------------

- intersection test - dominance
