# Navigation
Return to manpage [Index](ManPages)


# NAME

aa\_features - an opaque object representing a set of AppArmor kernel features

aa\_features\_new - create a new aa\_features object based on a path

aa\_features\_new\_from\_string - create a new aa\_features object based on a string

aa\_features\_new\_from\_kernel - create a new aa\_features object based on the current kernel

aa\_features\_ref - increments the ref count of an aa\_features object

aa\_features\_unref - decrements the ref count and frees the aa\_features object when 0

aa\_features\_write\_to\_file - write a string representation of an aa\_features object to a file

aa\_features\_is\_equal - equality test for two aa\_features objects

aa\_features\_supports - provides aa\_features object support status

aa\_features\_id - provides unique identifier for an aa\_features object

aa\_features\_value - the value associated with a given feature.

# SYNOPSIS

**#include &lt;sys/apparmor.h>**

**typedef struct aa\_features aa\_features;**

**int aa\_features\_new(aa\_features \*\*features, int dirfd, const char \*path);**

**int aa\_features\_new\_from\_file(aa\_features \*\*features, int fd);**

**int aa\_features\_new\_from\_string(aa\_features \*\*features, const char \*string, size\_t size);**

**int aa\_features\_new\_from\_kernel(aa\_features \*\*features);**

**aa\_features \*aa\_features\_ref(aa\_features \*features);**

**void aa\_features\_unref(aa\_features \*features);**

**int aa\_features\_write\_to\_file(aa\_features \*features, int dirfd, const char \*path);**

**bool aa\_features\_is\_equal(aa\_features \*features1, aa\_features \*features2);**

**bool aa\_features\_supports(aa\_features \*features, const char \*str);**

**char \*aa\_features\_id(aa\_features \*features);**

**char \*aa\_features\_value(aa\_features \*features, const char \*str, size\_t \*len);**

Link with **-lapparmor** when compiling.

# DESCRIPTION

The _aa\_features_ object contains information about the AppArmor features
supported by a kernel. The feature support information is based upon the files
AppArmor represents in securityfs, which is typically found at
/sys/kernel/security/apparmor/features/. That information may be parsed and
turned into a string or flat file in order to represent a set of features of a
kernel that is not currently running.

The aa\_features\_new() function creates an _aa\_features_ object based upon a
directory file descriptor and path. The _path_ can point to a file or
directory. See the openat(2) man page for examples of _dirfd_ and _path_. The
allocated _features_ object must be freed using aa\_features\_unref().

The aa\_features\_new\_from\_file() function is similar except that it
accepts an open file as the argument. The allocated _features_ object
must be freed using aa\_features\_unref().

The aa\_features\_new\_from\_string() function is similar except that it accepts a
NUL-terminated string representation of the AppArmor features as the _string_
argument. The length of the features string, not counting the NUL-terminator,
must be specified as the _size_ argument. The allocated _features_ object
must be freed using aa\_features\_unref().

The aa\_features\_new\_from\_kernel() function creates an _aa\_features_ object
from the current running kernel. The allocated _features_ object must be freed
using aa\_features\_unref().

aa\_features\_ref() increments the reference count on the _features_ object.

aa\_features\_unref() decrements the reference count on the _features_ object
and releases all corresponding resources when the reference count reaches zero.

The aa\_features\_write\_to\_file() function writes a string representation of the
_features_ object to the file specified by the _dirfd_ and _path_
combination.

aa\_features\_is\_equal() can be used to detect if the _features1_ and
_features2_ objects are equal. The definition of equality is private to
libapparmor and may be changed in ways that do not break backward
compatibility.

The aa\_features\_supports() function can be used to query the _features_ object
to determine if a feature is supported. The _str_ argument should be equal to
the path, relative to the "apparmor/features/" directory of securityfs, of the
feature to query. For example, to test if policy version 6 is supported, _str_
would be "policy/versions/v6".

The aa\_features\_id() function returns a string representation of an
identifier that can be used to uniquely identify an _aa\_features_ object.
The mechanism for generating the string representation is internal to
libapparmor and subject to change but an example implementation is
applying a hash function to the features string.

# RETURN VALUE

The aa\_features\_new() family of functions return 0 on success and _\*features_
will point to an _aa\_features_ object that must be freed by
aa\_features\_unref(). -1 is returned on error, with errno set appropriately, and
_\*features_ will be set to NULL.

aa\_features\_ref() returns the value of _features_.

aa\_features\_write\_to\_file() returns 0 on success. -1 is returned on error, with
errno set appropriately.

aa\_features\_is\_equal() returns true if _features1_ and _features2_ are equal
and false if they are not equal.

aa\_features\_supports() returns true if the feature represented by _str_ is
supported and false if it is not supported.

aa\_features\_id() returns a string identifying _features_ which must be
freed by the caller. NULL is returned on error, with errno set
appropriately.

aa\_features\_value() returns a null terminated string with is
associated length which must be freed by the caller. NULL is returned
on error, with errno set to ENOENT if the feature was not found,
ENODIR if the specified feature does not resolve to a leaf feature.

# ERRORS

The errno value will be set according to the underlying error in the
_aa\_features_ family of functions that return -1 or NULL on error.

# NOTES

The aa\_features\_id() function can be found in libapparmor version
2.13. All the other aa\_feature functions described above are present
in libapparmor version 2.10.

aa\_features\_unref() saves the value of errno when called and restores errno
before exiting in libapparmor version 2.12 and newer.

# BUGS

None known. If you find any, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

openat(2) and [https://wiki.apparmor.net](https://wiki.apparmor.net).
