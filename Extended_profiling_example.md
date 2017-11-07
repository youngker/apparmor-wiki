**DRAFT**

Profiling a large application - Nagios
======================================

As an example of profiling a large application, Nagios was chosen
since it is a daemon that runs scripts, can connect out to others and
has a web frontend. Profiling Nagios should touch on many aspects of
profiling with AppArmor, ehouch so that by the end of the tutorial,
you should feel comfortable profiling any application. Some AppArmor
features touched on:

-   creating policy
-   modifying policy
-   abstractions
-   reloading policy
-   child profiles
-   policy transitions
-   mod\_apparmor

Main Nagios process
-------------------

Default Ubuntu 10.04 LTS installation

Existing scripts
----------------

Start with ones that are enabled in Ubuntu 10.04 LTS by default.

adding new nagios scripts
-------------------------

-   add check\_apt
-   others

adding nrpe on server
---------------------

TODO

Profiling Nagios web frontend
=============================

-   discuss how we got to the [mod\_apparmor\_example](mod_apparmor_example) profile

Profiling NRPE clients
======================

TODO
