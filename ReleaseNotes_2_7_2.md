AppArmor 2.7.2 Release Notes
============================

AppArmor 2.7.2 is an incremental bug fix release over AppArmor 2.7.0,
that has focused on bug fixing of the userspace code. AppArmor 2.7.2
is purely a userspace release, it leverages the same kernel code as
AppArmor 2.7.0.

-   For information on other versions please see [AppArmor versions](AppArmor_versions)

Linux kernel compatability
--------------------------

As with prior releases, the AppArmor user space utilities are dependent
on a few bits of kernel functionality that were not accepted by
upstream when the kernel portion of AppArmor was merged into the Linux
kernel. Compatibility patches are included in this user space release.

See [upstream release notes](apparmor/upstream_release_notes)
for more information.

Improvements and Bugs Fixed
---------------------------

For a detailed listing of change see the changelog between tags 2.7
and 2.7.2 in the 2.7 branch

### Utils

- Fix profile autogeneration to include read access to interpreted scripts - autogeneration include appropriate includes for python/ruby scripts

### Profile fixes/updates

- abstractions

   - KDE4
   - p11-kit
   - python
   - aspell
   - enchant
   - fonts
   - ubuntu-media-players
   - ubuntu-integration
   - ubuntu-browsers.d/multimedia
   - ubuntu-bittorrent-clients
   - abstractions/apache2-common

- smbd
- dnsmask
- syslog-ng
- avahi
- cups
- dovecot
- sshd

### Misc

- Create /etc/apparmor.d/tunables/multiarch.d directory
