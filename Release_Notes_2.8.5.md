AppArmor 2.8.5 Release Notes
============================

[AppArmor 2.8.5](https://launchpad.net/apparmor/+milestone/2.8.5)
is an incremental bug fix release over AppArmor 2.8.4 that updated
policy and log parsing for some newer formats

AppArmor 2.8.5 includes revisions 2149 through 2153 from the [2.8
branch](http://bazaar.launchpad.net/~apparmor-dev/apparmor/2.8/changes/2153?start_revid=2148).

libapparmor
-----------

-   fix log parsing for new format of audit message
-   fix log parsing for new variant of the dmesg format

Policy updates
--------------

#### Abstractions

update policy abstractions for

-   X
-   aspell
-   base
-   cups
-   fonts
-   nameservices
-   p11-kit
-   php5
-   samba
-   ssl\_certs
-   ssl\_keys
-   ubuntu-browsers
-   ubuntu-email
-   ubuntu-helpers
-   user-mail

#### Reference Policies

update reference policies for

-   apache2
-   syslog-ng
-   identd
-   smbd
-   smbdldap
-   nscd
-   nameservice

