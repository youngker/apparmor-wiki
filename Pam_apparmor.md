pam\_apparmor
=============

AppArmor has a
[PAM](http://en.wikipedia.org/wiki/Pluggable_Authentication_Modules)
module that a can be used to attach profiles based off of
authentication done at the user or task level.

pam\_apparmor was built to support several different use cases,
including:

-   [Role-based access controls (RBAC)](AppArmorRBAC)
-   [Multilevel style security](AppArmorMLS)

For example, you can use AppArmor to support the common case of users
being grouped into different groups such as 'staff', 'students',
and 'admins'; or 'staff', 'admin', and 'audit'. By providing common
privileges in similarly named hats available to all the services (such
as sshd, ftpd, smbd, crond, atd, xdm/kdm/gdm, mgetty/mingetty/ngetty,
sudo), a site could easily restrict users to just the privileges
necessary for their own work, while granting more powerful privileges
to another class of users for their work.

In the past, 'restricted shells' have been used to try to emulate some
of these features, but all programs that the restricted shells spawned
had to be written very carefully. General-purpose text editors such
as emacs and vim have had dozens of security updates to try to fix
escape hatches when they are called via a restricted counterpart. Some
programs, such as mutt, don't even have restricted versions, because
there are so many ways to usefully execute any program, or read and
write files.

pam\_apparmor means that your restricted environments can include
any program, with any configuration, and you can be sure your users
will have only the privileges that you explicitly grant in the
AppArmor configuration files. Of course, AppArmor can be used in
addition to restricted shells and editors and programs, if you're a
belts-and-suspenders type of person. And it integrates very cleanly
with traditional Unix DAC and ACLs; the 'owner' permission attribute
can even be used to mitigate against mistakes in Unix DAC and ACLs.

The library is named /lib/security/pam\_apparmor.so and can be
integrated into any PAM-aware application by using the corresponding
/etc/pam.d/ configuration file.

Configuring pam\_apparmor
-------------------------

To add pam\_apparmor support to a pam enabled application, add a line
like the following to the pam configuration file for the application
(usually stored in /etc/pam.d/):

```
 session  optional       pam_apparmor.so
```

Likely you will to want add the pam\_apparmor module after other
session management modules. If you make the pam\_apparmor module
'required' instead of 'optional', the session will abort if
pam\_apparmor is not able to successfully find a hat to change\_hat
into. Be careful when making it required; it is possible to cause
all attempted logins to the service to fail if the apparmor policy
is insufficient.

By default, pam\_apparmor will attempt to change\_hat into a hat based
on the primary group name of the user logging in. If that hat fails to
exist, the module will attempt to change\_hat into a hat named DEFAULT
(it is recommended to ensure this hat exists in the apparmor profiles
for applications using pam\_apparmor).

However, this is configurable by adding an option to the pam
configuration line to modify what order and what attributes
pam\_apparmor will attempt to use when attempting to change\_hat. To
do so, add 'order=' followed by a comma separated list of types of
hats to try. The type of hats available are:

*   'user' - the username will be used as the hatname
*   'group' - the primary group will be used as the hatname
*   'default' - the string 'DEFAULT' will be used as the hatname.
    Generally, this should be the hat of last resort.

The order in the list determines the order the hat will be attempted. Some example configurations:


##### The default behavior

```
 session  optional       pam_apparmor.so order=group,default
```

##### Attempt to use only the username

```
 session  optional       pam_apparmor.so order=user
```

##### Username, groupname, DEFAULT

Use the username, followed by the primary groupname, followed by
DEFAULT if the prior hats do not exist in the apparmor profile:

```
 session  optional       pam_apparmor.so order=user,group,default
```

You can also add a 'debug' flag to the pam\_apparmor session line;
this will cause the pam module to report more of what it is attempting
to do to syslog.

pam\_apparmor example
---------------------

A working pam\_apparmor example can be seen in
[Pam\_apparmor\_example](Pam_apparmor_example).
