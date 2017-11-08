Background knowledge used in developing confinement techniques
--------------------------------------------------------------

### Methods of attaching profiles

AppArmor requires a profile be attached to an application before it
becomes mediated. The methods of attachment available dictate how
the user confinement or role can be realized. There are three primary
ways in which profile attachment can be achieved.

-   AppArmor policy directed changes

    -   Automatic profile attachment via program name (available in
        all versions of AppArmor)

        Profile names are the primary way in which AppArmor determines
        when an application should be confined. When a new application
        is launched by an unconfined process, AppArmor checks if
        the application should be confined. To determine whether the
        application should be confined, AppArmor checks if a profile of
        the same name as the application exists, and if so attaches it.

    -   Profile directed attachment

        When an application is launched from a confined process the
        profile is checked to determine execution permissions. If
        the profile allows the application to be executed, it
        also determines which profile will be attached to the new
        application.

        -    Exec modes in AppArmor 2.0 - AppArmor 2.1

             The execution modes in the earlier versions of AppArmor
             are limited to three basic choices which greatly affect
             what can be done for roles/confinement of users.

            -   px/Px - profile transition - transition to profile name matching the name of application
            -   ix - inherit transition - inherit the current profile (the profile confining the applications parent)
            -   ux/Ux - unconfined transition - drop profile confinement (not recommended for use).

        -   Exec modes in AppArmor 2.3 -

            AppArmor 2.3 added new exec modes and profile name options
            that aid in confining users, and setting up roles.

            -   cx/Cx - child ([subprofile](subprofile)) transition -
                transition to a [subprofile](subprofile) with a name
                matching the name of the application.

            -   px/Px -&gt; &lt;name&gt; - profile named transition - transition to the profile with the specified &lt;name&gt;
            -   cx/Cx -&gt; &lt;name&gt; - child named transition - transition to the subprofile with the specified &lt;name&gt;

            -   profile names in AppArmor 2.3 don't have to specify
                a profile name, but instead can have a name that is
                descriptive of the general confinement being used.

-   Application Directed change

    -    change\_hat (available in all versions of AppArmor)

         The change\_hat interface allows an application to do self
         directed profile transitions between a parent profile and
         special hat sub profiles. Applications must be modified
         to make use of change\_hat at the appropriate locations in
         the code.

         It is best used in situations where direct access to the
         parent profile's resources is not allowed (e.g., interpreters
         in Apache, applications hosted in Tomcat, different tabs in
         web browsers).

    -    change\_profile (available in AppArmor 2.3 and newer)

         The change\_profile interface is similar to change\_hat
         in allowing an application to do self directed profile
         transitions, except that it is more flexible. It can be used
         from both confined and unconfined applications, allowing
         one way transition to potentially any loaded profile. When
         called from a confined application the profiles that can
         be transitioned to are limited by the currently confining
         profile.

    -    set\_profile (available in AppArmor 2.0 and newer)

         The set\_profile interface allows root owned unconfined
         processes to change the AppArmor confinement of another
         processes. It can be used to remove confinement or force
         an application into confinement, or do direct profile to
         profile transitions.

-   Application directed change via proxy

    -    pam\_apparmor (available in AppArmor 2.1 and newer)

         The pam\_apparmor pam module can be used to specify
         confinement for applications using pam authentication. The
         pam\_apparmor module provides a pluggable way for applications
         to use change\_hat and change\_profile based on authentication
         without having to directly modify the application.

         -   In AppArmor 2.1, 2.2, and 2.3 the pam\_apparmor module
             uses change\_hat, so to be able to specify confinement
             it must be called from a confined application.

         -   In AppArmor 2.4/3.0 the pam\_apparmor module can use both
             change\_hat and change\_profile and is more flexible and
             has methods of specifying attachment without requiring
             it be called from a confined application.

    -   knightshell

        The knightshell is a special wrapper program for bash
        designed as an alternate method of achieving confined login
        shells without resorting to hardlinks. The knightshell
        must be set as the user's default login shell and uses the
        GECOS field in nss compatible login databases to specify
        confinement. It starts /bin/bash confined in a profile
        with a configurable hat. [For more details, see the source
        package](http://download.opensuse.org/repositories/home:/mge1512/openSUSE_Factory/src/knightshell-0.2.0-11.46.src.rpm).

    -   mod\_apparmor/mod\_changehat

        lhe mod\_apparmor plug-in for the Apache web server is not used
        to achieve standard user/role confinement but does provide
        subprofile confinement for web pages. mod\_apparmor is how
        AppArmor can be used to achieve specialized confinement for web
        applications; the confinement can be done via Apache directives
        &lt;Directory&gt;, &lt;Location&gt;, and &lt;Vhost&gt;,
        as well as individual URLs.

### Using hard links to provide alternate profiles

A hard link to an executable file creates a new name (alias) that
AppArmor profiles and rules can be matched against. Soft links can
not be used to create an alternate, because of the way AppArmor does
mediation it does not see a soft link as an alternate name. Creating
an alternate name provides a simple means by which multiple profiles
(one per name) can be attached to a single executable file.

The use of hardlinks does have some update and maintenance
problems. When a program is updated hardlinks remain pointed at the
previous version of the program. This can result in version problems,
failed executions, upgrade breakage, and in the worst case running
software with known security bugs.

### Anatomy of a profile to confine a user or create a role

The basic idea behind creating roles with profiles, is that a generic
profile is created with permissions that cover multiple executables. In
general the ix permission is used for all executables allowed within
the role, and the profile's access permissions are the union of
permissions for each executable.

AppArmor prior to the 2.3 release required that the generic role
profile name map to an executable so that it could be attached. This
limitation meant that the profile names where not descriptive, and
that tricks with hard links were needed to attach and define roles.

AppArmor 2.3 added several features that enhance creating role profiles.

-   profile names don't have to specify attachment so that the profile name can be descriptive.
-   new transition modes allow more flexibility on domain transitions when needed.
-   [owner conditional rules](owner_conditional_rules) enable defining
    broader profiles that still limit users effectively. (AppArmor
    can ensure different unix users are kept separate even if there
    are mistakes in the traditional filesystem permissions.)

Methods of confining a user / creating a role
---------------------------------------------

Setting up restricted admin profiles
------------------------------------

AppArmor prior to the 2.3 release required relied on setting the
users uid 0 to be able to create restricted admin profiles. AppArmor
2.3 added the ability to give a user limited capabilities so that a
non-uid-0 user could be given limited admin privileges.

### Roles via restricted login shell

AppArmor can be used to create “roles” (in the RBAC sense)
that operate as restricted shells in Linux. This even works on
root shells. For instance, suppose you have some junior system
administrators in your enterprise, and their job is to do system
log analysis looking for problems. They need root access to do this,
but you don't feel comfortable trusting them; they might be evil, or
they might just make mistakes. So you want to allow them to only have
\*part\* of root's privilege to access the system log, but \*not\*
the power to mess with the Oracle database, reboot the machine, etc.

To do this, you create a role using AppArmor. You do this by:

1.   Creating a “special” shell, hardlink to the shell, for the
     role, e.g. call it logbash for the role of syslog analyst.

2.   Create an AppArmor profile for logbash that restricts anyone
     running logbash to only do the necessary operations.

3.   Make logbash be the default login shell for people who will be
     operating in this role.

4.   Change the UID of these people to 0 so that they have root's
     privilege, but use their own password and are restricted to run
     logbash, so you don't have to share root's password.

In AppArmor 2.3 it is strongly recommended that pam\_apparmor is used
instead of this technique to create restricted shells.

#### How is AppArmor's “restricted shell” capability different from sudo?

If your suduoers file grants a user access to e.g. vim, then the user
can type :!\<enter\> and be presented with a shell. From that shell, the
user has complete privileges over the entire system. The same happens
with mutt, pine, less, more, and many other simple utilities. (Leave
off the ':' where necessary. Add 'sh' where necessary. :)

AppArmor would allow the user to spawn a shell only if a shell was
allowed in the confining profile. AppArmor will only allow the user
to execute commands, read files, and write files, that are defined
in the profile.

sudo has no control over the program once it has been
exec()'d. AppArmor can control execution all the way down.

AppArmor can be combined with sudo as an alternative RBAC solution
(see below).

### sudo

The sudo application provides the ability to run application

controlled way to run a specific applications with root privledges

problem - it doesn't limit what the application can do once run as root

This technique is very limited in pre 2.3 versions of AppArmor

-   placing a profile around sudo allows enforcing sudos config restrictions
    -    allows placing profiles around applications run from sudo (transition rules)

confining sudo can be done globally - everybody gets the same profile

within a profile - pre AppArmor 2.3

If you don't want to use a global sudo profile or want different
transitions than specified by a global sudo profile the sudo transition
must be specified within the confining profile.  This is done by
making sudo an ix transition

problem: can only ix, or use global profiles   boo

-   thus need to use hard linked command names to provide alternate confinement

can combine with pre 2.3.1 pam\_apparmor to provide some alternate confinement through change\_hat

-   problem can't do change\_hat within change\_hat

AppArmor 2.3

use named transitions and/or child profiles

### How can AppArmor be combined with sudo to provide RBAC?

Combining sudo with AppArmor provides a complementary form of RBAC that
can be combined with restricted login shells. When using restricted
login shell the user requires a different password per role, and the
either a role and its password must be shared between multiple users,
or multiple versions of the role need to be setup (one per user). When
combining AppArmor with sudo a single role can have multiple users,
each one using their own password to access the role.

To do this, you create a role using AppArmor. You do this by:

1.   Creating a “special” shell, hardlink to the shell, for the
     role, e.g. call it logbash for the role of syslog analyst.

2.   Create an AppArmor profile for logbash that restricts anyone
     running logbash to only do the necessary operations.

3.   Edit the /etc/sudoers.conf file, to setup a role, that allows
     the desired users to execute the “special” shell.

4.   users can enter the role by doing `> sudo logbash`

For the user to be able to directly transition to other roles the
“special” profile must provide a Px transition to sudo. If the
user should not be able to transition directly from role to role then
profile for the role should not be allowed to execute sudo.

If you wish to use AppArmor to confine sudo when using it to provide
roles, the sudo profile must contain Px transitions to all the
“special” shells that are setup to support different roles. The
controlled mapping of user to role is handled by sudo, so a user will
still only be able to access the roles he is allowed to by the sudo
config file.

If this is combined with restricted login shell style of RBAC the
“special” shell profile should contain a Px transition to sudo
and sudo should be confined with an AppArmor profile.

### knightshell

The knightshell is a special shell that uses change\_hat to set user
confinement on login. similar to confined login shell. This can be
used to provide default confinement for all users in a specific group,
or provide per-user confinement. However, it currently only works
with change\_hat, not change\_profile, and it is currently hard-coded
to run /bin/bash, which limits its usefulness somewhat. More generic
wrapper programs would be nice.
