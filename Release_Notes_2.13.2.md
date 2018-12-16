Introduction
============

AppArmor 2.13.2 is an incremental release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.18
kernel and ubuntu 18.04 kernel with the apparmor 3 development patches.

AppArmor 2.13.2 was released 2018-12-16.


# Obtaining the Release
These release notes cover all changes between 2.13.1 (2e922a9a9b31088c9d0f9760f7e1c71f8ad5d4b9) and 2.13.2 (???) [apparmor-2.13 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-2.13).

Tarball
-   <https://launchpad.net/apparmor/2.13/2.13.2/+download/apparmor-2.13.2.tar.gz>
-   sha256sum: ???
-   signature: <https://launchpad.net/apparmor/2.13/2.13.2/+download/apparmor-2.13.2.tar.gz.asc>


Build Infrastructure
--------------------

???
-   fix FTBFS w/older glibc


Policy Compiler (a.k.a apparmor\_parser)
----------------------------------------

???
-   allow specifying the parser config file

Init
----
-   fix permissions of apparmor.systemd helper script
-   skip XBPS conffile artifacts


Library
-------

???
-   do not honor $LIBAPPARMOR_DEBUG when `secure_getenv` is undefined

Utils
-----

???
-  genprof/logprof
   - error out on nested child profiles which are not currently supported

-   aa-notify
    - make message about notify-send package cross-distro compatible
    - Read user's configuration file from XDG_CONFIG_HOME

-   sandbox.py
    - remove unused exception binding


Policy
------
???

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
    - nmbd
      - add missing files
      - support writing to /run/systemd/notify
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