# Navigation
Return to manpage [Index}](ManPages)


# NAME

aa-complain - set an AppArmor security profile to _complain_ mode.

# SYNOPSIS

**aa-complain _&lt;executable>_ \[_&lt;executable>_ ...\] \[_-d /path/to/profiles_\] \[_--no-reload_\]**

# OPTIONS

**-d --dir  /path/to/profiles**

    Specifies where to look for the AppArmor security profile set.
    Defaults to /etc/apparmor.d.

**--no-reload**
   Do not reload the profile after modifying it.

# DESCRIPTION

**aa-complain** is used to set the enforcement mode for one or more profiles to _complain_ mode.
In this mode security policy is not enforced but rather access violations
are logged to the system log.

Note that 'deny' rules will be enforced even in complain mode.

# BUGS

If you find any bugs, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7), apparmor.d(5), aa-enforce(1), aa-disable(1),
aa\_change\_hat(2), and [https://wiki.apparmor.net](https://wiki.apparmor.net).
