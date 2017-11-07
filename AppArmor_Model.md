AppArmor is a flexible mandatory access control (MAC) extension for
Linux that doesn't enforce any single model. When set up appropriately
it is capable of being used to enforce several different models. With
that said it was designed with a targeted policy in mind and not all
versions of AppArmor are capable of supporting every model.

It is important to note that AppArmor is not a replacement for unix
DAC, or capabilities and it has been designed to work in conjunction
with DAC and capabilities. AppArmor leverages DACs existing labeling
(but not permissions) for owner and groups, and does not have a
concept of user or owner that is separate from the systems user
or owner. Certain security models require that policy be setup in
conjunction with other linux security controls, such as owner and
groups and the pam\_cap module.

What AppArmor supports
======================

AppArmor is in a state of development and so it currently does not
support all the features needed to provide confinement ...

It started with file and capability rules

Policy models
=============

AppArmor is capable (or will be) any of the following policy models.

Targeted - Targeted is the standard way in which AppArmor is
deployed. It trades off the security of a totally system look down
for easy of use and policy authoring, by recognizing that there
are set of high value applications that can be more easily confined
than the entire system while still providing value. In a targeted
policy only a selection of the task on a system are confined in
any meaningful way. Usually the targeted applications are network
facing apps. ??? something about it being hard to confine the desktop
??? Total system - Total system confinement involves confining every
application on the system. ??? what is missing from current apparmor
to do this well ??? link to doc explaining how to load policy in the
initrd, so that you can confine init confining all applications RBAC -
Role based access control can be done as a total system confinement or
a per Role confinement but all tasks within a role must be completely
confined. ???? MLS - pam\_cap + capability rules

???
===

basics of how AppArmor works - profile/domain centric - lazy static
labeling & hybrid labels (file, networks) - capabilities - ipc -
rlimits - DAC
