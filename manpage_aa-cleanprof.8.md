# NAME

aa-cleanprof - clean an existing AppArmor security profile.

# SYNOPSIS

**aa-cleanprof _&lt;executable>_ \[_&lt;executable>_ ...\] \[_-d /path/to/profiles_\] \[_--no-reload\]_ \[_-s_\]**

# OPTIONS

**-d --dir  /path/to/profiles**

    Specifies where to look for the AppArmor security profile set.
    Defaults to /etc/apparmor.d.

**--no-reload**
   Do not reload the profile after modifying it.

**-s --silent**

    Silently overwrites the profile without user prompt.

# DESCRIPTION

**aa-cleanprof** is used to perform a cleanup on one or more profiles.
The tool removes any existing superfluous rules (rules that are covered
under an include or another rule), reorders the rules to group similar rules
together and removes all comments from the file.

# BUGS

If you find any bugs, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7), apparmor.d(5), aa-enforce(1), aa-complain(1), aa-disable(1),
aa\_change\_hat(2), and [https://wiki.apparmor.net](https://wiki.apparmor.net).
