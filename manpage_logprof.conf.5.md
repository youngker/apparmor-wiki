# Navigation
Return to manpage [Index](ManPages)


# NAME

logprof.conf - configuration file for expert options that modify the
behavior of the AppArmor aa-logprof(1) program.

# DESCRIPTION

The aa-logprof(1) program can be configured to have certain default behavior
by the contents of logprof.conf.

The **\[qualifiers\]** section lists specific programs that should have
a subset of the full ix/px/ux list when asking what mode to execute
it using.

Since creating a separate profile for /bin/bash is dangerous, we can
specify that for /bin/bash, only (I)nherit, (U)nconstrained, and (D)eny
should be allowed options and only those will show up in the prompt when
we're asking about adding that to a profile.

Likewise, if someone currently exec's /bin/mount in ix or px mode, things
won't work, so we can provide only (U)nconstrained and (D)eny as options.

And certain apps like grep, awk, sed, cp, and mkdir should always
inherit the parent profile rather than having their own profile or
running unconfined, so for them we can specify that only (I)nherit and
(D)eny are the allowed options.

Any programs that are not listed in the qualifiers section get the full
(I)nherit / (P)rofile / (U)nconstrained / (D)eny option set.

If the user is doing something tricky and wants different behavior,
they can tweak or remove the corresponding line in the conf file.

The **\[defaulthat\]** section lists changehat-aware programs and what hat
aa-logprof(1) will collapse the entries to for that program if the user
specifies that the access should be allowed, but should not have it's
own hat.

The **\[globs\]** section allows modification of the logprof rule engine
with respect to globbing suggestions that the user will be prompted with.

The format of each line is-- "&lt;perl glob> = &lt;apparmor glob>".

When aa-logprof(1) asks about a specific path, if the perl glob matches the
path, it replaces the part of the path that matched with the corresponding
apparmor glob and adds it to the list of globbing suggestions.

Lines starting with # are comments and are ignored.

# EXAMPLE

    [qualifiers]
      # things will very likely be painfully broken if bash has it's own profile
      /bin/bash  = iu

      # mount doesn't work if it's confined
      /bin/mount = u

      # these helper utilities should inherit the parent profile and
      # shouldn't have their own profiles
      /bin/awk   = i
      /bin/grep  = i
      /bin/sed   = i

    [defaulthat]
      /usr/sbin/sshd           = EXEC
      /usr/sbin/httpd2         = DEFAULT_URI
      /usr/sbin/httpd2-prefork = DEFAULT_URI

    [globs]
      # /foo/bar/lib/libbaz.so -> /foo/bar/lib/lib*
      /lib/lib[^\/]+so[^\/]*$  = /lib/lib*so*

      # strip kernel version numbers from kernel module accesses
      ^/lib/modules/[^\/]+\/   = /lib/modules/*/

      # strip pid numbers from /proc accesses
      ^/proc/\d+/              = /proc/*/

# BUGS

If you find any bugs, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7), apparmor.d(5), aa-enforce(1), aa-complain(1),
aa-disable(1), aa\_change\_hat(2), aa-logprof(1), aa-genprof(1), and
[https://wiki.apparmor.net](https://wiki.apparmor.net).
