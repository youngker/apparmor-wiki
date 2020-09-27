# Navigation
Return to manpage [Index}](ManPages)


# NAME

aa\_stack\_profile, aa\_stack\_onexec - combine multiple profiles to confine a task

# SYNOPSIS

**#include &lt;sys/apparmor.h>**

**int aa\_stack\_profile(const char \*profile);**

**int aa\_stack\_onexec(const char \*profile);**

Link with **-lapparmor** when compiling.

# DESCRIPTION

AppArmor supports stacking two or more profiles when confining a task. The
result is an intersection of all profiles which are stacked. Stacking profiles
together is desirable when wanting to ensure that confinement will never become
more permissive. When changing between two profiles, as performed with
aa\_change\_profile(2), there is always the possibility that the new profile is
more permissive than the old profile but that possibility is eliminated when
using aa\_stack\_profile().

To stack a profile with the current confinement context, a task can use the
aa\_stack\_profile() function. The _profile_ parameter is a NUL-terminated
string indicating a profile name that should be stacked with the current
confinement.

Calling aa\_stack\_profile("profile\_a") while unconfined is equivalent to calling
aa\_change\_profile("profile\_a") since the intersection of unconfined and
"profile\_a" is "profile\_a". Calling aa\_stack\_profile("profile\_b") while
confined by "profile\_a" results in the task's confinement to be the
intersection of "profile\_a" and "profile\_b". The resulting confinement context
will be represented as "profile\_a//&profile\_b" in audit log messages, the
return value of aa\_getcon(2), etc.

Confined programs wanting to use aa\_stack\_profile() need to have rules
permitting stacking the named profile. See apparmor.d(8) for details.

Open file descriptors may not be remediated after a call to aa\_stack\_profile()
so the calling program must close(2) open file descriptors to ensure they
are not available after calling aa\_stack\_profile().

The aa\_stack\_onexec() function is like the aa\_stack\_profile() function
except it specifies that the stacking should take place on the next exec
instead of immediately. The delayed profile change takes precedence over any
exec transition rules within the confining profile. Delaying the stacking
boundary has a couple of advantages, it removes the need for stub transition
profiles and the exec boundary is a natural security layer where potentially
sensitive memory is unmapped.

# RETURN VALUE

On success zero is returned. On error, -1 is returned, and
errno(3) is set appropriately.

# ERRORS

- **EINVAL**

    AppArmor is not loaded, neither a profile nor a namespace was specified,
    or the communication via the `/proc/*/attr/current` file did not conform
    to protocol.

- **ENOMEM**

    Insufficient kernel memory was available.

- **ENOENT**

    The specified profile does not exist, or is not visible from the current
    namespace.

# NOTES

Using aa\_stack\_profile() and related libapparmor functions are the only way to
ensure compatibility between varying kernel versions. However, there may be
some situations where libapparmor is not available and directly interacting
with the AppArmor filesystem is required to stack a profile.

To immediately stack a profile named "profile\_a", as performed with
aa\_stack\_profile("profile\_a"), the equivalent of this shell command can be
used:

    $ echo -n "stackprofile profile_a" > /proc/self/attr/current

To stack a profile named "profile\_a" at the next exec, as performed with
aa\_stack\_onexec("profile\_a"), the equivalent of this shell command can be used:

    $ echo -n "stackexec profile_a" > /proc/self/attr/exec

These raw AppArmor filesystem operations must only be used when using
libapparmor is not a viable option.

# EXAMPLE

The following example shows a simple, if contrived, use of
aa\_stack\_profile().

    #include <stdlib.h>
    #include <string.h>
    #include <sys/apparmor.h>
    #include <sys/types.h>
    #include <sys/stat.h>
    #include <fcntl.h>
    #include <stdio.h>
    #include <unistd.h>

    static void read_passwd()
    {
            int fd;
            char buf[10];

            if ((fd=open("/etc/passwd", O_RDONLY)) < 0) {
                   perror("Failure opening /etc/passwd");
                   _exit(1);
            }

            /* Verify that we can read /etc/passwd */
            memset(&buf, 0, 10);
            if (read(fd, &buf, 10) == -1) {
                    perror("Failure reading /etc/passwd");
                    _exit(1);
            }
            buf[9] = '\0';
            printf("/etc/passwd: %s\n", buf);
            close(fd);
    }

    int main(int argc, char * argv[])
    {
            printf("Before aa_stack_profile():\n");
            read_passwd();

            /* stack the "i_cant_be_trusted_anymore" profile, which
             * should not have read access to /etc/passwd. */
            if (aa_stack_profile("i_cant_be_trusted_anymore") < 0) {
                perror("Failure changing profile -- aborting");
                _exit(1);
            }

            printf("After aa_stack_profile():\n");
            read_passwd();
            _exit(0);
    }

This code example requires a profile similar to the following to be loaded
with apparmor\_parser(8):

    # Confine stack_p to be able to read /etc/passwd and aa_stack_profile()
    # to the 'i_cant_be_trusted_anymore' profile.
    /tmp/stack_p {
        /etc/ld.so.cache          mr,
        /lib/ld-*.so*             mrix,
        /lib/libc*.so*            mr,

        /etc/passwd               r,

        # Needed for aa_stack_profile()
        /usr/lib/libapparmor*.so* mr,
        /proc/[0-9]*/attr/current w,
    }

As well as the profile to stack:

    profile i_cant_be_trusted_anymore {
        /etc/ld.so.cache      mr,
        /lib/ld-*.so*         mrix,
        /lib/libc*.so*        mr,
    }

The output when run:

    $ /tmp/stack_p
    Before aa_stack_profile():
    /etc/passwd: root:x:0:
    After aa_stack_profile():
    Failure opening /etc/passwd: Permission denied
    $

# BUGS

None known. If you find any, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues). Note that using
aa\_stack\_profile(2) without execve(2) provides no memory barriers between
different areas of a program; if address space separation is required, then
separate processes should be used.

# SEE ALSO

apparmor(7), apparmor.d(5), apparmor\_parser(8), aa\_change\_profile(2),
aa\_getcon(2) and [https://wiki.apparmor.net](https://wiki.apparmor.net).
