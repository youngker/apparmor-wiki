AppArmor Profiles
=================

Welcome to the AppArmor profiles wiki page!

This page contains resources necessary to begin modifying and
developing your own AppArmor profiles.

Where to get AppArmor profiles
------------------------------

#### Production

Production-ready AppArmor profiles should be packaged by your
distribution, either by adding a specific profile to the application
package itself, or by shipping a package containing all the AppArmor
profiles.

#### Development

Development of new AppArmor profiles is done in [this
repository](https://launchpad.net/apparmor-profiles).  The repository
contains work-in-progress profiles that are intended to be shipped by
distributions once they are deemed to be complete. Anyone is welcome
to contribute to the profiles that are located there, keeping in
mind that they are for general use and need to be compatible with
the default installation of the distribution they will eventually
end up in.  Once a distribution representative has decided that
a profile is ready for production use, it will be added to the
distribution's main packaging. The profile in the repository will
then be replaced with a text file describing where the profile has
been moved to, and the procedure to file bugs against it.  See the
[README file](https://git.launchpad.net/apparmor-profiles/tree/README)
for more information.

#### Example Profiles

The AppArmor user space tools contain some generic example
profiles. These have not been customized for individual distributions,
and are just for example purposes.

How to get the Development AppArmor profiles
--------------------------------------------

The development AppArmor profiles are in a Bazaar repository. Once
you've installed your distro's Bazaar package, you can download them
with the following command:

```
 git clone git://git.launchpad.net/apparmor-profiles
```

Locate the subdirectory that matches your distribution and version, and
look inside for various profiles that are currently in development. You
may use a profile by copying it to /etc/apparmor.d, and restarting
AppArmor:

```
 /etc/init.d/apparmor restart
 restart apparmor     # if upstart is being used, with initscripts that have been fully updated to support upstart
```

How to create or modify AppArmor profiles
-----------------------------------------

See the following wiki pages:

-   [Creating and modifying AppArmor policy with the tools](Profiling_with_tools)
-   [Creating and modifying AppArmor policy by hand](Profiling_by_hand)
-   [AppArmor Documentation](Documentation)

How to contribute AppArmor profiles
-----------------------------------

Patches and new profiles can be contributed by posting them to the
mailing list for review. Please see the [CommitPolicy](CommitPolicy)
and [Versioning](Versioning) before sending patches.

If you are a launchpad user, or wish to join launchpad, launchpad
allows creating custom branches of the apparmor-profiles repository
and you can submit merge requests from your own custom branch (see
[Using Launchpad with AppArmor](launchpadtutorial))
