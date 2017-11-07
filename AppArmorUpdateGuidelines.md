Guidelines for safely updating an AppArmor system

AppArmor is a system of interconnected components that can break in
some situations if changes are not applied in the correct order.

When ordering is important
==========================

Minor updates of any component of AppArmor should not result in
breakage, and those changes should be safe to roll out in any order
that is convenient. For example adding new rules to a policy file,
bug fixes to the compiler, or kernel, should all be safe to roll out
without any concerns to ordering.

When a new feature or semantic change is introduced ordering of role
out is important.

Preparing for changes
=====================

Significant changes should be staged and well tested before being
checked in. Testing needs to include component compatibility
testing. That is new parser on old kernel, and new kernel on old
parser, if either of these have changes.

Check in of changes should occur in the same order as recommended
roll out, to guarantee that the archive is in a consistent working
state for any given snapshot.

Order of roll out
=================

The kernel and parser should be introduced first. The ordering of
these should not generally matter, as long as the kernel keeps it
interface stable. If there is any doubt the parser should be rolled
out first as it is required to work with older and newer interfaces,
because in live update install situations, the new kernel will not
be live until a reboot.

After the parser and kernel are updated, the surrounding tools should
be updated.

The policy is the last component that should be updated. As it may
contain rules expressing new features that require and updated parser
and kernel.
