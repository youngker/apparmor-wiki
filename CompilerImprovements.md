This pages tracks improvements made to the AppArmor compiler
performance over each release.

Areas for improvement
=====================

1.  lexer - Front end duplicate include elimination
    -   per profile cache which files have already been included so they don't need to be processed multiple times

2.  Include parse caching
    -   cache include files post parsing so that they don't need to be re-parsed

3.  Rule duplicate removal (only done on files)
    -   almost unneeded if fix for 1 is done
    -   requires: to full replace (so code can be dropped) need 1 and 4

4.  aare -&gt; pcre convertion
    -   add native parsing of aare to the background eliminating this step

5.  early tree node merging
    -   as part of tree creation instead of blindly creating new nodes, merge into existing tree as parsing
    -   requires: 3

6.  tree simplification
    -   store alt nodes is sorted vector
    -   store cat nodes in vector
    -   do tree factoring on vectors, does not require tree rewrites or left/right manipulations
    -   merge char nodes into charset nodes
    -   replace char nodes with charset node to remove duplicate code

7. dfa creation - currently creates sets and removes duplicates which means a lot of creation and freeing
    -   push anode and nnode split into expr tree for nullable, firstpos, lastpos and follow calculations
        -   removes need to split sets, reducing allocation of set just to split it apart and lookup its constituent anode and nnode sets
    -   push anode and nnode caching into expr tree so it can be used by nullable, firstpos, lastpos
    -   add nnode vec merging, and move nnode to nodevec all the way through
        -   eliminates need to create nnode set to be immediately converted to nnode vec
    -   early accept node merging
        -   Instead of creating accept node sets do accept node merging and use single accept node to represent merged state
    -   nodevec merging cache
        -   merge operation cache provides duplicate elimination so that many node sets don't need to be recreated and freed after being lookuped at the cache level
    -   Eliminate nnode cache?
        -   its possible that between the nodevec merge cache and final combined nnode, anode cache the dedicated nnode cache is no longer benifical

8.  comb compression
    -   change to sliding window algorithm, to reduce set of slot comparisons done
    -   Note: diff compression also reduces the number of slot comparisons being done by reducing the number of transition

9.  Fix diff compression
    -   Current diff compression has a bug that needs to be found and fixed

10.  file cache
    -   cache files that have already been read so they don't need to be reread
    -   requires multiple profiles be processed by one compiler call to be effective (especially if duplicate include elimination is in effect)

11.  parallel compile
    -   compile individual profiles in parallel - separating the compile at the profile level should be fairly easy
        -   requires removal of global state
    -   compile profile components in parallel (see partial compiles)
        -   once partial compiles are in place it should be easy to compile these components separately
    -   pipeline profile construction.
        -   It is questionable whether a fine grained threading is worth while as the locking overhead is probably rather high but the different stages of the dfa constructions could be pipelined.
        -   tree optimization -&gt; DFA construction -&gt; minimization -&gt; diff-encode -&gt; comb compression
        -   since each stage is separate but dependent, separate threads could work on a given thread

12.  partial compiles - shared/precompiled
    -   share and potentially cache partial compile components
    -   includes are a natural boundary to precompile
    -   Requires dfa set operations

Improvements per Release
========================

-   2.1 DFA introduced
-   2.3 tree factoring
-   2.4 minimization
-   2.5
-   2.6 group info on state, singleton epsnode, remove ref count,
-   2.7 split dfa code - no major improvements
-   2.8 diff encoding
-   2.9 remove node hashing, fixing full minimization
-   2.10 early accept node grouping, and removal of accept alt node trees

Performance Tests
=================

small | Release | Time | Peak Mem | Nodes | States | Transitions 2.1 2.3 2.4 2.5 2.6 2.7 2.8

medium | Release | Time | Peak Mem | Nodes | States | Transitions 2.1 2.3 2.4 2.5 2.6 2.7 2.8

large | Release | Time | Peak Mem | Nodes | States | Transitions 2.1 2.3 2.4 2.5 2.6 2.7 2.8

huge | Release | Time | Peak Mem | Nodes | States | Transitions 2.1 2.3 2.4 2.5 2.6 2.7 2.8
