# Navigation
Return to manpage [Index}](ManPages)


# NAME

aa-disable - disable an AppArmor security profile

# SYNOPSIS

**aa-disable _&lt;executable>_ \[_&lt;executable>_ ...\] \[_-d /path/to/profiles_\] \[_--no-reload_\] \[_-r_\]**

# OPTIONS

**-d --dir  /path/to/profiles**

    Specifies where to look for the AppArmor security profile set.
    Defaults to /etc/apparmor.d.

**--no-reload**
   Do not unreload the profile after modifying it.

# DESCRIPTION

**aa-disable** is used to _disable_ one or more profiles.
This command will unload the profile from the kernel and prevent the
profile from being loaded on AppArmor startup.
The _aa-enforce_ and _aa-complain_ utilities may be used to to change
this behavior.

# BUGS

If you find any bugs, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7), apparmor.d(5), aa-enforce(1), aa-complain(1),
aa\_change\_hat(2), and [https://wiki.apparmor.net](https://wiki.apparmor.net).
