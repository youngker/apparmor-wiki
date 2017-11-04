Labels, type

In AppArmor the label is a name attached to a subject, proxy and some
time an object. It can be used to identify an object and as part of the
permission computation, and it may or may not be the type. Labels are
often domain types, but object types can have a label (name associated
with them).

The type can be broken down into two catagories

-   named type, an explicit name used directly in permission computations.
-   derived or implied type which is computed from the domain and its rules.

The type is an internal concept in AppArmor used to cache and uniquely
different

```
 subject, object, permission set
```

tuples. So that given a subject and object type the permission set
can be directly looked up in a cache with out having to do a full
rules check. Generally the type can be seen a splitting of the label
into a more fine grained set.

Specifically a profile can be split into different types based on
its permissions.

????
