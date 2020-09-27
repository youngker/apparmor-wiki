# Navigation
Return to manpage [Index}](ManPages)


# NAME

aa\_change\_hat  - change to or from a "hat" within a AppArmor profile

# SYNOPSIS

**#include &lt;sys/apparmor.h>**

**int aa\_change\_hat (char \*subprofile, unsigned long magic\_token);**

**int aa\_change\_hatv (char \*subprofiles\[\], unsigned long magic\_token);**

**int aa\_change\_hat\_vargs (unsigned long magic\_token, ...);**

Link with **-lapparmor** when compiling.

# DESCRIPTION

An AppArmor profile applies to an executable program; if a portion of
the program needs different access permissions than other portions,
the program can "change hats" to a different role, also known as a
subprofile.

To change into a new hat, it calls one of the family of change\_hat
functions to do so. It passes in a pointer to the _subprofile_ which it
wants to change into, and a 64bit _magic\_token_.  The _magic\_token_
is used to return out of the subprofile at a later time.

The aa\_change\_hat() function allows specifying the name of a single
_subprofile_ that the application wants to change into.  A pointer to the
name of the _subprofile_ is passed along with the _magic\_token_.  If the
profile is not present the call will fail with the appropriate error.

The aa\_change\_hatv() function allows passing a _NULL_ terminated vector
of pointers to _subprofile_ names which will be tried in order.  The
first _subprofile_ in the vector that exists will be transitioned to
and if none of the _subprofiles_ exist the call will fail with the
appropriate error.

The aa\_change\_hat\_vargs() function is a convenience wrapper for the
aa\_change\_hatv() function.  After the _magic\_token_ it takes an arbitrary
number of pointers to _subprofile_ names.  Similar to execl(3),
aa\_change\_hat\_vargs() assembles the list of _subprofile_ names into a
vector and calls aa\_change\_hatv().

If a program wants to return out of the current subprofile to the
original profile, it calls aa\_change\_hat() with a pointer to NULL as
the _subprofile_, and the original _magic\_token_ value. If the
_magic\_token_ does not match the original _magic\_token_ passed into the
kernel when the program entered the subprofile, the change back to the
original profile will not happen, and the current task will be killed.
If the _magic\_token_ matches the original token, then the process will
change back to the original profile.

As both read(2) and write(2) are mediated, a file must be listed in a
subprofile definition if the file is to be accessed while the process
is in a "hat".

# RETURN VALUE

On success zero is returned. On error, -1 is returned, and
errno(3) is set appropriately.

# ERRORS

- **EINVAL**

    The apparmor kernel module is not loaded or the communication via the
    `/proc/*/attr/current` file did not conform to protocol.

- **ENOMEM**

    Insufficient kernel memory was available.

- **EPERM**

    The calling application is not confined by apparmor, the specified
    _subprofile_ is not a _hat profile_, the task is being ptraced and the
    tracing task does not have permission to trace the specified _subprofile_ or the no\_new\_privs execution bit is
    enabled.

- **ECHILD**

    The application's profile has no hats defined for it.

- **ENOENT**

    The specified _subprofile_ does not exist in this profile but other hats
    are defined.

- **EACCES**

    The specified magic token did not match, and permissions to change to
    the specified _subprofile_ has been denied. This will in most situations
    also result in the task being killed, to prevent brute force attacks.

# EXAMPLE

The following code examples shows simple, if contrived, uses of
aa\_change\_hat(); a typical use of aa\_change\_hat() will separate
privileged portions of a process from unprivileged portions of a process,
such as keeping unauthenticated network traffic handling separate
from authenticated network traffic handling in OpenSSH or executing
user-supplied CGI scripts in apache.

The use of random(3) is simply illustrative. Use of `/dev/urandom` is
recommended.

First, a simple high-level overview of aa\_change\_hat() use:

    void foo (void) {
           unsigned long magic_token;

           /* get a random magic token value
           from our huge entropy pool */
           magic_token = random_function();

           /* change into the subprofile while
            * we do stuff we don't trust */
           aa_change_hat("stuff_we_dont_trust", magic_token);

           /* Go do stuff we don't trust -- this is all
            * done in *this* process space, no separate
            * fork()/exec()'s are done. */
           interpret_perl_stuff(stuff_from_user);

           /* now change back to our original profile */
           aa_change_hat(NULL, magic_token);
    }

Second, an example to show that files not listed in a subprofile ("hat")
aren't accessible after an aa\_change\_hat() call:

    #include <stdlib.h>
    #include <string.h>
    #include <sys/apparmor.h>
    #include <sys/types.h>
    #include <sys/stat.h>
    #include <fcntl.h>
    #include <stdio.h>
    #include <unistd.h>


    int main(int argc, char *argv[]) {
           int fd;
           unsigned long tok;
           char buf[10];

           /* random() is a poor choice */
           tok = random();

           /* open /etc/passwd outside of any hat */
           if ((fd=open("/etc/passwd", O_RDONLY)) < 0)
                   perror("Failure opening /etc/passwd");

           /* confirm for ourselves that we can really read /etc/passwd */
           memset(&buf, 0, 10);
           if (read(fd, &buf, 10) == -1) {
                   perror("Failure reading /etc/passwd pre-hat");
                   _exit(1);
           }
           buf[9] = '\0';
           printf("/etc/passwd: %s\n", buf);

           /* change hat to the "hat" subprofile, which should not have
            * read access to /etc/passwd -- even though we have a valid
            * file descriptor at the time of the aa_change_hat() call. */
           if (aa_change_hat("hat", tok)) {
                   perror("Failure changing hat -- aborting");
                   _exit(1);
           }

           /* confirm that we cannot read /etc/passwd */
           lseek(fd,0,SEEK_SET);
           memset(&buf, 0, 10);
           if (read(fd, &buf, 10) == -1)
                   perror("Failure reading /etc/passwd post-hat");
           buf[9] = '\0';
           printf("/etc/passwd: %s\n", buf);

           return 0;
    }

This code example requires the following profile to be loaded with
apparmor\_parser(8):

    /tmp/ch {
      /etc/ld.so.cache               mr,
      /etc/locale/**                 r,
      /etc/localtime                 r,
      /usr/share/locale/**           r,
      /usr/share/zoneinfo/**         r,
      /usr/lib/locale/**             mr,
      /usr/lib/gconv/*.so            mr,
      /usr/lib/gconv/gconv-modules*  mr,

      /lib/ld-*.so*         mrix,
      /lib/libc*.so*        mr,
      /lib/libapparmor*.so* mr,
      /dev/pts/*            rw,
      /tmp/ch               mr,

      /etc/passwd           r,

      ^hat {
        /dev/pts/*     rw,
      }
    }

The output when run:

    $ /tmp/ch
    /etc/passwd: root:x:0:
    Failure reading /etc/passwd post-hat: Permission denied
    /etc/passwd:
    $

# BUGS

None known. If you find any, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues). Note that
aa\_change\_hat(2) provides no memory barriers between different areas of a
program; if address space separation is required, then separate processes
should be used.

# SEE ALSO

apparmor(7), apparmor.d(5), apparmor\_parser(8), aa\_change\_profile(2),
aa\_getcon(2) and
[https://wiki.apparmor.net](https://wiki.apparmor.net).
