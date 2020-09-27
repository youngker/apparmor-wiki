# NAME

aa\_query\_label - query access permission associated with a label

aa\_query\_file\_path, aa\_query\_file\_path\_len - query access permissions of a file path

aa\_query\_link\_path, aa\_query\_link\_path\_len - query access permissions of a link path

# SYNOPSIS

**#include &lt;sys/apparmor.h>**

**int aa\_query\_label(uint32\_t mask, char \*query, size\_t size, int \*allowed, int \*audited);**

**int aa\_query\_file\_path(uint32\_t mask, const char \*label, size\_t label\_len, const char \*path, int \*allowed, int \*audited);**

**int aa\_query\_file\_path\_len(uint32\_t mask, const char \*label, size\_t label\_len, const char \*path, size\_t path\_len, int \*allowed, int \*audited);**

**int aa\_query\_link\_path(const char \*label, const char \*target, const char \*link, int \*allowed, int \*audited);**

**int aa\_query\_link\_path\_len(const char \*label, size\_t label\_len, const char \*target, size\_t target\_len, const char \*link, size\_t link\_len, int \*allowed, int \*audited);**

Link with **-lapparmor** when compiling.

# DESCRIPTION

The **aa\_query\_label** function fetches the current permissions granted by the
specified _label_ in the _query_ string.

The query is a raw binary formatted query, containing the label and
permission query to make. The returned _allowed_ and _audited_ values are
interpreted boolean values, simply stating whether the query is allowed and
if it is audited.

The mask of the query string is a bit mask of permissions to query and is
class type dependent (see **AA\_CLASS\_xxx** entries in _sys/apparmor.h_).

The format of the query string is also dependent on the **AA\_CLASS** and as
such the **aa\_query\_xxx** helper functions should usually be used instead
of directly using **aa\_query\_label**. If directly using the interface the
_query_ string is required to have a header of **AA\_QUERY\_CMD\_LABEL\_SIZE**
that will be used by **aa\_query\_label**.

The **aa\_query\_file\_path** and **aa\_query\_file\_path\_len** functions are helper
function that assemble a properly formatted file path query for the
**aa\_query\_label** function. The _label_ is a valid apparmor label as
returned by _aa\_splitcon_ with _label\_len_ being the length of the _label_.
The _path_ is any valid filesystem path to query permissions for. For the
**aa\_query\_file\_path\_len** variant the _path\_len_ parameter specifies the
number of bytes in the _path_ to use as part of the query.

The **aa\_query\_link\_path** and **aa\_query\_link\_path\_len** functions are helper
functions that assemble a properly formatted link path query for the
**aa\_query\_label** function. The _link\_len_ and _target\_len_ parameters
specify the number of bytes in the _link_ and _target_ to use as part of
the query.

# RETURN VALUE

On success 0 is returned, and the _allowed_ and _audited_ parameters
contain a boolean value of 0 not allowed/audited or 1 allowed/audited. On
error, -1 is returned, and errno(3) is set appropriately.

# ERRORS

- **EINVAL**

    The requested _mask_ is empty.

    The _size_ of the query is less than the query **AA\_QUERY\_CMD\_LABEL\_SIZE**

    The apparmor kernel module is not loaded or the kernel interface access
    interface is not available

- **ENOMEM**

    Insufficient memory was available.

- **EACCES**

    Access to the specified _label_ or query interface was denied.

- **ENOENT**

    The specified _label_ does not exist or is not visible.

- **ERANGE**

    The confinement data is too large to fit in the supplied buffer.

# NOTES

The label permissions returned are only valid for the time of the
query and can change at any point in the future.

# BUGS

None known. If you find any, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7), apparmor.d(5), apparmor\_parser(8), aa\_getcon(2), aa\_splitcon(3)
and [https://wiki.apparmor.net](https://wiki.apparmor.net).
