# NAME

aa\_getprocattr\_raw, aa\_getprocattr - read and parse procattr data

aa\_getcon, aa\_gettaskcon - get task confinement information

aa\_getpeercon - get the confinement of a socket's other end (peer)

# SYNOPSIS

**#include &lt;sys/apparmor.h>**

**int aa\_getprocattr\_raw(pid\_t tid, const char \*attr, char \*buf, int len, char \*\*mode);**

**int aa\_getprocattr(pid\_t tid, const char \*attr, char \*\*label, char \*\*mode);**

**int aa\_gettaskcon(pid\_t target, char \*\*label, char \*\*mode);**

**int aa\_getcon(char \*\*label, char \*\*mode);**

**int aa\_getpeercon\_raw(int fd, char \*buf, int \*len, char \*\*mode);**

**int aa\_getpeercon(int fd, char \*\*label, char \*\*mode);**

Link with **-lapparmor** when compiling.

# DESCRIPTION

The aa\_getcon function gets the current AppArmor confinement context for the
current task. The confinement context consists of a label and a mode. The label
is usually just the name of the AppArmor profile restricting the task, but it
may include the profile namespace or in some cases a set of profile names
(known as a stack of profiles). The mode is a string that describes how the
kernel is enforcing the policy defined in the profile. Profiles loaded in
"enforce" mode will result in enforcement of the policy defined in the profile
as well as reporting policy violation attempts. Profiles in "complain" mode
will not enforce policy but instead report policy violation attempts.

Some examples of possible returned \*label strings are "unconfined", "/sbin/dhclient",
and "Firefox". The string can consist of any non-NUL characters but it will be
NUL-terminated. The \*label string must be freed using free().

The possible \*mode strings are "enforce" and "complain". Additionally, \*mode may
be NULL when \*label is "unconfined". **The \*mode string must not be freed**. The
\*label and \*mode strings come from a single buffer allocation and are separated
by a NUL character.

The aa\_gettaskcon function is like the aa\_getcon function except it will work
for any arbitrary task in the system.

The aa\_getpeercon function is similar to that of aa\_gettaskcon except that
it returns the confinement information for task on the other end of a socket
connection.

The aa\_getpeercon\_raw function is the backend for the aa\_getpeercon function
and does not handle buffer allocation.

The aa\_getprocattr function is the backend for the aa\_getcon and aa\_gettaskcon
functions and handles the reading and parsing of the confinement data from
different arbitrary attr files and returns the processed results in
an allocated buffer.

The aa\_getprocattr\_raw() is the backend for the aa\_getprocattr function and
does not handle buffer allocation.

# RETURN VALUE

On success size of data placed in the buffer is returned, this includes the
mode if present and any terminating characters. On error, -1 is returned, and
errno(3) is set appropriately.

# ERRORS

- **EINVAL**

    The apparmor kernel module is not loaded or the communication via the
    `/proc/*/attr/file` did not conform to protocol.

- **ENOMEM**

    Insufficient kernel memory was available.

- **EACCES**

    Access to the specified _file/task_ was denied.

- **ENOENT**

    The specified _file/task_ does not exist or is not visible.

- **ERANGE**

    The confinement data is too large to fit in the supplied buffer.

# NOTES

If aa\_getpeercon\_raw returns -1 and errno is ERANGE, the value of \*len can be
used to reallocate buf so that it is sufficiently large enough to store the
confinement data.

# BUGS

None known. If you find any, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7), apparmor.d(5), apparmor\_parser(8), aa\_change\_profile(2),
aa\_splitcon(3) and [https://wiki.apparmor.net](https://wiki.apparmor.net).
