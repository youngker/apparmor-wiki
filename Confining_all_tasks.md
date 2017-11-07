This is a guide on how to use AppArmor to confine all tasks on
the system.

-   need to load policy early (initrd)
-   precompile policy and rebuild so that initrd can just use a loader instead of including the compiler
-   may need to use change\_profile to put init task into a profile

Reasons to do this total lock down Interest profile abstraction
development

Output goes to dmesg, then logs, then apparmor daemon for complain
messages if it is up and running

policy techniques - [Policy creation guide](CreatingPolicy) link back
to policy authoring techniques

can't just set up profiles base on application name globbing domain
hierarchy (like tomoyo) domain types (specific transitions between
types)
