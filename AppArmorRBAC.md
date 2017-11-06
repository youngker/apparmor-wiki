**WARNING** this page is being heavily edited

Using AppArmor to confine users and do RBAC
===========================================

To achieve RBAC apparmor uses a combination of
two[PAM](http://en.wikipedia.org/wiki/Pluggable_Authentication_Modules)
security modules. The pam\_cap.so module is used to raise a users
privileges while the pam\_apparmor.so module is used to further
restrict the users processes from what would be possible with the
granted capabilities.

This document only covers the suggested method of achieving RBAC
in the current version of AppArmor (2.5), documentation on earlier
versions is linked below.

Requirements
============

-   pam\_cap.so
-   pam\_apparmor.so

Setting up pam\_cap.so
======================

This is just a quick overview of how to set up the pam\_cap.so module,
for a more in depth explanation see following links

-   [posix file caps](http://www.friedhoff.org/posixfilecaps.html)
-   [linuxjournal](http://www.linuxjournal.com/article/10249)

installing pam\_cap.so
----------------------

-   Ubuntu
    -   from a terminal type - `sudo apt-get install libcap2 libcap2-bin`
    -   synaptics - search for libcap2 and select libcap2, libcap2-bin for installation and click apply

configuring PAM to use pam\_cap.so
----------------------------------

Editing the pam\_cap.so configuration file
------------------------------------------

/etc/security/capability.conf

Setting up pam\_apparmor.so
===========================

Use pam\_apparmor to assign profiles and or profile namespaces to a
user or process.

installing pam\_apparmor.so
---------------------------

-   Ubuntu
    -   from a terminal type - `sudo apt-get install libpam-apparmor`

configuring PAM to use pam\_apparmor.so
---------------------------------------

Editing the pam\_apparmor.so configuration file
-----------------------------------------------

### Specifying profiles

### Specifying namespaces

RBAC Policy choices
===================

AppArmor provides three alternative ways that RBAC policy can be
authored.

Single profile
--------------

Instead of breaking up permission per application (standard AppArmor
model), all permissions are encapsulated by a single profile that
confines all applications run by the user. This results

ix transitions Any application that needs

Profile tree
------------

Profile Namespace
-----------------

Profile Stacking (AppArmor 2.6)
-------------------------------

AppArmor 2.6 opens up another possible method of applying RBAC policy,
by allowing for composition of profiles through profile stacking. This
allow for the application of the regular profile set and a user
confining profile without having to create a custom profile tree or
custom profile namespace.

To do this a user profile is created that encompasses all the
permissions the user should have, and then a new profile layer is
created that is pointed at the other policy that should be applied
(eg. the standard set of profiles). Now any tasks run by the confined
user will have a combined policy applied.

### user profile considerations

-   leaving / collapsing the stack??

Outer single user profile + layer of regular profiles or new namespace

Links
=====

AppArmor has evolved over time, and with each new version features have
been added that affect how RBAC style confinement can be achieved in
AppArmor. In general each version of AppArmor builds on the previous
version so that the techniques used in previous version can still be
used. However each newer version either adds features that refine,
and improve on previous techniques, or introduces new techniques
making RBAC style policy easier and more flexible.

-   1.x - AppArmor versions prior to 2.0 exist only in the linux distro Immunix and are not covered here.
-   [ RBAC in AppArmor 2.0](RBAC_2_0) - Basic techniques with some notes on improvements
-   [ RBAC in AppArmor 2.1](RBAC_2_1) - openSUSE 10.3, Ubuntu 7.10 (Gutsy Gibbon), 2.1+ - Ubuntu 8.04 (Hardy Heron)
-   [ RBAC in AppArmor 2.3-2.4](RBAC_2_3) - openSUSE 11.0, openSUSE 11.1, Ubuntu 8.10 (Intrepid Ibex), Ubuntu 9.04, Ubuntu 9.10 (Karmic)
-   [ RBAC in AppArmor 2.5](RBAC_2_5) - development versions
-   [ Multilevel style security](AppArmorMLS) can be achieved by extending in AppArmor ??? by extending the RBAC techniques.

