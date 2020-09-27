# Navigation
Return to manpage [Index](ManPages)


# NAME

aa\_change\_profile, aa\_change\_onexec - change a tasks profile

# SYNOPSIS

**#include &lt;sys/apparmor.h>**

**int aa\_change\_profile(const char \*profile);**

**int aa\_change\_onexec(const char \*profile);**

Link with **-lapparmor** when compiling.

# DESCRIPTION

An AppArmor profile applies to an executable program; if a portion of
the program needs different access permissions than other portions,
the program can "change profile" to a different profile. To change into a
new profile, it can use the aa\_change\_profile() function to do so. It passes
in a pointer to the _profile_ to transition to. Confined programs wanting to
use aa\_change\_profile() need to have rules permitting changing to the named
profile. See apparmor.d(8) for details.

If a program wants to return out of the current profile to the
original profile, it may use aa\_change\_hat(2). Otherwise, the two profiles must
have rules permitting changing between the two profiles.

Open file descriptors may not be remediated after a call to aa\_change\_profile()
so the calling program must close(2) open file descriptors to ensure they
are not available after calling aa\_change\_profile(). As aa\_change\_profile()
is typically used just before execve(2), you may want to use open(2) or
fcntl(2) with close-on-exec.

The aa\_change\_onexec() function is like the aa\_change\_profile() function
except it specifies that the profile transition should take place on the
next exec instead of immediately.  The delayed profile change takes
precedence over any exec transition rules within the confining profile.
Delaying the profile boundary has a couple of advantages, it removes the
need for stub transition profiles and the exec boundary is a natural security
layer where potentially sensitive memory is unmapped.

# RETURN VALUE

On success zero is returned. On error, -1 is returned, and
errno(3) is set appropriately.

# ERRORS

- **EINVAL**

    The apparmor kernel module is not loaded, neither a profile nor a namespace
    was specified, or the communication via the `/proc/*/attr/current` file did
    not conform to protocol.

- **ENOMEM**

    Insufficient kernel memory was available.

- **EPERM**

    The calling application is confined by apparmor and the no\_new\_privs bit is
    set.

- **EACCES**

    The task does not have sufficient permissions to change its domain.

- **ENOENT**

    The specified profile does not exist, or is not visible from the current
    Namespace.

# EXAMPLE

The following example shows a simple, if contrived, use of
aa\_change\_profile(); a typical use of aa\_change\_profile() will
aa\_change\_profile() just before an execve(2) so that the new
child process is permanently confined.

    #include <stdlib.h>
    #include <string.h>
    #include <sys/apparmor.h>
    #include <sys/types.h>
    #include <sys/stat.h>
    #include <fcntl.h>
    #include <stdio.h>
    #include <unistd.h>

    int main(int argc, char * argv[])
    {
            int fd;
            char buf[10];
            char *execve_args[4];

            printf("Before aa_change_profile():\n");
            if ((fd=open("/etc/passwd", O_RDONLY)) < 0) {
                   perror("Failure opening /etc/passwd");
                   return 1;
            }

            /* Confirm for ourselves that we can really read /etc/passwd */
            memset(&buf, 0, 10);
            if (read(fd, &buf, 10) == -1) {
                    perror("Failure reading /etc/passwd");
                    return 1;
            }
            buf[9] = '\0';
            printf("/etc/passwd: %s\n", buf);
            close(fd);

            printf("After aa_change_profile():\n");

            /* change profile to the "i_cant_be_trusted_anymore" profile, which
             * should not have read access to /etc/passwd. */
            if (aa_change_profile("i_cant_be_trusted_anymore") < 0) {
                perror("Failure changing profile -- aborting");
                _exit(1);
            }

            /* confirm that we cannot read /etc/passwd */
            execve_args[0] = "/usr/bin/head";
            execve_args[1] = "-1";
            execve_args[2] = "/etc/passwd";
            execve_args[3] = NULL;
            execve("/usr/bin/head", execve_args, NULL);
            perror("execve");
            _exit(1);
    }

This code example requires a profile similar to the following to be loaded
with apparmor\_parser(8):

    profile i_cant_be_trusted_anymore {
        /etc/ld.so.cache      mr,
        /lib/ld-*.so*         mrix,
        /lib/libc*.so*        mr,

        /usr/bin/head ix,
    }

The output when run:

    $ /tmp/change_p
    Before aa_change_profile():
    /etc/passwd: root:x:0:
    After aa_change_profile():
    /usr/bin/head: cannot open `/etc/passwd' for reading: Permission denied
    $

If /tmp/change\_p is to be confined as well, then the following profile can be
used (in addition to the one for 'i\_cant\_be\_trusted\_anymore', above):

    # Confine change_p to be able to read /etc/passwd and aa_change_profile()
    # to the 'i_cant_be_trusted_anymore' profile.
    /tmp/change_p {
        /etc/ld.so.cache          mr,
        /lib/ld-*.so*             mrix,
        /lib/libc*.so*            mr,

        /etc/passwd               r,

        # Needed for aa_change_profile()
        /usr/lib/libapparmor*.so* mr,
        /proc/[0-9]*/attr/current w,
        change_profile -> i_cant_be_trusted_anymore,
    }

# BUGS

None known. If you find any, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues). Note that using
aa\_change\_profile(2) without execve(2) provides no memory barriers between
different areas of a program; if address space separation is required, then
separate processes should be used.

# SEE ALSO

apparmor(7), apparmor.d(5), apparmor\_parser(8), aa\_change\_hat(2) and
[https://wiki.apparmor.net](https://wiki.apparmor.net).
