Chronology of important events affecting AppArmor development:

-   AppArmor's predecessor SubDomain began life as a Grad project
-   1998/1999? WireX founded by Crispin Cowan and ???? to commercialize [SubDomain](https://www.usenix.org/legacy/publications/library/proceedings/lisa2000/full_papers/cowan/cowan_html/index.html), and develop other security extensions to Linux.
    -   WireX produced Immunix a secure Linux distribution based on Red Hat Linux using StackGuard, FormatGuard, and SubDomain
    -   WireX involved in creation of LSM
-   In early 2004 WireX rebranded to Immunix and then dropped its Linux distribution, refocusing instead on providing SubDomain for SuSE linux.
-   SubDomain rewritten to use LSM, despite WireX involvement in creating the LSM the porting of AppArmor to the LSM was done late after the LSM was created for political reasons. This meant that LSM was less than optimal for AppArmor and the feedback that could have improved the LSM during its development never occured.
-   A YaST GUI implemented for SubDomain on SuSE linux
-   In May 2005 Novell acquired Immunix and rebranded SubDomain as AppArmor and began cleaningup/rewritting the code for upstream linux kernel inclusion
-   AppArmor 2.0.1 released - based on SubDomain 2.0 code with AppArmor rebranding
-   AppArmor 2.1 released. Large portions of the kernel module where rewritten to use a new custom dfa based matching engine (dropping pcre for licencing reasons) and with name lookup being based on a large patch to vfs and LSM which passed the vfsmnt through into all of the relevant LSM hooks).
-   In October 2007 Novell layed-off most of the development team working on AppArmor, and re-assigned the remaining developers putting AppArmor in development mode.
-   June 2008 Novell releases AppArmor 2.3 released using another iteration of the vfs patches, and extension to the matching engine
-   In May 2009 Canonical Inc. picks up AppArmor maintenance and development.
-   Fall 2009 AppArmor 2.4 released. Based on a large update/rewrite to of the AppArmor 2.3 kernel module, it was updated to for creds, and the LSM path\_security hooks.
-   July 2010 AppArmor security module merged into security-next tree. Core functionality accepted into official Linux 2.6.36 kernel. A few compatibility small patches are needed to work with current userspace.

