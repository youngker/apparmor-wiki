Over view of AppArmor 2.4/3.0
=============================

AppArmor 2.4/3.0 introduces several improvements that affect authoring RBAC style policy.

-   Enhanced owner conditional rules
-   Better conditional variable support
-   Change\_profile rules can be made owner conditional
-   updated pam\_apparmor
    -   can use change\_profile rules
    -   allows for multiple roles per user, and will prompt for the role to choose
    -   allows for user + role mapping to allow for single user name under MLS style policy
    -   improved configuration allows for global configuration and per profile configuration as, desired
        -   user to profile mapping via hats (as in 2.1 and 2.3)
        -   user to profile mapping via password database GECOS field
        -   user to profile mapping via external files
-   ???? finer grained permissions, mount rules, authority

MLS style security in AppArmor 2.4
==================================

In AppArmor 2.4 and newer the techniques used to achieve an
RBAC style security can be extended to achieve an [MLS style of
security.](http://developer.novell.com/wiki/index.php/Apparmor_MLS)

pam\_apparmor
=============

?? problem with transition is that it still must be staged through
a stub, as the stub needs more permissions than the role profile
should be allowed. Best way to structure this? What of adding new
hat mode that uses external user mappings, and has access features
just like change\_profile.

Configuration of pam
--------------------

### Configuration to use change\_hat

### Configuration to use change\_profile

advantages of change\_profile rules note about limitation of
change\_profile in AppArmor 2.3

### Configuration of user to profile mapping via the GECOS field

### Configuration of user to profile mapping via external files

There are two possible types of external configuration files that
allow describing user to profile mappings.

#### External role files

External role files allow mapping a list of user names to a single role
profile. This configuration style allows breaking up the assignment
of roles to different admins. An admin may have the ability to edit
the users belong to a specific role file but not another, thus that
admin can only add or remove users from the admin type he manages.

External role files are controlled by the role= configuration option

```
 role=“<role name>,<file name>”

 role=“log_admin, /etc/apparmor/roles/log_admins”
```

Multiple role= options may be specified on the pam\_apparmor module
configuration line. A given user name may appear in each specified
role file. The first file the user name appears in will be designated
the users default role.

The format of the role file is a simple text file with a single user
name per line. Lines beginning with \# are treated as comments.

```
 > cat /etc/apparmor/roles/log_admin
 # AppArmor role file, place a single user per line
 tom
 jane
 > cat /etc/apparmor/roles/mail_admin
 # AppArmor role file, place a single user per line
 tom
```

??? must have a way to specify default unconfined user -&gt; setup
an unconfined role file ???

#### External user to role files

-  can have multiple files specified on config line - user can only
   appear in file once

External user to role db files

file format \<username\>=\<role1\>,\<role2\>,... where first is default

???? tool to read pam config, and find apparmor role files and list
users and all the roles they belong too,

`    roles and which users belong to them ???`

------------------------------------------------------------------------

-   specifying a role variable and then apply user maps to how policy uses it

> This is harder to configure, if the user is to belong to multiple
> profiles however it does allow splitting up the role list so that
> users can be individually added but then how do you specify a users
> default role?

owner limitations of change\_profile can be handled by PAM as long
as the chrole is forced through a specific PAM aware application
instead of trying to write directly to /proc/ \<pid\>/attr/current
The change\_profile rule needed for the pam application is a broad
change\_profile /\*\*,

Must use method 1 for the pam\_apparmor config file, so we can get
a default role. Must use method 2 for profile variables. But this
implies 2 configuration places yuk. It also doesn't allow an sub
admin to edit the policy directly, rather it requires

problems change\_profile rules are not owner conditional so direct
changing of a role is not advisable as any user in a profile with
a change\_profile rule can change to the profile ignoring what role
the user is configured to. The best way to change roles is to relogin
with the pam module asking for the role to choose.

```
 session  {optional,required}       pam_apparmor.so change_profile order=user select
```

change\_profile - use change\_profile, will only attempt to confine
users with a specified profile. If a profile is specified and fails,
login will fail order=user - only available order for change\_profile
currently

select - allow user to select from available roles

### AppArmor 2.4

-   advantage: change\_profile rules can be user conditional, allows
    more flexibility of which profile change\_profile rules are
    allowed in. Instead of being limited to a specific set of PAM
    aware applications, an app can call change\_profile directly.

setup a aachrole applications which allows changing roles, this
allows consolidating role change information into a single profile,
or just use an include. Problem for AA 2.3 as change\_profile is not
owner conditional

usage: `aachrole <role> application`

It is a wrapper around the change\_profile interface that attempts
to change the current role and then execs the application, which will
be under the same user but a different profile. If it fails it exits
with an error message.

Two approaches to allowing change of role, include the change\_profile
rules in the role profile allowing an application to attempt to change
its profile. Or more secure only put the change\_profile rules in
a profile for aachrole, and have a px transition to aachrole. This
restricts changing of roles to the aachrole command.

have 2 include files

```
 abstractions/roles
 abstractions/roles_users
```

> how does this map to the pam config file? Need some way to make this mapping work

for both approaches general applications include abstractions/roles,
and only special one include role\_users instead. For case 1
abstractions/roles includes abstractions/roles\_users, as well as a
px transition to aachrole. For case 2 abstractions/roles only contains
the transition. This can be made configurable with a conditional.

If login services are confined then they will need to include the
change\_profile rules as well.

Profile creation wizard can ask about type of profile, application
(service), role, login service, and auto include the correct files

??? sudo ???

combining sudo and logbash

2.0.1 2.1 2.3 2.4?

logbash sudo pam\_apparmor

ix, pix ... when to use

Restricted admin user with id 0 + profile user + profile with set caps sudo?\</file\>\</username\>
