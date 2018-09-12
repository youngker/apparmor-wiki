Introduction
============

AppArmor 2.13.1 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

Note: These release notes cover all changes between 2.13 (???5)
and 2.13.1 (???).


Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.

    


Detailed changelog
==================

Build Infrastructure
--------------------
-   Fix FTBFS w/older glibc


Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
-   allow specifying the parser config file
-   fix cache write message when stdin is used



Init
----

???

Library
-------

-   add support for multiple policy cache directories
-   add support for overlay cache locations
-   expand ignored file list
    -   add .pacsave
    -   add .pacnew
    -   add .dpkg
    -   add .dpkg-remove
-   expand skippable dir list
    -   add .git

pam_apparmor
------------
-   install pam_apparmor.so with write permission for its owner.


Utils
-----

-   genprof/logprof
    -   Set flags for profiles represented by a glob
    -   properly identify empty ouid/fsuid fields in logs
    -   simplify write_include() and drop write_single()
    -   change 'profile_changes' and 'serialize_opts' to dict()
    -   mark profiles with multiple rules in one line as known-failing
-   aa-status: split profile from exec name
-   aa-nofify
    -   add ability to customize notification message.
    -   set DBUS_SESSION_BUS_ADDRESS, needed by notify-send
    -   comment out use_group to remove group restrictions


Policy
------

-   abstractions
    -   base: allow ld.so.conf and friends.
    -   gnupg: allow pubring.kbx
    -   ubuntu-browsers: fix for 64bit openSUSE
    - add dri-enumerate abstraction
    - add new dri-common abstraction to contain basic DRI-specific rules.
    - move DRI-specific rules into it's own abstraction
    - nvidia
        -   allow reading memory block size
        - allow creating NVIDIA-specific user directories

-   mlmmj-send: allow reading digesters.d/*
-   mlmmj-sub: fix moderated subscription
-   dovecot
    -   config: allow dac_read_search and reading ssl-parameters.dat
    -   auth: allow writing /run/dovecot/old-stats-user
    -   add stats profile, and allow dovecot to run it
    -   dovecot-lda: allow reading anything under /usr/share/dovecot/protocols.d/
- dnsmasq: allow chown capability.
- ntp: allow clockstats


Documentation
-------------
-   add aa-teardown man page
-   aa_policy_cache and aa_features man pages to add new libapparmor functions to support multiple cache dirs, and overlay cache locations
-   update apparmor_parser man page for using overlay cache locations
-   update apparmor.d(7) to document conditional includes
-   update notify.conf man page, and its default configuration
-   update apparmor(7) manpage clarify the effect of reloading a profile.


Translations
------------

-   merge in .de translation updates from launchpad


Tests
-----

-   parser
    -   Update caching tests to use the --print-cache-dir option
    -   fix includes to allow white space
    -   add tests for relative path includes
-   libapparmor
    -   update for multiple caches
    -   update for overlay cache locations
-   utils
    -   make tests less verbose
    -   ignore tests for 'include if exists'  ... and some exotic includes that are not supported by the tools yet    
-   regression tests
    -   fix regression tests to pass on 4.14 upstream kernel