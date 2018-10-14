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
and 2.13.1 (2e922a9a9b31088c9d0f9760f7e1c71f8ad5d4b9) [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).

# Release Location

Tarball
-   <https://launchpad.net/apparmor/2.13/2.13.1/+download/apparmor-2.13.1.tar.gz>
-   sha256sum: 7a060d94c275e59f96bacd1da150e6fee2c9152a85bf57800109d07d51ef8afb
-   signature: <https://launchpad.net/apparmor/2.13/2.13.1/+download/apparmor-2.13.1,tar.gz.asc>

Branch https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13 Commit https://gitlab.com/apparmor/apparmor/commit/2e922a9a9b31088c9d0f9760f7e1c71f8ad5d4b9


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
-   fix Makefile hardcoded paths to flex and bison
-   do not output cache warning for stdin if not using cache
-   ignore abi rules

Init
----
-   fix permissions of apparmor.systemd helper script
-   skip XBPS conffile artifacts


Library
-------
-   do not honor $LIBAPPARMOR_DEBUG when `secure_getenv` is undefined
-   make `aa_policy_cache_add_ro_dir` function visible to fix dynamic linking of parser
-   not purge PMurHash.h on maintainerclean
-   add cache.d to skipable dir list
-   fix failure to create missing cache dir
-   fix build failure when enable-debug-output=yes
-   replace `scandirat` with open-coded variant so that apparmor can be built on musl libc
-   fix dirat_for_each2() fd handling
-   fix: remove empty LD_RUN_PATH from libapparmor-perl

Utils
-----
-  genprof/logprof
   - error out on nested child profiles which are not currently supported
   - fix writing alias rules
   - fix writing "link subset" rules
   - fix overwriting of child profile flags if they differ from the main profile
   - allow for named profiles without and attachment specification
   - add python3.7 to logprof.conf
   - add support for zsh in logprof.conf
   -   add basic support for abi rules to the tools

-   aa-notify
    - make message about notify-send package cross-distro compatible
    - Read user's configuration file from XDG_CONFIG_HOME

-   sandbox.py
    - remove unused exception binding


Policy
------
- Profiles
  - support distributions which merge sbin into bin
  - ping: support void linux binary location
  - traceroute: support void linux binary location
  - dnsmasq
    - add paths for NetworkManager connection sharing
    - add permission to open log files
  - allow running Thunderbird wrapper script
  - ntpd
    - allow access to ntp clockstat
    - add openntpd drift and socket files
    - support void linux binary location
  - samba
    - allow smbd to load new shared libraries
    - allow winbindd to read and write new kerberos cache location
    - nmbd add missing files
    - smbd add missing pid lock file
  - update usr.sbin.useradd to support usr-merge

- Tunables
  - Make variables value more readable by avoiding the use of too many alternations.
  - Add uid and uids kernel var placeholders

- Abstractions
  - add qt5 abstraction
  - add qt5-compose-cache-write abstraction
  - ubuntu-email: add new Thunderbird executable path
  - ubuntu-browsers.d/user-files: disallow access to the dirs of private files
  - private-files: disallow writes to thumbnailer dir (LP: #1788929)
  - private-files-strict: disallow access to the dirs of private files
  - user-files: disallow access to the dirs of private files
  - remove antiquated abstractions/launchpad-integration
  - kde: use qt5 abstration
  - samba: add missing log files
  - add recent documents write abstraction and update abstractions to use it
  - add OpenCL abstraction
  - kde: drop redundant rules for icons access
  - ssl
    - add dehydrated certificate support
    - support new location for ssl-params file
  - php: allow ICU (unicode support) data tables
  - Python:
    - add support for python 3.7
    - allow /usr/local/lib/python3/dist-packages
  - freedesktop.org:
    - factor out duplicated path components with variables
    - treat Flatpak exports the same way as bits shipped by the distro.
    - simplify by not attempting to guess the exhaustive list of files that can exist in {~/.local/share,/usr/share}/applications/.
    - refactor for consistency.
  - nvidia
    - opencl: don't allow PUx on nvidia-modprobe
    - use nvidia_modprobe profile inside opencl-nvidia


Tests
-----
- mount regression test: convert mount test to use MS_NODE
- use --config-file in tests so they are unaffected by the system parser.conf file

Documentation
-------------
- update documentation to references gitlab and updated bug reporting procedures.


Note
====

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.

