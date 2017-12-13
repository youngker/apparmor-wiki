AppArmor 2.8.1 Release Notes
============================

AppArmor 2.8.1 is an incremental bug fix release over AppArmor 2.8.0
that is focused on bug fixing userspace code.

Linux kernel compatability
--------------------------

As with prior releases, the AppArmor user space utilities are dependent
on a few bits of kernel functionality that were not accepted by
upstream when the kernel portion of AppArmor was merged into the
Linux kernel. Compatibility patches are included in this user space
release through kernel 3.6.

See [upstream release notes](Apparmor/upstream_release_notes) for more information.

Detailed changes
----------------

The following list of changes was taken from bzr log.

-  Subject: profiles - adjust pulseaudio in abstraction (ixr -> Pixr) *(revno: 2067)*

-  The following patch extends the libraries log parsing to support
   more date time formats. As this is causing failures on some systems
   *(revno: 2066)*

-  Subject: add aa-decode test script *(revno: 2065)*

-  Backported merge of various fixes from trunk in preparation of
   the 2.8.1 release *(revno: 2064)*. In this merge are the following
   trunk commits:

   -   2050 - parser - network rules debugging statements
   -   2057 - update ubuntu-browsers.d/java for IcedTea 7
   -   2058 - let sanitized-helper also allow access to /usr/local
   -   2059 - ubuntu-integration does not work properly with exo-open
   -   2062 - support alternate ping install location in /usr
   -   2064 - parser - update apparmor\_parser man page
   -   2065 - parser - correct apparmor\_parser -N command privilege
   -   2066 - parser - *just* the updated caching test message from this commit
   -   2065.1.1 - profiles update fonts abstraction for new fontconfig paths
   -   2065.1.{3,4} - profiles - Gnome applications are now quite interested in reading /usr/share/poppler/cMap/\*\*
   -   2069 - profiles - update extras README with mail list info
   -   2074 - tests - fix clone test on arm
   -   2076 - parser tests - fix test driver for exec() failure
   -   2079 - libapparmor - add pkgconfig support
   -   2083 - parser tests - fix fine grained timestamp detection in caching tests
   -   2090 - nvidia abstractions cleanups
   -   2092 - update skype profile
   -   2093 - add XCompose to abstractions/X
   -   2096 - dnsmasq network-manager integration

-  fix aa-decode by backporting all changes from trunk to 2.8 branch *(revno: 2063)*:
   -   speed up aa-decode by using a bash regex matching instead of calling egrep for each line.
   -   Fix aa-decode handling of stdin
   -   fix error handling in aa-decode

-  Add a small sleep call to the onexec test to give the forked
   process a chance to run before verifying it's current and future
   confinement state. In testing the combined sleeps added roughly a
   second to onexec.sh's total time on relatively reasonable hardware.
   *(revno: 2062)*

-  add CAP\_BLOCK\_SUSPEND to severity.db *(revno: 2061)*

-  Add a testcase for the issue fixed in commit 2059.  *(revno: 2060)*

-  fix a nasty little bug that can surface in apparmor 2.8 when
   Hats/children profiles are used.  *(revno: 2059)*

   the matchflags in the dfa backend are not getting properly reset, which
   results in a previously processed profiles match flags being used. This is
   not a problem for most permissions but can result in x conflict errors.

   Note: this should not result in profiles with the wrong x
   transitions loaded as it causes compilation to file with an
   x conflict.

-  Add kernel patches for 3.5 and 3.6 kernels *(revno: 2058)*

-  Update documentation of change\_hat and change\_profile apis *(revno: 2057)*

-  So the library version has not been being correctly bumped.
   Make this a little bit easier to follow *(revno: 2056)*

-  The apparmor coredump regression test was broken. *(revno: 2055)*

   -  It failed to remove coredump files named “core”
   -  It failed to properly detect “core.\<pid\>” files
   -  And it would fail if the coredump\_pattern had been modified
      to a different location.

   This lead one of the tests to report it was passing when it
   wasn't because it was detecting the previous tests core file.

   -  Fix the test to set the coredump\_pattern, to dump into the
      tmpdir used for the test.
   -  Make it so it will only detect the core file for the pid of
      the last test run.
   -  And extend the test to have a couple of extra test cases.

-  apparmor: add clearing the profile cache when inconsistent *(revno: 2054)*

-  ls moved from /bin/ to /usr/bin/ on openSUSE (usrMove) *(revno: 2053)*

-  The previous patch to fix policy compilation around the network
   flag had a serious flaw. The test for the network flag was being
   applied against both the kernel flags and the cache flags. This
   means that if either the kernel or the cache did not have the flag
   set then network mediation would be turned off.

   Thus if a kernel was booted without the flag, and a cache was
   generated based on that kernel and then the system was rebooted into
   a kernel with the network flag present, the parser on generating
   the new policy would detect the old cache did not support network
   and turn it off for the new policy as well.

   This can be fixed by either removing the old cache first or
   regenerating the cache twice. As the first generation will write
   that networking is supported in the cache (even though the policy
   will have it disabled), and the second generation will generate
   the correct policy.

   The following patch moves the test so that it is only applied to
   the kernel flags set. *(revno: 2052)*

-  Fix the parser so it checks for the presence of the network feature in the
   compatibility interface. Previously it was assuming that if the compatibility
   interface was present that network rules where also present, this is not
   necessarily true and causes apparmor to break when only the compatibility
   patch is applied.  *(revno: 2051)*

-  Have build check for presence of awk and fail with a sensible
   error message *(revno: 2050)*

-  create-apparmor.vim.py was failing on systems with python 2.5,
   fix that *(revno: 2049)*
