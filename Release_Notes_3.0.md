# Warning populating the Changes in this release is a WIP will be fully updated in the next few days

AppArmor 3.0 was released 2020-06-07.

# Introduction

AppArmor 3.0 is a major new release of the AppArmor user space that makes an important change to policy development and support. Its focus is transitioning policy to the new features abi and as such other new features have been limited.

Apprmor 3.0 is a bridge release between older AppArmor 2.x policy and the newer AppArmor 3 style policy which requires the declaration of a features abi. As such AppArmor 3.0 will be a short lived release, and will not receive long term support. The following AppArmor 3.1 feature release is planned to be a regular release, please take this into account when including AppArmor 3.0 into a distro release.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied). And supports features released in the 4.20
kernel.

The kernel portion of the project is maintained and pushed separately.


# Highlighted new features

- Policy now must declare the feature abi it was developed for if it is to use any new features. For further information please see the [wiki](AppArmorpolicyfeaturesabi).
- The use of profile names that are based on pathnames are deprecated. For further information please see the [wiki](DeprecateProfilePathName).
- Support for new kernel features (requires appropriate features abi tagging in policy)
  - upstream v8 network socket rules
  - xattr attachment conditionals
  - capabilities PERFMON and BPF
- rewritten aa-status
  - supports use in systems/images where python is not available
  - supports kill, unconfined and mixed profile modes
- rewritten aa-notify
  - move from perl to python 3
  - shared backend with other python tools
  - support use of aa.CONFDIR instead of hard coded /etc/apparmor
  - improved message layout
- improved support for kernels that support LSM stacking
- support profile modes
  - enforce (default when no mode flag is supplied)
  - kill (experimental)
  - unconfined (experimental)
- reference policy updated for 3.0 feature abi
- basic support for [systemd v246 early load of apparmor policy](https://gitlab.com/apparmor/apparmor/-/wikis/AppArmorInSystemd).

# Important Notes

- Potentially breaking change: AppArmor will now issue warning about policy that does not specify a feature abi if that policy is not pinned to a specific feature abi. AppArmor will compile such policy using a default feature abi instead of the kernels abi. For more information see the [wiki](AppArmorpolicyfeaturesabi).

- Potentially breaking change: AppArmor no longer loads snapd policy by default. It is expected that snapd users are using the snapd unit file. If this is not the case distros will need to revert ```0164fd05 init: stop loading snap policy``` OR take advantage of systemd v246 early load of apparmor policy.

# Obtaining the Release
These release notes cover all changes between 2.13 (f97782b100733770eebc7cf2839ba43683a74f46) and 3.0 (???) [apparmor-3.0 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-3.0).

Tarball
-   <https://launchpad.net/apparmor/3.0/3.0/+download/apparmor-3.0.tar.gz>
-   sha256sum: 
-   signature: <https://launchpad.net/apparmor/3.0/3.0/+download/apparmor-3.0.tar.gz.asc>

# Changes in this Release

Includes all the bug fixes and improvements in
- [2.13.1](Release_Notes_2.13.1)
- [2.13.2](Release_Notes_2.13.2)
- [2.13.3](Release_Notes_2.13.3)
- [2.13.4](Release_Notes_2.13.4)
- [2.13.5](Release_Notes_2.13.5)


And the following improvements

## General improvments
- drop obsolete code ([MR:423](https://gitlab.com/apparmor/apparmor/-/merge_requests/423),[LP:692406](https://bugs.launchpad.net/bugs/692406),[MR:416](https://gitlab.com/apparmor/apparmor/-/merge_requests/416))
- remove use of perl as dependency for regular use (still required as a build/test dependency)
- fix typos, and formating
- update comments
- update references to wiki, and gitlab

## Build Infrastructure
- Fix coverity scans ([MR:145](https://gitlab.com/apparmor/apparmor/-/merge_requests/145))
- cleanup Makefiles ([MR:272](https://gitlab.com/apparmor/apparmor/-/merge_requests/272),)
- CI Drop specifying python and pyflakes version ([MR:483](https://gitlab.com/apparmor/apparmor/-/merge_requests/483))
- cleanup compiler flags and make consistent


## Policy Compiler (a.k.a apparmor\_parser)
- make --config-file so it is no longer required to be the first option passed to the parser
- move default location of cache to /var/cache/apparmor/ ([MR:156](https://gitlab.com/apparmor/apparmor/-/merge_requests/156),[debug904637](https://bugs.debian.org/904637))
- output deprecated warning when using filenames as a profile name ([MR:506](https://gitlab.com/apparmor/apparmor/-/merge_requests/506))
- Add the ability to turn off jobs to ease with debugging ([MR:105](https://gitlab.com/apparmor/apparmor/-/merge_requests/105))
- Improve warn flags and add support for -Werror ([MR:600](https://gitlab.com/apparmor/apparmor/-/merge_requests/600))
- improve capability handling so it is not variable to which kernel headers are installed ([MR:578](https://gitlab.com/apparmor/apparmor/-/merge_requests/578))
- support experimental enforce, kill and unconfined profile modes ([MR:440](https://gitlab.com/apparmor/apparmor/-/merge_requests/440),[AABUG:7](https://gitlab.com/apparmor/apparmor/-/issues/7))
- add support for out-of-band transitions ([MR:414](https://gitlab.com/apparmor/apparmor/-/merge_requests/414))
- Support xattr exec attachment conditional ([MR:270](https://gitlab.com/apparmor/apparmor/-/merge_requests/270))
- improve static attachment overlap analysis providing better resolution when profile attachments overlap during exec  ([MR:326](https://gitlab.com/apparmor/apparmor/-/merge_requests/326))
- improve cache handling when optimizations are used ([MR:385](https://gitlab.com/apparmor/apparmor/-/merge_requests/385))
- limit the number of passes expr tree simplification does to avoid degenerate cases.
- add missing permissions generation for using new kernel interfaces used by LSM stacking
- fix various build errors, warnings, and refactor code ([MR:196](https://gitlab.com/apparmor/apparmor/-/merge_requests/196),[MR:196](https://gitlab.com/apparmor/apparmor/-/merge_requests/196),[MR:196](https://gitlab.com/apparmor/apparmor/-/merge_requests/196),[MR:616](https://gitlab.com/apparmor/apparmor/-/merge_requests/616),[MR:608](https://gitlab.com/apparmor/apparmor/-/merge_requests/608),[MR:569](https://gitlab.com/apparmor/apparmor/-/merge_requests/569),[MR:590](https://gitlab.com/apparmor/apparmor/-/merge_requests/590),[MR:561](https://gitlab.com/apparmor/apparmor/-/merge_requests/561),[MR:146](https://gitlab.com/apparmor/apparmor/-/merge_requests/146))
- Fix debug builds [MR:609](https://gitlab.com/apparmor/apparmor/-/merge_requests/609)
- remove resource leaks ([MR:439](https://gitlab.com/apparmor/apparmor/-/merge_requests/439))
- fix cross compilation with libintl ([MR:485](https://gitlab.com/apparmor/apparmor/-/merge_requests/485))
- remove deprecated dynamic exception specifications ([MR:356](https://gitlab.com/apparmor/apparmor/-/merge_requests/356))
- Improve error message format ([MR:610](https://gitlab.com/apparmor/apparmor/-/merge_requests/610))
- cleanups and distro compatibility improvements ([MR:512](https://gitlab.com/apparmor/apparmor/-/merge_requests/512))
- fix python deprecation warnings ([MR:492](https://gitlab.com/apparmor/apparmor/-/merge_requests/492))




## Init
- drop dead code ([MR:269](https://gitlab.com/apparmor/apparmor/-/merge_requests/269),[MR:263](https://gitlab.com/apparmor/apparmor/-/merge_requests/263))
- cleanup handling of mountpoints ([MR:394](https://gitlab.com/apparmor/apparmor/-/merge_requests/394))
- add is_container_with_internal_policy() function. ([MR:252](https://gitlab.com/apparmor/apparmor/-/merge_requests/252),[debug917874](https://bugs.debian.org/917874),[LP:1377338](https://bugs.launchpad.net/bugs/1377338))
- set SFS_MOUNTPOINT in is_container_with_internal_policy() ([MR:363](https://gitlab.com/apparmor/apparmor/-/merge_requests/363))
  - Adjust cache paths in apparmor.service ([MR:134](https://gitlab.com/apparmor/apparmor/-/merge_requests/134))
- fix return codes ([MR:256](https://gitlab.com/apparmor/apparmor/-/merge_requests/256))
- ignore .orig and .rej files when loading profiles ([MR:282](https://gitlab.com/apparmor/apparmor/-/merge_requests/282))
- fix issues detected by shellcheck ([MR:293](https://gitlab.com/apparmor/apparmor/-/merge_requests/293))
- Don't try to list files in a non-existent directories ([MR:252](https://gitlab.com/apparmor/apparmor/-/merge_requests/252),[debug917874](https://bugs.debian.org/917874),[LP:1377338](https://bugs.launchpad.net/bugs/1377338))
- add support for an additional profiles directory, defaulting to /var/lib/snapd/apparmor/profiles ([MR:252](https://gitlab.com/apparmor/apparmor/-/merge_requests/252),[debug917874](https://bugs.debian.org/917874),[LP:1377338](https://bugs.launchpad.net/bugs/1377338))
- warn when the profiles directory cannot be found ([MR:252](https://gitlab.com/apparmor/apparmor/-/merge_requests/252)[debug917874](https://bugs.debian.org/917874),[LP:1377338](https://bugs.launchpad.net/bugs/1377338))
- suppress warnings when booting in quiet mode ([MR:252](https://gitlab.com/apparmor/apparmor/-/merge_requests/252),[debug917874](https://bugs.debian.org/917874),[LP:1377338](https://bugs.launchpad.net/bugs/1377338))
- use the parser's automatic parallelization ([MR:252](https://gitlab.com/apparmor/apparmor/-/merge_requests/252),[debug917874](https://bugs.debian.org/917874),[LP:1377338](https://bugs.launchpad.net/bugs/1377338)) 
- make posix-compatible ([MR:355](https://gitlab.com/apparmor/apparmor/-/merge_requests/355),[LP:1377338](https://bugs.launchpad.net/bugs/1377338))
- improve slackware support ([MR:432](https://gitlab.com/apparmor/apparmor/-/merge_requests/432))
- aa-teardown
  - install in /usr/sbin not /sbin ([MR:97](https://gitlab.com/apparmor/apparmor/-/merge_requests/97))
  - use /bin/sh instead of /bin/bash ([MR:115](https://gitlab.com/apparmor/apparmor/-/merge_requests/115))



## Library
  - build with -fPIC ([MR:422](https://gitlab.com/apparmor/apparmor/-/merge_requests/422))
  - make compile flags consistent, and add -Wimplicit-fallthrough ([MR:549](https://gitlab.com/apparmor/apparmor/-/merge_requests/549),[MR:403](https://gitlab.com/apparmor/apparmor/-/merge_requests/403))
  - fix signed vs unsigned comparison issues ([MR:549](https://gitlab.com/apparmor/apparmor/-/merge_requests/549),[MR:558](https://gitlab.com/apparmor/apparmor/-/merge_requests/558))
  - fix warning from new compiler warn flags ([MR:561](https://gitlab.com/apparmor/apparmor/-/merge_requests/561))
  - require python3 ([MR:481](https://gitlab.com/apparmor/apparmor/-/merge_requests/481))
  - support new apparmor proc attr interfaces
  - Make libapparmor LSM stacking aware
  - Support alternate libc implementations ([MR:245](https://gitlab.com/apparmor/apparmor/-/merge_requests/245),[debug909966](https://bugs.debian.org/909966),[debug798955](https://bugs.debian.org/798955))
  - improve CI integration ([MR:434](https://gitlab.com/apparmor/apparmor/-/merge_requests/434),[MR:433](https://gitlab.com/apparmor/apparmor/-/merge_requests/433))
  - Fix cross compilation ([MR:462](https://gitlab.com/apparmor/apparmor/-/merge_requests/462))
  - update API documentation ([MR:559](https://gitlab.com/apparmor/apparmor/-/merge_requests/559))  
  - new api fn to lookup of features value ([MR:578](https://gitlab.com/apparmor/apparmor/-/merge_requests/578))
  - add log testcases for exec with and without target= ([MR:405](https://gitlab.com/apparmor/apparmor/-/merge_requests/405))





## Utils
- fix cross compilation with libintl ([MR:485](https://gitlab.com/apparmor/apparmor/-/merge_requests/485))
- fix build to use compiler warning flags ([MR:542](https://gitlab.com/apparmor/apparmor/-/merge_requests/542))
- fix cleanup
- aa-enabled
  - make LSM stacking aware
- aa-status
  - rewrite so it standalone and doesn't have external dependencies so it can be used in minimal installs ([MR:473](https://gitlab.com/apparmor/apparmor/-/merge_requests/473),[LP:1865519](https://bugs.launchpad.net/bugs/1865519))
  - make LSM stacking aware
  - support mixed, kill, and unconfined profile modes
  - bump json version
- aa-exec
  - Improve error, debug and verbose messages ([MR:128](https://gitlab.com/apparmor/apparmor/-/merge_requests/128))
  - Improve handling of errors when changing profiles fail ([MR:129](https://gitlab.com/apparmor/apparmor/-/merge_requests/129))
  - Error out on conflicting aa-exec parameters ([MR:540](https://gitlab.com/apparmor/apparmor/-/merge_requests/540))
  - fix warning from new compiler warn flags ([MR:561](https://gitlab.com/apparmor/apparmor/-/merge_requests/561))
- aa-decode
  - rewrite in python ([MR:321](https://gitlab.com/apparmor/apparmor/-/merge_requests/321))
- aa-notify
  - Re-implement aa-notify in Python ([MR:341](https://gitlab.com/apparmor/apparmor/-/merge_requests/341),[AABUG:16](https://gitlab.com/apparmor/apparmor/-/issues/16))
  - use aa.CONFDIR to find configuration ([MR:372](https://gitlab.com/apparmor/apparmor/-/merge_requests/372))

## cboltz utils  TODO
- fix writing of alias rules ([MR:119](https://gitlab.com/apparmor/apparmor/-/merge_requests/119))
- error out when alias declared inside of a profile ([MR:116](https://gitlab.com/apparmor/apparmor/-/merge_requests/116))
- improve testing output ([MR:194](https://gitlab.com/apparmor/apparmor/-/merge_requests/194))
- add support for xattr attachment conditionals ([MR:285](https://gitlab.com/apparmor/apparmor/-/merge_requests/285))
- Extend common DebugLogger with option to log to stderr ([MR:325](https://gitlab.com/apparmor/apparmor/-/merge_requests/325))
- fully support 'include if exists' ([MR:499](https://gitlab.com/apparmor/apparmor/-/merge_requests/499),[MR:507](https://gitlab.com/apparmor/apparmor/-/merge_requests/507),[LP:1738879](https://bugs.launchpad.net/bugs/1738879))
- support abi rules ([MR:525](https://gitlab.com/apparmor/apparmor/-/merge_requests/525))




## Policy
- improve CI integration of shipped policy ([MR:449](https://gitlab.com/apparmor/apparmor/-/merge_requests/449))
- add permissions to access new apparmor api interfaces where necessary
- use @{PROC} and @{run} throughout policy ([MR:455](https://gitlab.com/apparmor/apparmor/-/merge_requests/455))
- adjust local include to use "include if exists" ([MR:489](https://gitlab.com/apparmor/apparmor/-/merge_requests/489)
- tag policy with te AppArmor 3.0 abi ([MR:491](https://gitlab.com/apparmor/apparmor/-/merge_requests/491),[MR:555](https://gitlab.com/apparmor/apparmor/-/merge_requests/555))
- move from "#include" to "include" ([MR:553](https://gitlab.com/apparmor/apparmor/-/merge_requests/553),[MR:563](https://gitlab.com/apparmor/apparmor/-/merge_requests/563))
- new @{etc_ro} and @{etc_rw} variables and use in policy ([MR:585](https://gitlab.com/apparmor/apparmor/-/merge_requests/585))

#### abstractions
  - General changes
    - update abstractions to support local additions directory
    - test abstractions against apparmor_parser ([MR:237](https://gitlab.com/apparmor/apparmor/-/merge_requests/237))
  - apache2-common
    - fix path for apache2 stapling-cache ([MR:133](https://gitlab.com/apparmor/apparmor/-/merge_requests/133))
  - base
    - allow mr on *.so* in common library paths ([MR:345](https://gitlab.com/apparmor/apparmor/-/merge_requests/345),[Tails16414](https://redmine.tails.boum.org/code/issues/16414))
    - support suse /usr/etc/ ([MR:447](https://gitlab.com/apparmor/apparmor/-/merge_requests/447),[BOO:1161756](http://bugzilla.opensuse.org/show_bug.cgi?id=1161756))
    - add FIPS-140-2 lib validation hmac files to abstractions/base ([MR:595](https://gitlab.com/apparmor/apparmor/-/merge_requests/595),Bug: [LP:1891664](https://bugs.launchpad.net/bugs/1891664))
  - dbus-session-strict
    - make x11 socket read-only([MR:281](https://gitlab.com/apparmor/apparmor/-/merge_requests/281))
  - dri-common
    - added ability to read /usr/share/drirc.d/ directory ([MR:314](https://gitlab.com/apparmor/apparmor/-/merge_requests/314),[MR:308](https://gitlab.com/apparmor/apparmor/-/merge_requests/308))
  - exo-open
    - new abstrction ([MR:404](https://gitlab.com/apparmor/apparmor/-/merge_requests/404),[MR:467](https://gitlab.com/apparmor/apparmor/-/merge_requests/467),[AABUG:](https://gitlab.com/apparmor/apparmor/-/issues/73))
  - fonts
    - updat for Debian and openSUSE ([MR:96](https://gitlab.com/apparmor/apparmor/-/merge_requests/96))
  - freedesktop.org
    - treat Flatpak exports the same way as bits shipped by the distro ([MR:71](https://gitlab.com/apparmor/apparmor/-/merge_requests/71))
  - gio-open
    - new abstrction ([MR:404](https://gitlab.com/apparmor/apparmor/-/merge_requests/404))
  - gvfs-open
    - new abstrction ([MR:404](https://gitlab.com/apparmor/apparmor/-/merge_requests/404))
  - hosts_access
    - new abstraction for tcpwrappers style access to hosts files ([MR:605](https://gitlab.com/apparmor/apparmor/-/merge_requests/605),[LP:1864466](https://bugs.launchpad.net/bugs/1864466))
  - kde
    - treat Flatpak exports the same way as bits shipped by the distro ([MR:71](https://gitlab.com/apparmor/apparmor/-/merge_requests/71))
  - kde-open
    - new abstrction ([MR:404](https://gitlab.com/apparmor/apparmor/-/merge_requests/404))
  - launchpad-integration
    - disallow writes to thumbnailer dir ([MR:203](https://gitlab.com/apparmor/apparmor/-/merge_requests/203),[LP:1788929](https://bugs.launchpad.net/bugs/1788929))
  - mesa
    - new abstraction ([MR:137](https://gitlab.com/apparmor/apparmor/-/merge_requests/137))
    - allow locking shader cache ([MR:148](https://gitlab.com/apparmor/apparmor/-/merge_requests/148))
  - nameservice
    - allow accessing the libnss-systemd VarLink sockets and DBus APIs ([MR:480](https://gitlab.com/apparmor/apparmor/-/merge_requests/480))
    - support suse /usr/etc/ ([MR:447](https://gitlab.com/apparmor/apparmor/-/merge_requests/447),[BOO:1161756](http://bugzilla.opensuse.org/show_bug.cgi?id=1161756))
  - nvidia
    - allow reading application profiles ([MR:125](https://gitlab.com/apparmor/apparmor/-/merge_requests/125))
  - nss-systemd
    - allow accessing the libnss-systemd VarLink sockets and DBus APIs ([MR:480](https://gitlab.com/apparmor/apparmor/-/merge_requests/480))
  - opencl
    - new abstraction support for POCL, Mesa Clover, Intel Beignet and NVIDIA CUDA ([MR:124](https://gitlab.com/apparmor/apparmor/-/merge_requests/124))
  - opencl-nvidia
    - disallow writes to thumbnailer dir ([MR:203](https://gitlab.com/apparmor/apparmor/-/merge_requests/203),[LP:1788929](https://bugs.launchpad.net/bugs/1788929))
  - php-fpm
    - fix worker name ([MR:619](https://gitlab.com/apparmor/apparmor/-/merge_requests/619))
  - private_files
    - deny access to mutt files ([MR:276](https://gitlab.com/apparmor/apparmor/-/merge_requests/276))
    - disallow writes to thumbnailer dir ([MR:203](https://gitlab.com/apparmor/apparmor/-/merge_requests/203),[LP:1788929](https://bugs.launchpad.net/bugs/1788929))
  - private_files_strict
    - deny access to aws files ([MR:276](https://gitlab.com/apparmor/apparmor/-/merge_requests/276))
    - disallow writes to thumbnailer dir ([MR:203](https://gitlab.com/apparmor/apparmor/-/merge_requests/203),[LP:1788929](https://bugs.launchpad.net/bugs/1788929))
  - qt5
    - new abstraction with common rules for qt5 based applications ([MR:99](https://gitlab.com/apparmor/apparmor/-/merge_requests/99))
  - qt5-compose-cache-write
    - fix anonymous shared memory access ([MR:302](https://gitlab.com/apparmor/apparmor/-/merge_requests/301))
  - qt5-settings-write
    - fix anonymous shared memory access ([MR:302](https://gitlab.com/apparmor/apparmor/-/merge_requests/302))
  - samba
    - allow smbd to load new shared libraries ([MR:121](https://gitlab.com/apparmor/apparmor/-/merge_requests/121),[BOO:1092099](https://bugzilla.opensuse.org/show_bug.cgi?id=1092099))
    - allow winbindd to read and write new kerberos cache location ([MR:121](https://gitlab.com/apparmor/apparmor/-/merge_requests/121),[BOO:1092099](https://bugzilla.opensuse.org/show_bug.cgi?id=1092099))
    - support Arch ([MR:411](https://gitlab.com/apparmor/apparmor/-/merge_requests/411))
  - ssl_certs
    - add letsencrypt support ([MR:283](https://gitlab.com/apparmor/apparmor/-/merge_requests/283))
    - update for dehydrated ([MR:299](https://gitlab.com/apparmor/apparmor/-/merge_requests/299))
    - add dehydrated certificate location ([MR:161](https://gitlab.com/apparmor/apparmor/-/merge_requests/161))
    - add for CertBot on openSUSE Leap ([MR:397](https://gitlab.com/apparmor/apparmor/-/merge_requests/397))
  - ssl_keys
    - add letsencrypt support ([MR:283](https://gitlab.com/apparmor/apparmor/-/merge_requests/283))
    - update for dehydrated ([MR:299](https://gitlab.com/apparmor/apparmor/-/merge_requests/299))
    - add dehydrated certificate location ([MR:161](https://gitlab.com/apparmor/apparmor/-/merge_requests/161))
    - add for CertBot on openSUSE Leap ([MR:397](https://gitlab.com/apparmor/apparmor/-/merge_requests/397))
  - ubuntu-browsers
    - remove obsolete NPAPI support ([MR:417](https://gitlab.com/apparmor/apparmor/-/merge_requests/417))
    - disallow writes to thumbnailer dir ([MR:203](https://gitlab.com/apparmor/apparmor/-/merge_requests/203),[LP:1788929](https://bugs.launchpad.net/bugs/1788929))
  - ubuntu-email
    - support launching Thunderbird
  - Vulkan
    - add new abstraction ([MR:126](https://gitlab.com/apparmor/apparmor/-/merge_requests/126))
  - wayland
    - make x11 socket read-only ([MR:281](https://gitlab.com/apparmor/apparmor/-/merge_requests/281))
  - X
    - make x11 socket read-only ([MR:281](https://gitlab.com/apparmor/apparmor/-/merge_requests/281))
  - xdg-open
    - new abstrction ([MR:404](https://gitlab.com/apparmor/apparmor/-/merge_requests/404))

#### profiles
  - General changes
    - move profiles to short names with attachments instead of file pathnames ([MR:565](https://gitlab.com/apparmor
    - support distributions which merge sbin into bin
/apparmor/-/merge_requests/565),[MR:620](https://gitlab.com/apparmor/apparmor/-/merge_requests/620))
  - chromium profile
    - import from ubuntu and fixup to be more generic ([MR:606](https://gitlab.com/apparmor/apparmor/-/merge_requests/606))
    - cleanup comments, /lib/ and /usr/lib cleanups, suport lib{32,64} ([MR:611](https://gitlab.com/apparmor/apparmor/-/merge_requests/611))
  - dhclient
    - Add path for Fedora/RHEL/CentOS/Oracle Linux ([MR:291](https://gitlab.com/apparmor/apparmor/-/merge_requests/291))
    - Support correct path for lease file on IPv6 ([MR:291](https://gitlab.com/apparmor/apparmor/-/merge_requests/291))
    - fix for debian buster ([MR:570](https://gitlab.com/apparmor/apparmor/-/merge_requests/570))
  - dhcpd
    - Support lease file for IPv6 ([MR:291](https://gitlab.com/apparmor/apparmor/-/merge_requests/291))
  - dnsmasq
    - Add pid file used by NetworkManager ([MR:288](https://gitlab.com/apparmor/apparmor/-/merge_requests/288))
    - Adjust pattern for log files to comply SELinux ([MR:288](https://gitlab.com/apparmor/apparmor/-/merge_requests/288))
    - allow peer=libvirtd to support named profile ([MR:304](https://gitlab.com/apparmor/apparmor/-/merge_requests/304),[BOO:1118952](https://bugzilla.opensuse.org/show_bug.cgi?id=1118952))
    - add paths for NetworkManager connection sharing
  - dovecot
    - allow access to suid_dumpable ([MR:286](https://gitlab.com/apparmor/apparmor/-/merge_requests/286))
    - move from ssl to OpenSSL access ([MR:337](https://gitlab.com/apparmor/apparmor/-/merge_requests/337))
    - auth processes need to read from postfix auth socket ([MR:336](https://gitlab.com/apparmor/apparmor/-/merge_requests/336))
    - let dovecot/anvil rw the auth-penalty socket ([MR:336](https://gitlab.com/apparmor/apparmor/-/merge_requests/336))
    - allow chroot'ing the auth processes ([MR:336](https://gitlab.com/apparmor/apparmor/-/merge_requests/336))
    - allow FD passing between dovecot and dovecot's anvil ([MR:336](https://gitlab.com/apparmor/apparmor/-/merge_requests/336))
    - align {pop3,managesieve}-login to imap-login ([MR:338](https://gitlab.com/apparmor/apparmor/-/merge_requests/338))
    - restrict access by owner to the dovecot/config socket ([MR:338](https://gitlab.com/apparmor/apparmor/-/merge_requests/338))
    - align {pop3,managesieve}-login to imap-login ([MR:338](https://gitlab.com/apparmor/apparmor/-/merge_requests/338))
    - add configuration files created by openresolv ([MR:457](https://gitlab.com/apparmor/apparmor/-/merge_requests/457))
    - allow apparmor api access ([MR:566](https://gitlab.com/apparmor/apparmor/-/merge_requests/566))
  - fingerd
    - needs lock access on /run/utmp ([MR:153](https://gitlab.com/apparmor/apparmor/-/merge_requests/153))
  - freshclam
    - add pidfile and /var/lib/clamav access ([MR:381](https://gitlab.com/apparmor/apparmor/-/merge_requests/381))
  - lighttpd
    - minor updates, added Debian/Ubuntu integration ([MR:153](https://gitlab.com/apparmor/apparmor/-/merge_requests/153))
  - lsb_release
    - added permissions needed by openSUSE implementation ([MR:154](https://gitlab.com/apparmor/apparmor/-/merge_requests/154))
  - mysqld
    - Allow mysqld directory for MySQL PID file ([MR:450](https://gitlab.com/apparmor/apparmor/-/merge_requests/450))
  - nmdb
    - support Arch ([MR:411](https://gitlab.com/apparmor/apparmor/-/merge_requests/411))
  - ntpd
    - allow access to ntp clockstat
    - add openntpd drift and socket files
    - support void linux binary location
  - ping
    - support void linux binary location
  - php-fpm
    - allow only one pid file ([MR:601](https://gitlab.com/apparmor/apparmor/-/merge_requests/601))
    - fix profile name in signal and change_profile rule ([MR:601](https://gitlab.com/apparmor/apparmor/-/merge_requests/601))
    - profile: adjust /run paths ([MR:601](https://gitlab.com/apparmor/apparmor/-/merge_requests/601))
    - Add php-fpm isolation with apparmor hats ([MR:577](https://gitlab.com/apparmor/apparmor/-/merge_requests/577))
    - allow only one pid file ([MR:617](https://gitlab.com/apparmor/apparmor/-/merge_requests/617),[AABUG:115](https://gitlab.com/apparmor/apparmor/-/issues/115)) 
  - postdrop
    -    - Allow to read icu *.dat files in postfix-related profiles ([MR:546](https://gitlab.com/apparmor/apparmor/-/merge_requests/546))
  - postfix
    - Allow access to dynamicmaps ([MR:284](https://gitlab.com/apparmor/apparmor/-/merge_requests/284))
    - eliminate redundant rules ([MR:284](https://gitlab.com/apparmor/apparmor/-/merge_requests/284))
    - allow reading icu *.dat ([MR:615](https://gitlab.com/apparmor/apparmor/-/merge_requests/615))
  - postmap
    - Allow to read icu *.dat files in postfix-related profiles ([MR:546](https://gitlab.com/apparmor/apparmor/-/merge_requests/546))
  - postqueue
    - Allow to read icu *.dat files in postfix-related profiles ([MR:546](https://gitlab.com/apparmor/apparmor/-/merge_requests/546))
  - sendmail
    - Allow to read icu *.dat files in postfix-related profiles ([MR:546](https://gitlab.com/apparmor/apparmor/-/merge_requests/546))
  - smbd
    - support Arch ([MR:411](https://gitlab.com/apparmor/apparmor/-/merge_requests/411))
  - sshd
    - need write access to Kerberos ticket cache ([MR:153](https://gitlab.com/apparmor/apparmor/-/merge_requests/153))
  - statd
    - updated so that it works on modern env ([MR:153](https://gitlab.com/apparmor/apparmor/-/merge_requests/153))
  - traceroute
    - support void linux binary location
  - useradd
    - support usr-merge ([MR:152](https://gitlab.com/apparmor/apparmor/-/merge_requests/152))
  - windbindd
    - support Arch ([MR:411](https://gitlab.com/apparmor/apparmor/-/merge_requests/411))
  - wireshark ([MR:143](https://gitlab.com/apparmor/apparmor/-/merge_requests/143))
    - allow saving pcaps with optional gzip compression
    - allow creating QT compose cache
    - restrict hidden file creation under ~/.config/
    - drop useless/redundant rules
    - fix access to configuration profiles
    - add a comment for QtProject.conf rules
    - mention that dri rules were backported from abstraction/dri-enumerate
    - incorporate feedback from Talkless an cboltz

## Documentation
  - update bug reporting and documentation to use gitlab
  - apparmor.d
    - update language description with over arching info, eg. declarative
    - update network address family names list [6b2765637e8ed2b8e0e4d1cfae0a9d6b2e955306]
    - add info on profile header ([MR:505](https://gitlab.com/apparmor/apparmor/-/merge_requests/505))
    - add info on kill and unconfined profile modes ([MR:440](https://gitlab.com/apparmor/apparmor/-/merge_requests/440),[AABUG:7](https://gitlab.com/apparmor/apparmor/-/issues/7))
    - add info on xattr attachment conditionals [MR:270](https://gitlab.com/apparmor/apparmor/merge_requests/270)
    - add info on feature abis ([MR:491](https://gitlab.com/apparmor/apparmor/-/merge_requests/491))
    - add info on pinning and feature abi interation ([MR:570](https://gitlab.com/apparmor/apparmor/-/merge_requests/579))
    - fix dbus example
    - fix typos and whitespacing issues
  - apparmor_parser    
    - Add info on disabling jobs ([MR:105]:https://gitlab.com/apparmor/apparmor/merge_requests/105)
    - Add info on config-file
    - Add info on feature abi options (MR:491](https://gitlab.com/apparmor/apparmor/-/merge_requests/491),[579](https://gitlab.com/apparmor/apparmor/-/merge_requests/579))
    - Add info on -Werror and warn flags ([MR:600](https://gitlab.com/apparmor/apparmor/-/merge_requests/600))
  - apparmor
    - Document various debugging options ([debug:826218](https://bugs.debian.org/826218))
  - aa-status
    - document kill, unconfined and mixed modes
  - libapparmor
    - document aa_features_new_from_file() ([MR:491](https://gitlab.com/apparmor/apparmor/-/merge_requests/491))
    - document aa_features_value() ([MR:578]( https://gitlab.com/apparmor/apparmor/-/merge_requests/578))
  - new xattrs man page



## Translations
- sync translation from launchpad


## Tests
- Add tests for aa-notify ([MR:324](https://gitlab.com/apparmor/apparmor/-/merge_requests/324))
- Add some tests for complex profile names ([MR:360](https://gitlab.com/apparmor/apparmor/-/merge_requests/360))
- Add tests for shared aa library functions used in command line scripts ([MR:328](https://gitlab.com/apparmor/apparmor/-/merge_requests/328))
- Add testcases for 'owner link' rules ([MR:369](https://gitlab.com/apparmor/apparmor/-/merge_requests/369))
- fix policy cache test to work correctly with hierarchial caches ([MR:348](https://gitlab.com/apparmor/apparmor/-/merge_requests/348))
-regression tests
  - Add a build-dep target to Makefile to aid in setup
  - fix swap test permission warning ([MR:108](https://gitlab.com/apparmor/apparmor/-/merge_requests/108))
  - Add NO_NEW_PRIVS regression tests ([MR:408](https://gitlab.com/apparmor/apparmor/-/merge_requests/408),[MR:424](https://gitlab.com/apparmor/apparmor/-/merge_requests/424))
  - support building policy cache tests against older versions of libapparmor ([MR:407](https://gitlab.com/apparmor/apparmor/-/merge_requests/407))
  - make LSM stacking aware ([MR:504](https://gitlab.com/apparmor/apparmor/-/merge_requests/504))
  - use --config-file in tests so they are unaffected by the system parser.conf file
  - add FIPS-140-2 lib validation hmac files ([MR:595](https://gitlab.com/apparmor/apparmor/-/merge_requests/595),Bug: [LP:1891664](https://bugs.launchpad.net/bugs/1891664))



# Note

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.
