# NAME

aa\_policy\_cache - an opaque object representing an AppArmor policy cache

aa\_policy\_cache\_new - create a new aa\_policy\_cache object from a path

aa\_policy\_cache\_ref - increments the ref count of an aa\_policy\_cache object

aa\_policy\_cache\_unref - decrements the ref count and frees the aa\_policy\_cache object when 0

aa\_policy\_cache\_remove - removes all policy cache files under a path

aa\_policy\_cache\_replace\_all - performs a kernel policy replacement of all cached policies

aa\_policy\_cache\_dir\_path - returns the path to the aa\_policy\_cache directory

aa\_policy\_cache\_dir\_path\_preview - returns a preview of the path to the aa\_policy\_cache directory without an existing aa\_policy\_cache object

# SYNOPSIS

**#include &lt;sys/apparmor.h>**

**typedef struct aa\_policy\_cache aa\_policy\_cache;**

**int aa\_policy\_cache\_new(aa\_policy\_cache \*\*policy\_cache, aa\_features \*kernel\_features, int dirfd, const char \*path, uint16\_t max\_caches);**

**int aa\_policy\_cache\_add\_ro\_dir(aa\_policy\_cache \*policy\_cache, int dirfd, const char \*path);**

**aa\_policy\_cache \*aa\_policy\_cache\_ref(aa\_policy\_cache \*policy\_cache);**

**void aa\_policy\_cache\_unref(aa\_policy\_cache \*policy\_cache);**

**int aa\_policy\_cache\_remove(int dirfd, const char \*path);**

**int aa\_policy\_cache\_replace\_all(aa\_policy\_cache \*policy\_cache, aa\_kernel\_interface \*kernel\_interface);**

**char \*aa\_policy\_cache\_dir\_path(aa\_policy\_cache \*policy\_cache, int level);**

**char \*aa\_policy\_cache\_dir\_path\_preview(aa\_features \*kernel\_features, int dirfd, const char \*path);**

Link with **-lapparmor** when compiling.

# DESCRIPTION

The _aa\_policy\_cache_ object contains information about a set of AppArmor
policy cache files. The policy cache files are the binary representation of a
human-readable AppArmor profile. The binary representation is the form that is
loaded into the kernel.

The aa\_policy\_cache\_new() function creates an _aa\_policy\_cache_
object based upon a directory file descriptor and path. See the
openat(2) man page for examples of _dirfd_ and _path_. The _path_
must point to a directory and it will be used as the basis for the
location of policy cache files. See _aa\_policy\_cache\_dir\_path_ to
find out which directory will be used to store the binary policy cache
files. If additional overlay cache directories are used (see
_aa\_policy\_cache\_add\_ro\_dir_) the directory specified in
_aa\_policy\_cache\_new_ is the first directory searched and is the
writable overlay. If _kernel\_features_ is NULL, then the features of
the current kernel are used. When specifying a valid
_kernel\_features_ object, it must be compatible with the features
of the kernel of interest.  The value of _max\_caches_ should be equal
to the number of caches that should be allowed before old caches are
automatically reaped. The definition of what is considered to be an
old cache is private to libapparmor. Specifying 0 means that no new
caches should be created and only existing, valid caches may be used.
Specifying UINT16\_MAX means that a new cache may be created and that
the reaping of old caches is disabled. The allocated
_aa\_policy\_cache_ object must be freed using aa\_policy\_cache\_unref().

The aa\_policy\_cache\_add\_ro\_dir() function adds an existing cache directory
to the policy cache, as a readonly layer under the primary directory
the cache was created with. When the cache is searched for an existing
cache file the primary directory will be searched and then the readonly
directories in the order that they were added to the policy cache.
This allows the policy cache to be seeded with precompiled policy
that can be updated by overlaying the read only cache file with one
written to the primary cache dir.

aa\_policy\_cache\_ref() increments the reference count on the _policy\_cache_
object.

aa\_policy\_cache\_unref() decrements the reference count on the _policy\_cache_
object and releases all corresponding resources when the reference count
reaches zero.

The aa\_policy\_cache\_remove() function deletes all of the policy cache files
based upon a directory file descriptor and path. The _path_ must point to a 
directory. See the openat(2) man page for examples of _dirfd_ and _path_.

The aa\_policy\_cache\_replace\_all() function can be used to perform a policy
replacement of all of the cache policies in the cache directory represented by
the _policy\_cache_ object. If _kernel\_interface_ is NULL, then the current
kernel interface is used. When specifying a valid _kernel\_interface_ object,
it must be the interface of the currently running kernel.

The aa\_policy\_cache\_dir\_path() function provides the path to the cache
directory for a _policy\_cache_ object at _level_ in the policy cache
overlay of cache directories. A _level_ of 0 will always be present
and is the first directory to search in an overlay of cache
directories, and will also be the writable cache directory
layer. Binary policy cache files will be located in the directory
returned by this function.

The aa\_policy\_cache\_dir\_levels() function provides access to the number
of directories that are being overlayed to create the policy cache.

# RETURN VALUE

The aa\_policy\_cache\_new() function returns 0 on success and _\*policy\_cache_
will point to an _aa\_policy\_cache_ object that must be freed by
aa\_policy\_cache\_unref(). -1 is returned on error, with errno set appropriately,
and _\*policy\_cache_ will be set to NULL.

aa\_policy\_cache\_ref() returns the value of _policy\_cache_.

aa\_policy\_cache\_remove() and aa\_policy\_cache\_replace\_all() return 0 on success.
\-1 is returned on error, with errno set appropriately.

aa\_policy\_cache\_dir\_path() returns a path string which must be freed by the
caller. NULL is returned on error, with errno set appropriately.

aa\_policy\_cache\_dir\_levels() returns a number indicating the number of
directory levels there are associated with the _policy\_cache_.

aa\_policy\_cache\_dir\_path\_preview() is the same as
aa\_policy\_cache\_dir\_path() except that it doesn't require an existing
_aa\_policy\_cache_ object. This is useful if the calling program cannot
create an _aa\_policy\_cache_ object due to lack of privileges needed to
create the cache directory.

# ERRORS

The errno value will be set according to the underlying error in the
_aa\_policy\_cache_ family of functions that return -1 or NULL on error.

# NOTES

All aa\_policy\_cache functions described above, except for the
aa\_policy\_cache\_dir\_path() function was added in libapparmor version
2.13. All the other aa\_policy\_cache functions described above are
present in libapparmor version 2.10.

aa\_policy\_cache\_unref() saves the value of errno when called and restores errno
before exiting in libapparmor version 2.12 and newer.

# BUGS

None known. If you find any, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

aa\_features(3), aa\_kernel\_interface(3), openat(2) and
[https://wiki.apparmor.net](https://wiki.apparmor.net).
