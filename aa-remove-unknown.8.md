# NAME

aa-remove-unknown - remove unknown AppArmor profiles

# SYNOPSIS

**aa-remove-unknown** \[option\]

# DESCRIPTION

**aa-remove-unknown** will inventory all profiles in /etc/apparmor.d/, compare
that list to the profiles currently loaded into the kernel, and then remove all
of the loaded profiles that were not found in /etc/apparmor.d/. It will also
report the name of each profile that it removes on standard out.

# OPTIONS

- -h, --help

    displays a short usage statement.

- -n

    dry run; only prints the names of profiles that would be removed

# EXAMPLES

    $ sudo ./aa-remove-unknown -n
    Would remove 'test//null-/usr/bin/whoami'
    Would remove 'test'

    $ sudo ./aa-remove-unknown
    Removing 'test//null-/usr/bin/whoami'
    Removing 'test'

# BUGS

None. Please report any you find to Gitlab at
[https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7)
