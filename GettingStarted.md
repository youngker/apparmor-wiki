A Quick Overview
----------------

AppArmor is Mandatory Access Control (MAC) like security system for
Linux. AppArmor confines individual programs to a set of files,
capabilities, network access and rlimits, collectively known as
the AppArmor policy for the program, or simply as a profile. New
or modified policy can be applied to the running system without
a reboot. AppArmor aims to be easy to understand and use for most
common requirements by presenting its profiles in an administrator
friendly language.

AppArmor is selective in its confinement such that some programs on the
system may be confined, while others not. Selective confinement allows
an administrator the flexibility to turn off a problematic profile
for troubleshooting while leaving other parts of the system confined.

Unconfined programs are run under standard Linux Discretionary Access
Control (DAC) security. AppArmor augments traditional DAC in that
confined programs are evaluated under traditional DAC first and if
DAC allows the behavior then AppArmor policy consulted.

AppArmor supports per-profile learning (complain) mode to help users
write and maintain policy. Learning mode allows for a profile to be
created by running a program normally and learning its behavior. After
AppArmor has sufficiently learned the behavior, the profile may
be turned to enforcing mode. While the resulting profile may be
more lenient than a hand-crafted profile tailored for a specific
environment and application, learning mode can greatly reduce the
effort and knowledge needed to use AppArmor and add an important
layer of security.

Versions of AppArmor
--------------------

There are two main versions of AppArmor the 2.x series (current) and
the 3.x series (development). The 2.x series has seen incremental
improvements over its life with only incremental breaks in
semantic compatibility. The 3.x series is a significant expansion of
AppArmor. Both of the main series use the same basic policy language,
with only minor semantic differences. The 3.x series allows for a
much expanded policy and fine-grained control.
