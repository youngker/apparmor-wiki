# Navigation
Return to manpage [Index}](ManPages)


# NAME

aa\_kernel\_interface - an opaque object representing the AppArmor kernel interface for policy loading, replacing, and removing

aa\_kernel\_interface\_new - create a new aa\_kernel\_interface object from an optional path

aa\_kernel\_interface\_ref - increments the ref count of an aa\_kernel\_interface object

aa\_kernel\_interface\_unref - decrements the ref count and frees the aa\_kernel\_interface object when 0

aa\_kernel\_interface\_load\_policy - load a policy from a buffer into the kernel

aa\_kernel\_interface\_load\_policy\_from\_file - load a policy from a file into the kernel

aa\_kernel\_interface\_load\_policy\_from\_fd - load a policy from a file descriptor into the kernel

aa\_kernel\_interface\_replace\_policy - replace a policy in the kernel with a policy from a buffer

aa\_kernel\_interface\_replace\_policy\_from\_file - replace a policy in the kernel with a policy from a file

aa\_kernel\_interface\_replace\_policy\_from\_fd - replace a policy in the kernel with a policy from a file descriptor

aa\_kernel\_interface\_remove\_policy - remove a policy from the kernel

aa\_kernel\_interface\_write\_policy - write a policy to a file descriptor

# SYNOPSIS

**#include &lt;sys/apparmor.h>**

**typedef struct aa\_kernel\_interface aa\_kernel\_interface;**

**int aa\_kernel\_interface\_new(aa\_kernel\_interface \*\*kernel\_interface, aa\_features \*kernel\_features, const char \*apparmorfs);**

**aa\_kernel\_interface \*aa\_kernel\_interface\_ref(aa\_kernel\_interface \*kernel\_interface);**

**void aa\_kernel\_interface\_unref(aa\_kernel\_interface \*kernel\_interface);**

**int aa\_kernel\_interface\_load\_policy(aa\_kernel\_interface \*kernel\_interface, const char \*buffer, size\_t size);**

**int aa\_kernel\_interface\_load\_policy\_from\_file(aa\_kernel\_interface \*kernel\_interface, int dirfd, const char \*path);**

**int aa\_kernel\_interface\_load\_policy\_from\_fd(aa\_kernel\_interface \*kernel\_interface, int fd);**

**int aa\_kernel\_interface\_replace\_policy(aa\_kernel\_interface \*kernel\_interface, const char \*buffer, size\_t size);**

**int aa\_kernel\_interface\_replace\_policy\_from\_file(aa\_kernel\_interface \*kernel\_interface, int dirfd, const char \*path);**

**int aa\_kernel\_interface\_replace\_policy\_from\_fd(aa\_kernel\_interface \*kernel\_interface, int fd);**

**int aa\_kernel\_interface\_remove\_policy(aa\_kernel\_interface \*kernel\_interface, const char \*fqname);**

**int aa\_kernel\_interface\_write\_policy(int fd, const char \*buffer, size\_t size);**

Link with **-lapparmor** when compiling.

# DESCRIPTION

The _aa\_kernel\_interface_ object contains information about the AppArmor
kernel interface for policy loading, replacing, and removing.

The aa\_kernel\_interface\_new() function creates an _aa\_kernel\_interface_ object
based on an optional _aa\_features_ object and an optional path to the apparmor
directory of securityfs, which is typically found at
"/sys/kernel/security/apparmor/". If _kernel\_features_ is NULL, then the
features of the current kernel are used. When specifying a valid
_kernel\_features_ object, it must be compatible with the features of the
currently running kernel. If _apparmorfs_ is NULL, then the default location
is used. The allocated _kernel\_interface_ object must be freed using
aa\_kernel\_interface\_unref().

aa\_kernel\_interface\_ref() increments the reference count on the
_kernel\_interface_ object.

aa\_kernel\_interface\_unref() decrements the reference count on the
_kernel\_interface_ object and releases all corresponding resources when the
reference count reaches zero.

The aa\_kernel\_interface\_load() family of functions load a policy into the
kernel. The operation will fail if a policy of the same name is already loaded.
Use the aa\_kernel\_interface\_replace() family of functions if you wish to
replace a previously loaded policy with a new policy of the same name. The
aa\_kernel\_interface\_replace() functions can also be used to load a policy that
does not correspond to a previously loaded policy.

When loading or replacing from a buffer, the _buffer_ will contain binary
data. The _size_ argument must specify the size of the _buffer_ argument.

When loading or replacing from a file, the _dirfd_ and _path_ combination are
used to specify the location of the file. See the openat(2) man page for
examples of _dirfd_ and _path_.

It is also possible to load or replace from a file descriptor specified by the
_fd_ argument. The file must be open for reading and the file offset must be
set appropriately.

The aa\_kernel\_interface\_remove\_policy() function can be used to unload a
previously loaded policy. The fully qualified policy name must be specified
with the _fqname_ argument. The operation will fail if a policy matching
_fqname_ is not found.

The aa\_kernel\_interface\_write\_policy() function allows for a policy, which is
stored in _buffer_ and consists of _size_ bytes, to be written to a file
descriptor. The _fd_ must be open for writing and the file offset must be set
appropriately.

# RETURN VALUE

The aa\_kernel\_interface\_new() function returns 0 on success and
_\*kernel\_interface_ will point to an _aa\_kernel\_interface_ object that must
be freed by aa\_kernel\_interface\_unref(). -1 is returned on error, with errno
set appropriately, and _\*kernel\_interface_ will be set to NULL.

aa\_kernel\_interface\_ref() returns the value of _kernel\_interface_.

The aa\_kernel\_interface\_load() family of functions, the
aa\_kernel\_interface\_replace() family of functions,
aa\_kernel\_interface\_remove(), and aa\_kernel\_interface\_write\_policy()
return 0 on success. -1 is returned on error, with errno set appropriately.

# ERRORS

The errno value will be set according to the underlying error in the
_aa\_kernel\_interface_ family of functions that return -1 on error.

# NOTES

All aa\_kernel\_interface functions described above are present in libapparmor
version 2.10 and newer.

aa\_kernel\_interface\_unref() saves the value of errno when called and restores
errno before exiting in libapparmor version 2.12 and newer.

# BUGS

None known. If you find any, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

aa\_features(3), openat(2) and [https://wiki.apparmor.net](https://wiki.apparmor.net).
