WARNING: Draft - Work in progress
=================================

Requirements
============

-   Xapparmor XACE plug-in
-   3.0 AppArmor kernel query interface
    -   4.0 AppArmor kernel for userspace caching of permission checks
-   2.?? AppArmor user space

Introduction
============

WARNING: The AppArmor Xace plugg-in is a work in progress and changes
may be made to policy and capabilities before development is done.

AppArmor provides a security extensions for the X server via the Xace
framework. ...

Requirement
===========

-   An AppArmor userspace that supports the Xapparmor language extension
    -   out of tree patches <git://git.launchpad.net/~jjohansen/apparmor>
-   Policy updated with Xapparmor rules
-   AppArmor Kernel module that supports the AppArmor policydb, aa\_getpeercon, and the apparmor query interface
    -   Ubuntu
        -   16.04 Xenial
        -   15.10
        -   15.04
    -   Suse - poinnter to OBS
    -   Out of tree patches: <git://kernel.ubuntu.com/jj/apparmor-backports>
        -   branches
-   XServer that has been patched with the xapparmor security extension
    -   Ubuntu ppa
    -   Suse obs
    -   patches <git://git.launchpad.net/~jjohansen/xorg-server>

Configuring XAppArmor in the **xorg.conf** file
===============================================

If the X-server AppArmor enforcement needs to be run in a specific
mode, then the option may be added to the **`xorg.conf`** file
(normally in ). The option entries are as follows:

  
“AppArmor mode disabled”

“AppArmor mode selective”

“AppArmor mode enforcing”

Note that the entry must be exact otherwise it will be ignored. An example entry is:

```
    Section "Module"
        SubSection "extmod"
            Option "AppArmor mode enforcing"
        EndSubSection
    EndSection
```

The current default mode is **enforcing.**

Policy Lanaguage
================

Example
=======

Security snappy applications on the ubuntu desktop
==================================================

Thanks and Acknowledgements
===========================

This work would not have been possible without the hard work put in
by the people who developed the Xace framework for the X Server and
in particular the XSelunix security extension which was used as a
starting point for the development of the AppArmor Xace plugg-in.
