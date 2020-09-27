# NAME

aa-audit - set an AppArmor security profile to _audit_ mode.

# SYNOPSIS

**aa-audit _&lt;executable>_ \[_&lt;executable>_ ...\] \[_-d /path/to/profiles_\] \[_--no-reload_\] \[_-r_\]**

# OPTIONS

**-d --dir  /path/to/profiles**

    Specifies where to look for the AppArmor security profile set.
    Defaults to /etc/apparmor.d.

**--no-reload**
   Do not reload the profile after modifying it.

**-r --remove**

    Removes the audit mode for the profile.

# DESCRIPTION

**aa-audit** is used to set one or more profiles to audit mode.
In this mode security policy is enforced and all access (successes and failures) are logged to the system log.

The _--remove_ option can be used to remove the audit mode for the profile.

# BUGS

If you find any bugs, please report them at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7), apparmor.d(5), aa-enforce(1), aa-complain(1), aa-disable(1),
aa\_change\_hat(2), and [https://wiki.apparmor.net](https://wiki.apparmor.net).
