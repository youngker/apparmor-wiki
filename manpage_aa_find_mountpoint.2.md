# Navigation
Return to manpage [Index](ManPages)


# NAME

aa\_is\_enabled - determine if apparmor is available

aa\_find\_mountpoint - find where the apparmor interface filesystem is mounted

# SYNOPSIS

**#include &lt;sys/apparmor.h>**

**int aa\_is\_enabled(void);**

**int aa\_find\_mountpoint(char \*\*mnt);**

Link with **-lapparmor** when compiling.

# DESCRIPTION

The aa\_is\_enabled function returns true (1) if apparmor is enabled.
If it isn't it sets the errno(3) to reflect the reason it is not
enabled and returns 0.

The aa\_find\_mountpoint function finds where the apparmor filesystem is mounted
on the system, and returns a string containing the mount path.  It is the
caller's responsibility to free(3) the returned path.

# RETURN VALUE

**aa\_is\_enabled**
On success 1 is returned. On error, 0 is returned, and errno(3) is set
appropriately.

**aa\_find\_mountpoint**
On success zero is returned. On error, -1 is returned, and errno(3) is set
appropriately.

# ERRORS

**aa\_is\_enabled**

- **ENOSYS**

    AppArmor extensions to the system are not available.

- **ECANCELED**

    AppArmor is available on the system but has been disabled at boot.

- **ENOENT**

    AppArmor is available (and maybe even enforcing policy) but the interface is
    not available.

- **ENOMEM**

    Insufficient memory was available.

- **EPERM**

    Did not have sufficient permissions to determine if AppArmor is enabled.

- **EACCES**

    Did not have sufficient permissions to determine if AppArmor is enabled.

- **EBUSY**

    AppArmor is enabled but does not have access to shared interfaces, and
    only private interfaces are available.

**aa\_find\_mountpoint**

- **ENOMEM**

    Insufficient memory was available.

- **EACCES**

    Access to the required paths was denied.

- **ENOENT**

    The apparmor filesystem mount could not be found

# BUGS

None known. If you find any, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7), apparmor.d(5), apparmor\_parser(8), and
[https://wiki.apparmor.net](https://wiki.apparmor.net).
