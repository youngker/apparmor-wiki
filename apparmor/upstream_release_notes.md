The upstream version does not match previous versions of AppArmor as
some changes were required to get AppArmor upstream. This page covers
notes on using the upstream version for each kernel release.

2.6.36 - 2.6.39
===============

The upstream version is missing network mediation and some of the
interfaces for introspection. This means:

-   2.6.36-2.6.39 AppArmor does not mediate network accesses

-   There is a set of compatibility patches that can be applied on top
    of 2.6.36-.39 kernels to reintroduce networking and introspection
    interfaces (see below for more info on patching the kernel),
    so that AppArmor can be used with out the following limitations.

-   To be able to load policy to this version of AppArmor you will
    need at least version 2.6 of the tools.

    -   an updated apparmor\_parser is needed to be able to generate
        and load policy as /sys/kernel/security/apparmor/matching and
        /sys/kernel/security/apparmor/features were used to generate
        the correct policy for the current kernel.

    -   If you are compiling policy with an alternate kernel loaded
        you may need to pass a match (-m flag) string to the compiler
        so that it can generate policy that will load.

    -   Policy generated for the upsteam kernels should generally load
        on non upstream kernels as network rules are not required to
        be in the compiled policy.

    -   Policy generated for a non the none upstream kernels with
        the compatibility patches applied will not work on the
        current upstream kernels unless a match flags is passed to
        the compiler. So that the generated policy does not contain
        network rules.

-   the introspection interfaces that are missing are:

    -   /sys/kernel/security/apparmor/features
    -   /sys/kernel/security/apparmor/matching
    -   /sys/kernel/security/apparmor/profiles

    -   The REMOVE and RESTART portions of the initscripts are
        broken. They used /sys/kernel/security/apparmor/profiles to
        determine what profiles were currently loaded.

	-   REMOVE can be simulated by using either of the following
	    but they will not remove profiles that are not currently
	    in the profiles directory
            -   apparmor\_parser -R /etc/apparmor.d/?\*.\* \# simple assumes all profiles in the profile directory have an embedded.
            -   ?????

	-   REPLACE can be simulated by using either of the following
	    but this will not remove any profile that has been deleted
	    from the profile directory.
            -   apparmor\_parser -r /etc/apparmor.d/?\*.\* \# simple assumes all profiles in the profile directory have an embedded.
            -   ????

    -   aa-status will not work as it uses
        /sys/kernel/security/apparmor/profiles

-   /proc/\<pid\>/attr/\* interfaces are supported so ps -Z based
    introspection will work.

Patching 2.6.36 - 2.6.39
========================

The above limitations can be avoided by patching the kernel, the
patches can be found:

-   in the release tarballs of the userspace tools
-   patches for 2.6.36 http://kernel.org/pub/linux/security/apparmor/AppArmor-2.6/apparmor-v2.6+v2.4-compat-for-2.6.36.patch
-   patches for 2.6.37 - 2.6.38 http://kernel.org/pub/linux/security/apparmor/AppArmor-2.6/apparmor-v2.6+v2.4-compat-for-2.6.37.patch
-   patches for 2.6.39 http://kernel.org/pub/linux/security/apparmor/AppArmor-2.6/apparmor-v2.6+v2.4-compat-for-2.6.39.patch

The specific instructions for patching and building a kernel will
depend on the distro being used. The following instructions apply
for kernels obtained from kernel.org

-   obtain kernel source

    -   Download most recent release tarball from http://www.kernel.org
        and unpack it

        ```
tar -xjvf linux-2.6.XX.XX.tar.bz2
        ```

    -   or obtain git kernel by

        ```
 git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
        ```

-   patch the kernel
    -   ??

-   configure the kernel
    -   make defconfig
    -   make menuconfig
        -   select ???

-   build the kernel
    -   in the base directory of the kernel tree do

        ```
 make
        ```

-   Install the kernel
    ```
 make install
 make modules_install
 depmod -a
    ```

-   Build initrmafs if needed (this is very distro dependant)
    -   For ubuntu

        ```
 sudo update-initramfs -ck <kernelversion>
        ```
