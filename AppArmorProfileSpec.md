Requirements
============

-   AppArmor 2.3: namespaces

<!-- -->

-   AppArmor 3.5: stacking AppArmor

<!-- -->

-   AppArmor 4: delegation, variants, subtypes, instances, aliases

Profile names
=============

Profile names can take on two forms, a name that defines an attachment
(file name) or a more restricted non-attaching profile name.

Profile names that define an attachment can have the following form

```
 /[^\x00/]?(/?[^\x00/])*
```

Profiles names that are non-attaching have the following form

```
 [a-zA-Z0-9]([a-zA-Z0-9+._~-])*
```

Profile names must begin with an alphanumeric character or the **/**
character

Profile names do not end with **/** character

// - profile and namespace hierarchy seperator

```
 profile_A//child_1
 :ns1://profile_A
 :ns1//ns2://profile_B
```

Special Prefixes
================

*null-* reserved for the special dynamically created learning profiles

```
 null-1234
 null-/usr/bin/firefox
```

*:* - namespace

```
 :ns1:
 :ns1:profile       # ssh style
 :ns1://profile     # url style
```

= - abs root prefix. Indicates the label is from absolute root based
view, used internally for secids.

```
 =profile_A
```

& - Stacking

```
 profile_A//&profile_B
 profile_A//&profile_A//child_1
 profile_A//&:ns1://profile_B
```

+ - Delegation

```
 profile_A//+delegate_1
 profile_A//+delegate_1//&profile_B//+delegate_1
```

~ - profile variant (user name or other conditional match)

```
 profile_A//~jj
 profile_A//~jj//child_1
```

`#` - instance == dynamic variant created by change\_instance, where
the trailing \# is unique. This will only ever show up as the last
element of a label.

instance of profile\_A

```
 profile_A//#1
```

instance of profile\_A//&profile\_B

```
 profile_A//&profile_B//#2
```

`*` - subtype of a profile, where the number is unique to the
permission division. Subtypes only show up in specific queries that
request them. They are not meant for regular user space consumption
but are made available to user space that libapparmor can provide a
permission lookup cache.

```
 profile_A//*1
```

The subtype is a per profile value so that \*1 in conjunction with
profile A is different than \*1 with profile B. The subtype will
always show up as part of the profile it is associated with.

The subtype of a child is expressed as

```
 profile_A//child_B//*1
```

Subtypes in a stack

```
 profile_A//*1//&profile_B//*1
 profile_A//*1//&profile_B//*2
```

Aliases
=======

AppArmor 4 allows for Aliases to be used for profiles, stacks and
delegation, allowing for shortened names to be used. Aliases are
declared at the profile level, or when in a profile for its children,
using the alias command

```
'alias' <name>'='<name>','
```

E.G.

```
alias shorty=A//&B//+C,
```

It is important to note that an alias is only guaranteed to be used in
the case of an exact label match. That is given the label *A//&B//+C*
the name *shorty* will be used. However given another label even
if the label contains the aliased label the alias may or may not be
used. Whether it will be used is an implementation issue, with the
general rule being if an alias is a perfect subset of a label name
string, with no overlap with other aliases it might be used.

Example 1
---------

```
alias shorty=A//&B//+C,
label A//&B//+C//&D,
```

reported label `shorty//&D`

Example 2
---------

```
alias shorty=A//&C//+D,
label A//&B//&C//+D,
```

reported label `A//&B//&C//+D`. The B in the text string keeps the
alias from being an exact match so the alias will likely not be used.

Note: an implementation is free to use the alias in Eg 2. Which
would result in the reported label *shorty//&B*. The reason it is
not guaranteed is because there is a runtime cost to compute whether
an alias should be used, and with potentially overlapping aliases
finding consistent minimal label names is difficult and costly.

Aliases with in policy
----------------------

Even though an alias may not be reported by the AppArmor kernel
module for a given label, policy is free to specify it as long as it
is declared in the policy that is being loaded together as a single
unit. This ensures that the kernel knows of the alias and can parse
it correctly. So using Eg. 2 even if the kernel does not report
*shorty//&B* the policy author is free to use it.

Aliases may even be used in declaring other aliases as long as the expand results in a label name that contains only profiles.

Example 3
---------

```
alias shorty=A//&B//+C,
alias foo=shorty&//D,
```

Aliases declared as such will be reported from the kernel based on matching of the expanded form.

Aliasing profile names
----------------------

Aliases for profile names are not supported at this time. If desired or needed it is recommend shortend profile names be used along with the separated attachment specification.

For example, instead of`

```
profile /usr/lib/firefox/firefox{,*[^s][^h]} { ... }
```

use
```
profile firefox /usr/lib/firefox/firefox{,*[^s][^h]} { ... }
```
