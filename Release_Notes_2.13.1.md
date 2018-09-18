Introduction
============

AppArmor 2.13.1 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

Note: These release notes cover all changes between 2.13 (f97782b100733770eebc7cf2839ba43683a74f46)
and 2.13.1 (???) [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).


Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.


Build Infrastructure
--------------------
-   fix FTBFS w/older glibc
-   fix install loction of aa-teardown
-   coverity: capture separate log files for each coverity invocation


Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------
-   allow specifying the parser config file
-   fix cache write message when stdin is used
-   provide typedefs for comparison_fn_t and __free_fn_t to fix build issues on musl based systems
-   fix bugs in --config-file handling
-   make --config-file so it is no longer required to be the first option passed to the parser
-   move default cache location to /var/cache

Init
----
-   fix permissions of apparmor.systemd helper script


Library
-------
-   do not honor $LIBAPPARMOR_DEBUG when `secure_getenv` is undefined
-   make `aa_policy_cache_add_ro_dir` function visible to fix dynamic linking of parser
-   not purge PMurHash.h on maintainerclean
-   add cache.d to skipable dir list
-   fix failure to create missing cache dir
-   fix build failure when enable-debug-output=yes
-   replace `scandirat` with open-coded variant so that apparmor can be built on musl libc


Utils
-----
-  genprof/logprof
   - error out on nested child profiles which are not currently supported
   - fix writing alias rules
   - fix writing "link subset" rules
   - fix overwriting of child profile flags if they differ from the main profile
   - allow for named profiles without and attachment specification
   - add python3.7 to logprof.conf

-   aa-notify
    - make message about notify-send package cross-distro compatible

-   sandbox.py
    - remove unused exception binding


Policy
------
- Profiles
  - update samba
  - update usr.sbin.useradd to support usr-merge

- Tunables
  - Make variables value more readable by avoiding the use of too many alternations.

- Abstractions
  - add recent documents write abstraction and update abstractions to use it
  - add OpenCL abstraction
  - kde: drop redundant rules for icons access
  - ssl: add dehydrated certificate support
  - php: allow ICU (unicode support) data tables
  - Python:
    - add support for python 3.7
    - allow /usr/local/lib/python3/dist-packages
  - freedesktop.org:
    - factor out duplicated path components with variables
    - treat Flatpak exports the same way as bits shipped by the distro.
    - simplify by not attempting to guess the exhaustive list of files that can exist in {~/.local/share,/usr/share}/applications/.
    - refactor for consistency.


Tests
-----
- mount regression test: convert mount test to use MS_NODE
- use --config-file in tests so they are unaffected by the system parser.conf file

Documentation
-------------
- update documentation to references gitlab and updated bug reporting procedures.

