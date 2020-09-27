# NAME

aa\_splitcon - split the confinement context into a label and mode

# SYNOPSIS

**#include &lt;sys/apparmor.h>**

**char \*aa\_splitcon(char \*con, char \*\*mode);**

Link with **-lapparmor** when compiling.

# DESCRIPTION

The aa\_splitcon() function splits a confinement context into separate label
and mode strings. The @con string is modified so that the label portion is NUL
terminated. The enforcement mode is also NUL terminated and the parenthesis
surrounding the mode are removed. If @mode is non-NULL, it will point to the
first character in the enforcement mode string on success.

The Linux kernel's /proc/<PID>/attr/current interface appends a
trailing newline character to AppArmor contexts that are read from that file.
If @con contains a single trailing newline character, it will be stripped by
aa\_splitcon() prior to all other processing.

# RETURN VALUE

Returns a pointer to the first character in the label string. NULL is returned
on error.

# EXAMPLE

    Context                        Label               Mode 
    -----------------------------  ------------------  -------
    unconfined                     unconfined          NULL
    unconfined\n                   unconfined          NULL
    /bin/ping (enforce)            /bin/ping           enforce
    /bin/ping (enforce)\n          /bin/ping           enforce
    /usr/sbin/rsyslogd (complain)  /usr/sbin/rsyslogd  complain

# BUGS

None known. If you find any, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

aa\_getcon(2) and [https://wiki.apparmor.net](https://wiki.apparmor.net).
