Work Items
==========

This is a list of outstanding work items that can be done again the
current code base.

For a list of improvements and extensions to AppArmor see the [development roadmap](DevelopmentRoadmap)

Parser
------

##### General

-   Convert parser to straight C++ compile and then strip
-   Remove pcre
-   Convert parser into front end and library(ies)
    -   Front end, switch and input control
    -   Parsing Layer and semantic check - convert text to parse tree
    -   Mid Layer parse tree manipulation
    -   Back end
        -   rule to compiled struct - DFA/dDFa/XFA
        -   compiled struct matching
        -   compiled struct compression

##### Compatability

-   check for newer kernel version
-   output new split dfa, permission format
-   allow sharing dfa
-   network rules?

##### Caching

-   move cache out of /etc/apparmor.d/ into /lib/apparmor/
-   store off which files are parsed
-   read file list and stat to check for newer files against binary cache timestamp
-   fallback to parsed file stamps to validate cache before doing compute

##### Front end

-   cleanup internal flags and toggles, shouldn't have to special case -S to get output at backend
-   Cleanup iteration of files specified and front driver code
-   permission check update, should be using capable instead of checking for root
-   move profile control symlinks into /etc/apparmor/

##### Parsing

-   make parser reentrant
-   setup multiple entry points to parser, profile, general rule, file rule, network rule, variable, ...
-   Cleanup semantic parsing
-   Cleanup permission parsing of rules

##### Mid

-   convert rules, codomains to C++ classes
-   convert lists, to C++ list iterators
-   convert twalk to C++ container
-   Sort and rule merge cleanup
    -   use iterator sort
    -   cleanup merge comparisons
-   new stage doing front factoring for rules with the same permissions before expr parsing
-   Fix caching loading so its not split across front, and current backend loading of policy

##### EXPR

-   split expr from dfa
-   native expr parsing of AA rules
-   set up shared nodes struct for exprs
-   use single shared eps node, instead of doing allocations?
-   have normalization use single shared eps node so we don't have to do tail, alt direction check on factoring
-   have shared accept states that aren't ref counted but released when common shared struct is released
-   Add more debug to expr tree simplification
    -   number of iterations
    -   number of changes on a given iteration
    -   Find out why expr tree simplfication is taking so long
-   Add short cuts for expr tree simplification, need to reduce recursion and looping
    -   when pulling up alt nodes in normalization - maybe don't need to recurse on each child? Or can recurse less, currently done after each pull up
    -   hashing of node to speedup comparison
    -   character set conversion? Convert nodes into character sets?
    -   Inverted character set conversion? convert inverted sets into standard sets?
-   Fix dump of Nodes into standard C++ redirects
-   Split out as much dfa from expr trees as possible

##### DFA

-   hashing of sets for quicker construction comparison
-   store less dfa info in expr node, store as much intermediate information as possible in the DFA creation routine
-   pull cases into dfa node
-   get rid of dfa level map of state to transition table
-   add routine to clear, expr tree references from dfa, having references back to expr tree is good for debugging but should not be needed in normal use
-   dfa matching routines using computed dfa
-   DFA Set operations
-   Incremental DFA construction, instead of single large regex break
    into multiple, and do minimization on each and then merge. If the
    split is done correctly this should reduce the number of extra
    states created and eliminated

-   Cache precompiled DFA chunks
    -   save off chunk
    -   load chunk
    -   check time stamp
    -   combine chunk into accumulating policy

-   Accept States
    -   Dfa sharing, need to track perms per shared profile/hat and extract perms for each profile/hat after the fact
    -   dfa negative states for permission subtraction
        -   special states for per rule subtraction

-   Minimization
    -   better state comparison
    -   hash partitioning of non-accepting states
    -   For debugging purposes allow starting from 2 basic partitions
    -   For debugging purposes allow minimizing to a single accept state (ie different permissions are not a reason for partitions splitting

-   Compression
    -   Better stats
    -   Reduce overhead of tracking free position list
    -   more options on ordering of rules being inserted
    -   bitmap comparison
    -   hashing schemes to speedup compression?
    -   dfa matching routines using compressed dfa

##### Back End Binary Policy writing

    -   move to C++ stream

Genprof/Logprof
---------------

-   suggest abstractions with variable name matches. Eg, if
    /proc/filesystems is denied, aa-logprof will not suggest
    abstractions/base even though it has @{PROC}/filesystems

-   have aa-logprof only show entries after the 'Last Modified' date
    (which it writes to the profile)

Profile Repository
------------------

-   bleah current implementation is an eye sore

init scripts
------------

pam\_apparmor
-------------

-   update to use change\_profile
-   jdstrand: some form of testing (DONE via QRT)

tomcat\_apparmor
----------------

-   get change\_hat plugin for tomcat working with tomcat 6

mod\_apparmor
-------------

-   ???

kernel
------

-   remove unused dfa code
-   use lsm audit
-   use audit/complain masks instead of a flag
-   hierarchical namespaces
-   path improvements (flags to control)
    -   deleted file
    -   disconnected paths
    -   chroots
-   features interface as sysfs style dir
-   merge common of file\_perm path\_perm
-   multi profile load

Miscellaneous
-------------

-   add aa-reload (or similar) as a wrapper around 'apparmor\_parser -r' with perhaps some added functionality
-   verify fuse mounts with test cases

test suites
-----------

### parser

-   userside matching engine regression. Setup tests of matching against known rules, and strings.
-   userside matching of kernel regression tests (run known request traces against policy used in kernel test except against user side matching).
-   incorporate new 'error' tests from Makefile into testsuite proper
-   check binary output in some way
-   check dump output against what is loaded
-   use more unit testing
-   add capability to run certain tests as root

### kernel

#### regression tests

-   way to mark expected profile on program
-   way to mark supported version on test (error code, pass/fail for each)
-   better/more flexible profile description support (profile generation)
-   better way to share/reuse test for conditionals
-   load/remove/replace tests
-   namespace create/remove tests
-   break tests into different catagories and directories
-   provide easier access to different sets of tests to run subsets
-   procattr read expected value tests
-   supported interface version tests
    -   do basic loads for all supported versions
    -   run tests for supported versions

-   failed name lookup tests (lazy unmounts, deleted files, exec through proc fds, etc)
-   namespace tests
-   named transition tests
-   regex profile name tests
-   change\_profile onexec
-   expand open permissions tests with different flags passed
-   alias tests
-   variable and conditional tests
-   children profiles
-   audit tests
    -   audit rules
    -   quiet
    -   global flags
-   conditional tests
    -   owner
    -   extended owner
    -   group
    -   inode ...
-   expand link tests
-   chroots
    -   flags affecting path resolution
-   mount - extend mount tests, tests for new mount rules
-   bind mounts
-   pivot roots
-   union mounts
-   stacked filesystems
    -   unionfs - tests
    -   aufs
    -   ecryptfs
-   network tests
-   logging tests
-   ipc tests
-   eventually reorganize so this is more like the other suites where the tests are drop in

#### stress tests

-   fix existing broken tests and reenable
-   leak tests
-   add structure to tests

### libapparmor

-   rename so that the test files give an indication of what they do
-   create regression tests for bindings with following priority: first perl, then python, then java and ruby

### mod\_apparmor

-   add upstream tests, but have them disabled by default (since a running apache will be needed)
-   enable the upstream tests in QRT

Documentation
=============

### wiki

-   Release Notes
    -   update and collate into single location

-   overview
    -   walk throughs

-   Technical documentation
    -   10000' view (overview) see above
    -   1000' view course explanation of what happens at walk through level
    -   100' view detail of what is done and why for each part

-   Roadmap

### Man pages

-   \[jdstrand\] write pam\_apparmor
-   write change\_hatv(2)
-   write change\_onexec(2)
-   genprof logprof

### Tech documents

-   update and fold into wiki versions

### Presentations

-   add to wiki

### comparisions

-   comparisons to other security projects
    -   weaknesses and advantages

### Tutorials to write

Once written, these need to be integrated into [Documentation](Documentation)

-   [Extended\_profiling\_example](Extended_profiling_example)
-   [Revising\_and\_fine\_tuning\_policy\_and\_abstractions](Revising_and_fine_tuning_policy_and_abstractions)

### Misc

Once written, these need to be integrated into [Documentation](Documentation)

-   Update [Raise task capabilities](Raising_Task_Capabilities)
-   Write [Confining\_all\_tasks](Confining_all_tasks)
-   Update [AppArmorSandboxing](AppArmorSandboxing) as sandboxing capabilities come online
-   Write [Binary\_Profile\_Language](Binary_Profile_Language)
-   Write [AppArmorLog](AppArmorLog)
-   Write [Controlling\_Profile\_State](Controlling_Profile_State)
-   Write [ExampleProfiles](ExampleProfiles)

