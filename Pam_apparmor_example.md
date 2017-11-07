Example pam\_apparmor configuration
===================================

AppArmor allows you great flexibility in setting up policies
with pam\_apparmor. This example provides a basic pam\_apparmor
configuration using the change\_hat methodolgy (the current
implementation) that can be extended later for your site
requirements. In this example, we confine the 'su' binary such
that when a user uses su to run commands as another user, AppArmor
will apply access controls on the changed to user. This can later
be extended to other binaries such as 'sshd' and 'login'. This
example is known to work on Ubuntu 10.04 LTS with AppArmor 2.5, but
should work fine with AppArmor 2.3 (it is based on techniques from
[RBAC\_2\_3](RBAC_2_3)).

It is recommended while configuring pam\_apparmor that you login to
another terminal as root so that in case something goes wrong with
your configuration, you can use this terminal to make any necessary
changes. Eg:

```
 $ sudo -i
```

or:

```
 $ su -
```

Overview
--------

The idea behind pam\_apparmor is simple: when a someone uses a confined
binary, that binary will transition to an AppArmor role via PAM. So
if 'su' is configured for use with pam\_apparmor, when a user invokes
'su', PAM is consulted. When the PAM session is started, pam\_apparmor
will change\_hat() to either a hat that matches the username, a hat
that matches the primary group, or the DEFAULT hat (which depends on
the order specified in the pam configuration). This hat (typically)
provides a rudimentary policy and declares a transition to a role
profile when the user's shell is started. So with our su example:

1.  The user does 'su - gray'
2.  su then performs a change\_hat() (via pam\_apparmor) to ^gray
3.  When gray's shell is started, the user transitions to the confined\_user profile

Put simply, when the user does 'su - gray', the user changes to the
'confined\_user' role.

Initial configuration
---------------------

Make sure you have pam\_apparmor available on your system. For example,
on Ubuntu:

```
 $ sudo apt-get install libpam-apparmor
```

Next, integrate pam\_apparmor with su by adjusting /etc/pam.d/su
to have:

```
 # omit 'debug' when in production
 session optional     pam_apparmor.so order=user,group,default debug
```

pam\_apparmor policy files
--------------------------

In this example, we will break out the pam\_apparmor policy into
various files to help illustrate the various parts and how they work
together. It also serves as a method to make maintaining your policy
easier. AppArmor doesn't care if the files are broken apart or in one
monolithic file, but we will use the following files in this example:

-   /etc/apparmor.d/pam\_binaries: policy for binaries with profiles (eg 'su')
-   /etc/apparmor.d/pam\_roles: policy for hats referenced in pam/mappings (ie, our 'roles')
-   /etc/apparmor.d/pam/mappings: hats referenced in pam\_binaries

In other words, /etc/apparmor.d/pam\_binaries contains policy
for the binaries that are integrated with pam (in this case
'su'). /etc/apparmor.d/pam\_roles contains the policy for the different
roles at your site, and /etc/apparmor.d/pam/mappings maps login names
to an AppArmor role.

### /etc/apparmor.d/pam\_binaries

Create /etc/apparmor.d/pam\_binaries to have:

```
 #
 # This file contains the policy for the confined binaries that use
 # libpam-apparmor.
 #
 #include <tunables/global>
 /bin/su {
    #include <abstractions/authentication>
    #include <abstractions/base>
    #include <abstractions/nameservice>
    # Include the file with all of our username/group to role mappings
    #include <pam/mappings>
    capability chown,
    capability setgid,
    capability setuid,
    owner /etc/environment r,
    owner /etc/shells r,
    owner /etc/default/locale r,
    owner @{HOMEDIRS}/*/.Xauthority rw,
    owner @{HOMEDIRS}/*/.Xauthority-c w,
    owner @{HOMEDIRS}/*/.Xauthority-l w,
    @{HOME}/.xauth* rw,
    owner @{PROC}/sys/kernel/ngroups_max r,
    /usr/bin/xauth rix,
    owner /var/run/utmp rwk,
 }
```

### /etc/apparmor.d/pam\_roles

Now create different roles in /etc/apparmor.d/pam\_roles:

```
 #
 # This file contains the roles as referenced by pam/mappings
 #
 #include <tunables/global>
 # By default, allow users to read, lock and link to their own files anywhere,
 # but only write to files in their home directory. Only allow limited execution
 # of files.
 profile default_user {
    #include <abstractions/base>
    #include <abstractions/bash>
    #include <abstractions/consoles>
    #include <abstractions/nameservice>
    deny capability sys_ptrace,
    owner /** rkl,
    @{PROC}/** r,
    /bin/**  Pixmr,
    /usr/bin/** Pixmr,
    owner @{HOMEDIRS}/ w,
    owner @{HOMEDIRS}/** w,
 }
 # Allow confined_users to read, write, lock and link to their own files
 # anywhere, and execute from some places.
 profile confined_user {
    #include <abstractions/base>
    #include <abstractions/bash>
    #include <abstractions/consoles>
    #include <abstractions/nameservice>
    deny capability sys_ptrace,
    owner /** rwkl,
    @{PROC}/** r,
    /bin/**  Pixmr,
    /usr/bin/** Pixmr,
    owner @{HOMEDIRS}/bin/** ixmr,
 }
```

### /etc/apparmor.d/pam/mappings

Now create /etc/apparmor.d/pam/mappings to map the usernames/groups to roles:

```
 #
 # This file contains the mappings from users to roles for the binaries
 # confined with AppArmor and configured for use with libpam-apparmor. Users
 # without a mapping will not be able to login.
 #
 # The default hat is a confined user. The hat contains only the permissions
 # necessary to transition to the user's login shell. All other permissions have
 # been moved into the default_user profile.
 ^DEFAULT {
   #include <abstractions/authentication>
   #include <abstractions/nameservice>
   capability dac_override,
   capability setgid,
   capability setuid,
   /etc/default/su r,
   /etc/environment r,
   @{HOMEDIRS}/.xauth* w,
   /bin/{,b,d,rb}ash Px -> default_user,
   /bin/{c,k,tc}sh Px -> default_user,
 }
 # gray is a confined user. The hat contains only the permissions necessary
 # to transition to gray's login shell. All other permissions have been
 # moved into the confined_user profile.
 ^gray {
   #include <abstractions/authentication>
   #include <abstractions/nameservice>
   capability dac_override,
   capability setgid,
   capability setuid,
   /etc/default/su r,
   /etc/environment r,
   @{HOMEDIRS}/.xauth* w,
   /bin/{,b,d,rb}ash Px -> confined_user,
   /bin/{c,k,tc}sh Px -> confined_user,
 }
 # Don't confine members whose primary group is 'admin' who are not specifically
 # confined. Systems without this special primary group may want to define an
 # unconfined 'root' hat in this manner (depending on site policy). 
 ^admin {
   #include <abstractions/authentication>
   #include <abstractions/nameservice>
   capability dac_override,
   capability setgid,
   capability setuid,
   /etc/default/su r,
   /etc/environment r,
   @{HOMEDIRS}/.xauth* w,
   /bin/{,b,d,rb}ash Ux,
   /bin/{c,k,tc}sh Ux,
 }
```

### Applying the policy

After adjusting policy, you must reload the profiles and roles (the
mappings are pulled in automatically):

```
 $ sudo apparmor_parser -r -T -W /etc/apparmor.d/pam_binaries /etc/apparmor.d/pam_roles
```

See if they were loaded:

```
 $ sudo aa-status
 ...
 17 profiles are in enforce mode.
    /bin/su
    /bin/su//admin
    /bin/su//gray
 ...
    confined_user
 ...
```

You may of course update /etc/apparmor.d/pam\_binaries
and /etc/apparmor.d/pam\_roles individually. Changes
to /etc/apparmor.d/pam/mappings require you to reload
/etc/apparmor.d/pam\_binaries (because it has the mappings file as
an \#include).

Extending
---------

Once you are comfortable with your 'su' configuration, you can extend
this to other binaries like sshd and login by:

-   adding a 'session optional pam\_apparmor.so ...' entry to the corresponding pam configuration
-   adding policy for the binary to /etc/apparmor.d/pam\_binaries

To add a new user or role:

-   adjust /etc/apparmor.d/pam\_roles for the new role
-   adjust /etc/apparmor.d/pam/mappings to map the login name to the AppArmor role

Caveats
-------

pam\_apparmor does not consult secondary groups, so using role groups
is currently limited to primary groups only.

Final thoughts
--------------

Much of /etc/apparmor.d/pam/mappings is boiler plate and can be
abstracted out and then added via an \#include. Also remember that
there is nothing special about the pam\_binaries, pam\_roles and
pam/mappings files. You can use other names or include all of the
policy in one file. For ease of understanding and maintenance on
larger sites, breaking the policy up into smaller chunks in this way
can help a lot.
