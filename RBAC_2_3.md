Intro
=====

This document covers using the improvements made in AppArmor 2.3 for
RBAC style access control.

AppArmor 2.3 is shipped in the following distros

-   openSUSE 11
-   openSUSE 11.1
-   SUSE Linux Enterprise (Server, Desktop) 11
-   Ubuntu 8.10 (Intrepid Ibex)

Further documentation concerning using AppArmor for RBAC style access
control can be found at

-   [AppArmor RBAC](http://developer.novell.com/wiki/index.php/Apparmor_RBAC)
-   [RBAC in AppArmor 2.1](http://developer.novell.com/wiki/index.php/Apparmor_RBAC_in_version_2.1)

Overview of RBAC in AppArmor 2.3
================================

AppArmor 2.3 introduced several
[improvements](http://developer.novell.com/wiki/index.php/Apparmor_2_3)
that affect authoring RBAC style policy.

-   Admin profiles can allow mount/umount as long as they contain
    capability sys\_admin.

-   Role based profiles can have descriptive names, instead of using
    an executable file, or alias name.

-   Named transitions can be used instead of hard links to confine
    a single executable with different profiles dependent on the
    situation.

-   Better separation of users sharing a role profile through owner
    conditional rules.

-   The cx and named profiles transitions can be used to work around
    the limitations of the hat profile.

    In particular a named transition for the login shell can be used
    to transition to a role profile, which can use change\_hat thus
    allowing access to PAM based applications.

Remaining limitations:

-   AppArmor can only control mounts at an all or nothing level they
    can not control where mounts occur, or what is being mounted.

-   AppArmor has only very course networking controls.

-   AppArmor ipc control is very limited, so much so that some ipc
    mechanism are not currently controlled.

Setting up restricted admin profiles
====================================

Instead of giving admin users the [root uid (uid == 0)](http://developer.novell.com/wiki/index.php/Apparmor_RBAC),
and setting up a hard link to attach a profile, it is recommended that
pam\_cap.so and pam\_apparmor.so are used together, where pam\_cap
is used to raise a user's capabilities and pam\_apparmor is used to
set an initial profile on the user.

This has several advantages over the previous technique

-   The uid of the user does not need to be modified in the password database
-   The uid of admin users is not shared so that log messages can be disambiguated
-   It is possible to separate admin file access using owner rules.

There are also some disadvantages that must be considered

-   Some applications expect a uid == 0 for root privileges

foo
===

update other techniques - basic confined shell - knightshell - sudo profile, - named transitions

pam\_apparmor
=============

AppArmor 2.3 removes, or provides workarounds for many of the
limitations that pam\_apparmor had in 2.1. In particular

-   the new transition modes (cx, named transitions) allow for the
    hat profile to be left for a generic profile. This removes the
    need to use hard link aliases as means of applying profiles to
    applications that shouldn't have a global profile (eg. bash).

    Also the use of the new transitions to leave the hat profiles,
    allows breaking the limitation of the confined user calling back
    into the PAM stack through su or sudo.

In 2.3 pam\_apparmor which uses change\_hat retains the following
limitations:

-   PAM aware services must be confined to take advantage of
    pam\_apparmor.

-   Policy transitions must be specified in profiles using hats. This
    means the subpolicy can't directly call any PAM services because
    hat profiles can not contain hats of their own. Once a user is
    logged in he can not use su, sudo or any other service that calls
    into the PAM stack from the confining hat profile. To get around
    this limitation a transition to another profile or the unconfined
    state can be done through either px, or ux.

-   Users are limited to a single login profile per service (determined
    by profile's hats) so that having a user belong to multiple roles
    is not straight forward. Multiple roles can be achieved with the
    techniques described in the Multiple roles section.

-   The assignment of roles to users is done in policy. This means
    for an admin to be able assign a particular role to a given user,
    the admin must have authority to change and reload policy. It is
    not possible to allow a restricted admin to do any role management,
    as the admin could change his own confinement.

-   When searching for a hat by group, only the primary group is
    checked, not supplementary groups

Mapping the user to a profile
-----------------------------

The basic configuration to map users to roles remains the same as in
AppArmor 2.1, however the new abilities of 2.3 allow for improvements
in the actual policy. As in AppArmor 2.1 the login application must be
confined and the mapping of users to policy is done through hats. Even
the hat profile is structured the same as what is recommended for
AppArmor 2.1 (a transition stub) except a named transition is used
instead of setting up an alias (hard linked shell) and changing the
users default shell to the alias.

Reworking the /bin/su example profile from AppArmor 2.1 using AppArmor
2.3 we get

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

     /bin/bash px -> confined_user,
   }

   # don't confine members of wheel who are not specifically confined
   ^wheel {
     #include <abstractions/authentication>
     #include <abstractions/nameservice>

     /etc/default/su r,
     /etc/environment r,
     /home/*/.xauth* w,
     /bin/bash Ux,
   }
 }
```

Structure of the role profile
-----------------------------

The role profile (in this case confined\_user) in AppArmor 2.3 is
largely the same as that in 2.1, except its name is not required
to map to an executable. This allows the role profile to have a
descriptive name and be applied many different executables (different
login shells).

The confined user profile, has been made more generic than the
AppArmor 2.1 profile, by leveraging owner conditional rules. The
owner conditional link has an added benefit of enforcing
[Openwall](http://www.openwall.com/linux/README.shtml) style hard
link restrictions.

```
 profile confined_user {
   #include <abstractions/base>
   #include <abstractions/bash>
   #include <abstractions/consoles>
   #include <abstractions/nameservice>
   deny capability sys_ptrace,

   # allow users to read, write, lock and link to their own files anywhere
   owner /** rwkl,
   /proc/** r,
   /bin/**  mrix,
   /usr/bin/** mrix,
   owner @{HOME}/bin/** mrix,
 }
```

If desired the basic role profile can have its permissions broken
out further by using children profiles.

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

-  confine login services to get confined users, maybe don't confine
   some services directly

-  unconfined user to confined user through service profile

-  transitions from confined user to pam service

> should calling the confined service be part of the users profile (generally no)

-  it should transition to a special profile (pam services need elevated privs)
   -   can use global profile or named transitions

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
have access to the user's files then the same uid and home directory
should be used. If the role should be limited in access to the user's
files then it is better to use a different uid and home directory.

For example, the user tom has a confined user account, and an admin
account that is allowed access to his regular files. The user jane
has a confined user account, and an admin account that is not allowed
access to her regular files.

The /etc/passwd file would look like:

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
       /bin/bash px -> confined_user,
   }
    ^tom_admin {
      ...
       /bin/bash px -> admin_user,
   }
   ^jane {
      ...
       /bin/bash px -> confined_user,
   }
    ^jane_admin {
      ...
       /bin/bash px -> admin_user,
   }
 }
```

Notice that the admin\_user profile is used by both tom and jane even
though jane\_admin is not allowed regular access to jane's files. This
can be achieved by through the use of owner rules since jane\_admin has
a different uid than jane. This profile sharing may not be appropriate
in some cases, in which case a custom profile should be used.

This is the recommended way to achieve multiple roles per user as
it doesn't reintroduce the limitations of the hard link hack. It is
recognized that adding multiple users per role has its limitations and
AppArmor 2.4 extends pam\_apparmor to address this. The pam\_apparmor
from AppArmor 2.4 has backwards compatible with AppArmor 2.3 and
may be used in place of pam\_apparmor in 2.3, as long as certain
new features are not used. For full details please see the AppArmor
2.4 documentation.

### Use hard links to create multiple login binaries

It is possible to use the bash alias hack to create alternative
roles. It still retains all the limitations of the hard link to login
shell and also requires users to remember custom command names, so
it is not the recommended way to achieve multiple roles. (It could
also use a different PATH; for example, the first entry could be
/usr/local/admin\_role/.)

To provide multiple roles a hard link (alias) is created per role type
to each program used to change roles (su, sudo, ..), and a different
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

If multiple admin types are to be used (ie. log admin, full admin,
..) separate aliases and profiles are needed for each.

Special care must be taken when making hard links to setuid binaries,
like sudo.

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
