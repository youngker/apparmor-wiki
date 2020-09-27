# Navigation
Return to manpage [Index}](ManPages)


# NAME

aa-enforce - set an AppArmor security profile to _enforce_ mode from
being disabled or _complain_ mode.

# SYNOPSIS

**aa-enforce _&lt;executable>_ \[_&lt;executable>_ ...\] \[_-d /path/to/profiles_\] \[_--no-reload_\]**

# OPTIONS

**-d --dir / path/to/profiles**

    Specifies where to look for the AppArmor security profile set.
    Defaults to /etc/apparmor.d.

**--no-reload**
   Do not reload the profile after modifying it.

# DESCRIPTION

**aa-enforce** is used to set one or more profiles to _enforce_ mode.
This command is only relevant in conjunction with the _aa-complain_ utility
which sets a profile to complain mode and the _aa-disable_ utility which
unloads and disables a profile.
The default mode for a security policy is enforce and the _aa-complain_
utility must be run to change this behavior.

# BUGS

If you find any bugs, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7), apparmor.d(5), aa-complain(1), aa-disable(1),
aa\_change\_hat(2), and [https://wiki.apparmor.net](https://wiki.apparmor.net).
