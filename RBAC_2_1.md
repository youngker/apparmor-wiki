Intro
=====

This document covers using the improvements made in AppArmor 2.1 for
RBAC style access control.

AppArmor 2.1 is shipped in the following distros

-   openSUSE 10.3
-   Ubuntu 7.10 (Gutsy Gibbon)
-   2.1+ - Ubuntu 8.04 (Hardy Heron)

Further documentation concerning using AppArmor for RBAC style access
control can be found at

-   [AppArmor RBAC](http://developer.novell.com/wiki/index.php/Apparmor_RBAC)

Overview of RBAC in AppArmor 2.1
================================

AppArmor 2.1 still can use the earlier techniques for confining
users and providing RBAC style policy. In addition it introduces the
pam\_apparmor which provides a new way to attach policy to users.

pam\_apparmor
=============

The pam\_apparmor module is a
[PAM](http://www.kernel.org/pub/linux/libs/pam/) module that provides a
method of attaching profiles to users without having to edit the user
login information. However because of some limitations in AppArmor
2.1 it may still editing the user login information may still be
required. It is not installed by default and must be installed and
configured before it can be used.

In AppArmor 2.1 pam\_apparmor uses the change\_hat api, which
constrains its use to the following conditions: the application calling
into the PAM api must be confined, and all policy attachments are
done with hats of the confining profile. This means that each service
providing login (gdm, getty, ssh, ...) can be individually configured
as to whether they provide login confinement, or even what confinement
is done by giving each service a different profile. Unfortunately there
is no easy single configuration point for the case where confinement
should be applied equally to all services; in this case the same
confinement needs to be setup in multiple profiles.

The limitations of pam\_apparmor in 2.1 are:

-   PAM aware services must be confined to take advantage of
    pam\_apparmor. An unconfined application calling into PAM means
    that policy transitions do not get applied to the user.

-   only ix, px/Px, and ux/Ux transitions are available. This means
    that applications called from the hat must either have a global
    (px) profile or that the hat must use ix and contain all the
    permissions for applications that will be used. The end result is
    that the hat is the role profile and to work around this hard links
    to applications like the shell can be used in similar manners as
    in 2.0.

-   policy transitions must be specified in profiles using hats. This
    means the subpolicy can't directly call any PAM services because
    hat profiles can not contain hats of their own. Once a user
    is logged in, unless a transition is made to another profile,
    he can not use su, sudo or any other service that calls into
    the PAM stack from the confining hat profile. To get around this
    limition a transition to another profile or the unconfined state
    can be done through either px, or ux.

-   Users are limited to a single login profile so that they can not
    easily change between roles.

-   Accounts that are to used for admin tasks must use uid = 0

-   The assignment of roles to users is done in policy. This means
    for an admin to be able assign a particular role to a given user,
    the admin must have authority to change and reload policy. It is
    not possible to allow a restricted admin to do any role management,
    as the admin could change his own confinement.

Configuring the PAM session
---------------------------

To add pam\_apparmor support to a pam enabled application or service,
add a line like the following to the pam configuration file for the
application (usually stored in /etc/pam.d/):

```
 session  optional       pam_apparmor.so
```

Likely you will want add the pam\_apparmor to other session management
modules. To make it common to all sessions it can be added to the
common-session file. If you make the pam\_apparmor module 'required'
instead of 'optional', the session will abort if pam\_apparmor is
not able to successfully find a profile or hat to change into.

Be careful when making it required; it is possible to cause all
attempted logins to the service to fail if the AppArmor policy is
insufficient.

By default, pam\_apparmor will attempt to change\_hat into a hat based
on the primary group name of the user logging in. If that hat fails to
exist, the module will attempt to change\_hat into a hat named DEFAULT
(it is recommended to ensure this hat exists in the AppArmor profiles
for applications using pam\_apparmor).

However, this is configurable by adding an option to the pam
configuration line to modify what order and what attributes
pam\_apparmor will attempt to use when attempting to change\_hat. To
do so, add 'order=' followed by a comma separated list of types of
hats to try. The type of hats available are:

-   'user' - the username will be used as the hatname
-   'group' - the primary group will be used as the hatname
-   'default' - the string 'DEFAULT' will be used as the hatname. Generally, this should be the hat of last resort.

The order in the list determines the order the hat will be attempted. Some example configurations:

The default behavior:

```
 session  optional       pam_apparmor.so order=group,default
```

Attempt to use only the username:

```
 session  optional       pam_apparmor.so order=user
```

Use the username, followed by the primary groupname, followed by
DEFAULT if the prior hats do not exist in the apparmor profile:

```
 session  optional       pam_apparmor.so order=user,group,default
```

Mapping users to a profile
--------------------------

In pam\_apparmor login application confinement is done through a
combination of confining the login services and setting up hats and
setting the user login shell to a special name using a hard link
alias. It is possible to just use hats to achieve confinement but
this has several disadvantages, and is not recommended.

This is is not significantly different than directly setting up a login
shell alias and accompanying profile except the use of hats allows
for control of confinement on a per service level. If the user is to
be confined when login in through a service then a px transition is
used, causing the shell profile to be attached, if however the user
is not be confined when logging in from a given service then a Ux
transition is used.

It is important to note that if a login service is not confined then
the default profile for the user's login shell will be applied. Thus
an unconfined login service will always confine users that have a
special login shell.

```
 /bin/su {
   #include <abstractions/authentication>
   #include <abstractions/base>
   #include <abstractions/nameservice>   
    capability chown,
   capability setgid,
   capability setuid,    
    owner /etc/environment r,
   owner /home/*/.Xauthority rw,
   owner /home/*/.Xauthority-c w,
   owner /home/*/.Xauthority-l w,
   /{root,home/*}/.xauth* rw,
   owner /proc/sys/kernel/ngroups_max r,
   /usr/bin/xauth rix,
   owner /var/run/utmp rwk,
   
    # tom is a confined user.  The hat contains only the permissions necessary,
   # to transition the tom's login shell.  All other permissions have been
   # moved into the confined_user profile
   ^tom {
     #include <abstractions/authentication>
     #include <abstractions/nameservice>
     
      capability dac_override,
     capability setgid,
     capability setuid,
     
      /etc/default/su r,
     /etc/environment r,
     /home/*/.xauth* w,
     
      /bin/user_bash px,
   }
   
    #don't confine members of wheel who are not specifically confined
   ^wheel {
     #include <abstractions/authentication>
     #include <abstractions/nameservice>
     capability dac_override,
     capability setgid,
     capability setuid,
     
      /etc/default/su r,
     /etc/environment r,
     /home/*/.xauth* w,  
      /bin/user_bash Ux,
   }
 }
```

In the above example profile there are a few important things to note:

-   The hats providing the mapping of a user to confinement profile
    are a stub and don't require the full access that the users login
    shell profile would. However the stub requires several permissions
    (cap setuid) that a confined users shell should not have.

-   That tom will be confined while other users belonging to group
    wheel will not be even though they share the same login shell.

### Using hats for the role profile

There is an alternative technique that is more flexible that setting
up the hat a transition stub. The entire confining profile can be
contained within the hat. This technique has a couple advantages

-   The special hard linked login shell is not required
-   The user can be mapped to different profiles, determined by the login service used

The disadvantages of this technique are that

-   the role profile is a hat so change\_hat can not be used by any of the applications confined by the role profile.
-   the role profile must use a px transition to any login service that uses PAM, this is because pam\_apparmor requires the ability to use change\_hat.
-   the hat must contain all the permissions required by both the
    confining shell and the login service stub profile, and as shown
    above the stub profile needs to contains permissions that in
    general should not be given to a confined user.

A role profile as a hat containing a non-root user, may provide
adequate restrictions as unix DAC permissions will be enforced, some
what negating the effects of giving the profile access to capability
setuid, etc. However this profile provides less value to confined root
users, or if a confined user is able exploit a system flaw allowing
for privilege escalation.

Structure of the role profile
-----------------------------

-??? may be contained above, may be not depends on mapping of hat and role profile

**TODO** explanation

Sharing profile mappings between multiple PAM services
------------------------------------------------------

PAM allows setting up multiple different authentication services
and setting up separate profiles for each service if they are to
have the same user to profile mappings is cumbersome. Fortunately
PAM modules tend to require the same permission sets within the hat
profiles making it possible to make universal hat profiles that are
shared via includes between the profile for the services.

To do this the hats are placed into an include file that is shared
by each service

Selectively confining PAM services
----------------------------------

-   confine login services to get confined users, maybe don't confine some services directly

-   unconfined user to confined user through service profile

-   transitions from confined user to pam service

> should calling the confined service be part of the users profile (generally no)?

> it should transition to a special profile (pam services need elevated privs)

> can use global profile or named transitions

Assigning a user to multiple roles
----------------------------------

Users are limited to a single login profile per service (determined
by profile's hats) so that having a user belong to multiple roles is
not straight forward. Multiple roles can be achieved with either of
the following techniques. The first technique is preferred and should
generally be used instead of the hard link hack technique.

### Providing the user with multiple user names

A user is given multiple accounts with different user names. Depending
on the amount of permission separation between roles, the accounts can
be assigned the same uid or different uids. If the role should still
have access to the users files then the same uid and home directory
should be used. If the role should be limited in access to the users
files then it is better to use a different uid and home directory.

For example, the user tom has a confined user account, and an admin
account that is allowed access to his regular files. The user jane
has a confined user account, and an admin account that is not allowed
access to her regular files.

The /etc/passwd file would look like

```
 tom:x:1000:100:Tom:/home/tom:/bin/bash
 tom_admin:x:1000:100:Tom:/home/tom:/bin/bash
 jane:x:1001:100:Jane:/home/jane:/bin/bash
 jane_admin:x:1002:100:Jane:/home/jane_admin:/bin/bash
```

The profile mapping users to confined roles

```
 /bin/su {
   ...
    ^tom {
      ...
       /bin/user_bash px,
   }
    ^tom_adim {
      ...
       /bin/admin_bash px,
   }
   ^jane {
      ...
       /bin/user_bash px,
   }
    ^jane_adim {
      ...
       /bin/admin_bash px,
   }
 }
```

Notice that the admin\_user profile is used by both tom and jane even
though jane\_admin is not allowed regular access to jane's files. This
can be achieved by through the use of owner rules since jane\_admin has
a different uid than jane. This profile sharing may not be appropriate
in some cases, in which case a custom profile should be used.

### Use hard links to create multiple login binaries

??? Already doin gthis ???

It is possible to use the bash alias hack to create alternative
roles. It still retains all the limitations of the hard link to
login shell and also requires users to remember custom command names,
so it is not the recommended way to achieve multiple roles.

To provide multiple roles a hard link (alias) is created per role type
to each program used to change roles su, sudo, ..), and a different
profile containing the needed transitions is provided per alias. The
user then uses the alias to change role.

For example, for su an admin version created with the name of
su\_admin (ln /bin/su /bin/su\_admin) is created. When a user wants
to change their role, they use /bin/su\_admin instead of /bin/su. The
/bin/su\_admin profile contains the correct mapping of user name to
role profile.

```
 /bin/su_admin {
   ...
    ^tom {
      ...
       /bin/bash px -> admin_user,
   }
 }
```

The original binary (/bin/su in this case) may have its own profile
with its own mappings or may even run unconfined.

If multple admin types are to be used (ie. log admin, full admin,
..) separate aliases and profiles are needed for each.

Setting up transitions within the confined users profile
--------------------------------------------------------

Generally it is recommended that a separate profile be setup
for any application that raises privilege, which is the case for
login services. If a confined user is to use these services the
confining profile should use a px, or named transition to execute the
service. This prevents the raised privileges required by the service
from leaking into the confined users profile.

It is not recommended that hat mappings for a user be put in the
confined users profile, as hats do not provide sufficient separation
of privilege in this use case.

Example pam\_apparmor configurations and policies
-------------------------------------------------

For these examples the following users and their primary groups will be

| user | group|
|:-----|:-----|
| tom  |users |
| jack |users |
| jane |wheel |
| root |wheel |

### Default configuration

pam session configuration line

```
 session  required       pam_apparmor.so
```

profile mapping users to policy

```
 /bin/su {
   ...
    ^users {
     ...
    }
    ^wheel {
     ...
    }
    ^DEFAULT {
     ...
    }
 }
```

### User profiles first

pam session configuration line

```
session  required       pam_apparmor.so order=user,group,default
```

profile mapping users to policy

```
 /foo/su {
   ...
    ^tom {
     ...
    }
    ^users {
     ...
    }
    ^wheel {
     ...
    }
    # If the session is required remove ^DEFAULT to fail logins of users
   # who have not been added to the policy.
   ^DEFAULT {
     #Add the Ux transition when the session is required if users not
     #added to the policy should be unconfined
     /** Ux,
   }
 }
```

Note that the use of the ^DEFAULT hat to unconfine users that have not
been added to policy. This is necessary even if the session type is
optional, if the login service is confined by an AppArmor profile. This
is because when a session type is optional the pam application will
fall back to the confining profile and use its permissions. This
profile may allow sufficient permissions for the user to log in but,
the user will still be confined by it. If the profile does not allow
sufficient permissions the login will fail in unpredictable ways.

confinement profile name is that of login+matching hat name

must be done in policy must be confined must be within a hat root
still must be done with uid 0

configuration

using \#includes to share confinement policy.

use ux on default hat to not confine some users??

-   pam\_apparmor should be the last session type in the session stack because of remaining in hat profile if

> pam_apparmor session succeeds but later session fails

interaction of pam\_apparmor with other techniques, - application
uses pam to change to root and then runs applications

-   if in confined profile can cause change to fail and stay in same profile, and still have change user succeed???

-   provides different transitions dependent on profile confining application calling into the PAM stack

-   combining sudo with other confinement techniques
