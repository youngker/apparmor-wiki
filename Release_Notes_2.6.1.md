AppArmor 2.6.1 Release Notes
============================

AppArmor 2.6.1 is an incremental bug fix release over AppArmor 2.6.0,
that has focused on bug fixing of the userspace code. AppArmor 2.6.1
is purely a userspace release, it leverages the same kernel code as
AppArmor 2.6.0.

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

### AppArmor apache2 module (mod\_apparmor)

-   Fix build time linking issue that prevented mod\_apparmor from
    working ([LP: \#737074](https://launchpad.net/bugs/737074))

### AppArmor Parser

-   Allow the parser to specify more network protocols by fixing the set filtered out at build time ([LP: \#732837](https://launchpad.net/bugs/732837))
-   Fix parser to check its own timestamp against cached profiles, to ensure that on parser upgrades, the caches get regenerated ([LP: \#731184](https://bugs.launchpad.net/bugs/731184))
-   Fix profile matching when an attachement name doesn't contain a regex pattern (e.g. profile chromium-browser /usr/lib/chromium-browser/chromium-browser) ([LP: \#731155](https://bugs.launchpad.net/bugs/731155))
-   Add workaround for older kernels that didn't properly filter out newer network protocols beyond AF\_MAX ([LP: \#727478](https://bugs.launchpad.net/bugs/727478))
-   Fix rc.apparmor.functions breakage ([LP: \#735429](https://launchpad.net/bugs/735429))

### AppArmor Profiles

-   Minor fixups to profiles
-   Fix 'make check' test target to cover the profiles in extras as intended

### AppArmor regression tests

-   Fix simple tcp test and re-enable by default

