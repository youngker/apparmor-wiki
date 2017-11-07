(DRAFT)

Introduction
============

AppArmor provides great flexibility in confining web applications
in Apache through mod\_apparmor. In this example, the main Apache
process will be considered trusted, and [mod\_apparmor](mod_apparmor)
will be used by Apache to change\_hat() into different profiles based
on the web application. This example is known to work on Ubuntu 10.04
LTS with AppArmor 2.5. This example is based on the work of Marc
Deslauriers when providing an example profile for phpsysinfo in Ubuntu.

Overview
========

The idea behind mod\_apparmor is simple: if Apache is configured
to use mod\_apparmor, when someone navigates to a URL in Apache,
Apache will transition to an AppArmor profile via mod\_apparmor
using AppArmor's change\_hat() mechanism. For simplicity, the
main Apache process in the example will be considered trusted
and it therefore will be confined with a lenient profile
(/etc/apparmor.d/usr.lib.apache2.mpm-prefork.apache2
in this example, but [it can be named
anything](Policy_Layout#Profile_file_naming_conventions)). Profiles
for the web applications are located in /etc/apparmor.d/apache2.d. We
will confine MoinMoin and nagios in this example.

Initial Configuration
=====================

Apache must be configured to use mod\_apparmor. In Ubuntu, this is done by:

```
 $ sudo apt-get install libapache2-mod-apparmor
 $ sudo a2enmod apparmor
 $ sudo aa-enforce /etc/apparmor.d/usr.lib.apache2.mpm-prefork.apache2
 $ sudo /etc/init.d/apache2 restart
```

This installs mod\_apparmor on the system, enables the mod\_apparmor
module in Apache, loads the policy for the Apache process, the restarts
Apache so it is confined. Non-Ubuntu systems may have a different name
for the libapache2-mod-apparmor package. Systems without 'a2enmod' can
add the following to the appropriate place in apache.conf/httpd.conf:

```
 LoadModule apparmor_module /usr/lib/apache2/modules/mod_apparmor.so
```

The policy in /etc/apparmor.d/usr.lib.apache2.mpm-prefork.apache2 as used on Ubuntu 10.04 LTS is:

```
    #include <tunables/global>

    /usr/lib/apache2/mpm-prefork/apache2 {
      #include <abstractions/base>
      #include <abstractions/nameservice>

      capability kill,
      capability net_bind_service,
      capability setgid,
      capability setuid,
      capability sys_tty_config,

      / rw,
      /** mrwlkix,

      ^DEFAULT_URI {
        #include <abstractions/base>
        #include <abstractions/nameservice>

        / rw,
        /** mrwlkix,

      }

      ^HANDLING_UNTRUSTED_INPUT {
        #include <abstractions/nameservice>

        / rw,
        /** mrwlkix,

      }

      # This directory contains web application
      # package-specific apparmor files.

      #include <apache2.d>
    }
```

This policy will need to be adjusted for your system, especially if
not using Ubuntu. Pay particular attention to the confined binary
name '/usr/lib/apache2/mpm-prefork/apache2 { }', which may need to be
changed to something like '/usr/sbin/httpd'. In this lenient policy
for Apache, the ^DEFAULT\_URI and ^HANDLING\_UNTRUSTED\_INPUT hats
allow access to everything on the system, just like an unconfined
Apache would (DAC permitting).

Configure Apache Directories and Virtual Hosts
==============================================

Once Apache is configured to use mod\_appamor and is confined
with AppArmor (see above), you are ready to configure Apache
for your web applications by \<Directory\>, \<DirectoryMatch\>,
\<Location\>, \<LocationMatch\> or \<VirtualHost\>. When confining
by \<Directory\*\> and \<Location\*\>, use the **AAHatName**
apache configuration option. You can also specify a default hat
with the **AADefaultHatName** apache configuration option on
a \<VirtualHost\> basis for when there is no matching hat (see
[mod\_apparmor](mod_apparmor) for details). For simplicity, we will
use \<Directory\*\> only in this example. Configuring MoinMoin and
Nagios is beyond the scope of this tutorial and it is assumed they
are setup and working correctly.

MoinMoin
--------

Assuming your Apache configuration has something like this for MoinMoin:

```
 Alias /wiki/ "/usr/share/moin/htdocs/"
 Alias /static/ "/usr/share/moin/htdocs/"
 ScriptAlias /Wiki "/var/lib/moin/mywiki/moin.cgi"

 <Directory /var/lib/moin/mywiki>
   ...
 </Directory>

 <Directory /usr/share/moin/htdocs>
   Order deny,allow
   Allow from all
 </Directory>
```

adjust the `<Directory /var/lib/moin/mywiki>` directive to have:

```
 <Directory /var/lib/moin/mywiki>
   AAHatName mywiki
 </Directory>
```

Next, create a policy for the ^mywiki hat in /etc/apparmor.d/apache2.d/mywiki:

```
 ^mywiki {
   #include <abstractions/apache2-common>
   #include <abstractions/base>
   #include <abstractions/nameservice>
   #include <abstractions/python>

   # for log writing (could be abstracted)
   /var/log/apache2/access.log w,
   /var/log/apache2/error.log w,

   # Access to standard MoinMoin system files
   /etc/mime.types r,
   /etc/moin/ r,
   /etc/moin/farmconfig.py{,c} r,
   deny /etc/moin/*.pyc w,
   /usr/bin/python2.[567] ixr,
   /usr/lib/python2.[567]/** mr,
   /usr/share/moin/** r,
   /usr/share/python-support/python-moinmoin/** r,
   /var/www/ r,

   # Access specific to 'mywiki'
   /etc/moin/mywiki.py{,c} r,
   /var/lib/moin/mywiki/ r,
   /var/lib/moin/mywiki/** rw,

   /var/lib/moin/mywiki/moin.cgi ixr,
 }
```

Notice how this profile uses the paths in Apache's
configuration. Adding other features such as SSL would require
additional access rules. If you host multiple wiki's, you might want
to put some of the above into a site-specific MoinMoin abstraction.

Once Apache has been adjusted and the AppArmor profile in place,
reload the profile and restart Apache with:

```
 $ sudo apparmor_parser -r /etc/apparmor.d/usr.lib.apache2.mpm-prefork.apache2
 $ sudo /etc/init.d/apache2 restart
```

Then see if AppArmor picked up the configuration:

```
 $ sudo aa-status
 apparmor module is loaded.
 22 profiles are loaded.
 22 profiles are in enforce mode.
    ...
    /usr/lib/apache2/mpm-prefork/apache2
    /usr/lib/apache2/mpm-prefork/apache2//DEFAULT_URI
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT
    /usr/lib/apache2/mpm-prefork/apache2//mywiki
    ...
 0 profiles are in complain mode.
 17 processes have profiles defined.
 17 processes are in enforce mode :
    /usr/lib/apache2/mpm-prefork/apache2 (3425) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (19398) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (4196) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (20910) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (18272) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (3055) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (11867) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (4401) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (20663) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (21132) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (26986) 
    ...
 0 processes are in complain mode.
 0 processes are unconfined but have a profile defined.
```

Nagios
------

The process is similar to the above for all confined web
applications. For example, assuming your Apache configuration has
something like this for Nagios:

```
 ScriptAlias /cgi-bin/nagios3 /usr/lib/cgi-bin/nagios3
 ScriptAlias /nagios3/cgi-bin /usr/lib/cgi-bin/nagios3
 Alias /nagios3/stylesheets /etc/nagios3/stylesheets
 Alias /nagios3 /usr/share/nagios3/htdocs

 <DirectoryMatch (/usr/share/nagios3/htdocs|/usr/lib/cgi-bin/nagios3|/etc/nagios3/stylesheets|/usr/share/nagios/htdocs/images)>
   ...
 </DirectoryMatch>
```

adjust the `<DirectoryMatch>` directive to have:

```
 <DirectoryMatch (/usr/share/nagios3/htdocs|/usr/lib/cgi-bin/nagios3|/etc/nagios3/stylesheets|/usr/share/nagios/htdocs/images)>
   AAHatName nagios
   ...
 </Directory>
```

Next, create a policy for the ^nagios hat in /etc/apparmor.d/apache2.d/nagios:

```
 ^nagios {
   #include <abstractions/apache2-common>
   #include <abstractions/base>

   # for log writing (could be abstracted)
   /var/log/apache2/access.log w,
   /var/log/apache2/error.log w,

   /etc/nagios3/ r,
   /etc/nagios3/** r,

   /usr/lib/cgi-bin/nagios3/*.cgi ixr,

   /usr/share/nagios{,3}/htdocs/ r,
   /usr/share/nagios{,3}/htdocs/** r,

   /var/cache/nagios3/objects.cache rw,
   /var/cache/nagios3/status.dat rw,

   # For trends
   /var/log/nagios3/nagios.log r,
   /var/log/nagios3/archives/*.log r,

   # Access to this file cause commands to be run. Allow access to this file
   # with extreme care
   audit deny /var/lib/nagios3/rw/nagios.cmd rw,
 }
```

Once Apache has been adjusted and the AppArmor profile in place,
reload the profile and restart Apache with:

```
 $ sudo apparmor_parser -r /etc/apparmor.d/usr.lib.apache2.mpm-prefork.apache2
 $ sudo /etc/init.d/apache2 restart
```

Then see if AppArmor picked up the configuration:

```
 $ sudo aa-status
 apparmor module is loaded.
 22 profiles are loaded.
 22 profiles are in enforce mode.
    ...
    /usr/lib/apache2/mpm-prefork/apache2
    /usr/lib/apache2/mpm-prefork/apache2//DEFAULT_URI
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT
    /usr/lib/apache2/mpm-prefork/apache2//mywiki
    /usr/lib/apache2/mpm-prefork/apache2//nagios
    ...
 0 profiles are in complain mode.
 17 processes have profiles defined.
 17 processes are in enforce mode :
    /usr/lib/apache2/mpm-prefork/apache2 (3425) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (19398) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (4196) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (20910) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (18272) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (3055) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (11867) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (4401) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (20663) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (21132) 
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT (26986) 
    ...
 0 processes are in complain mode.
 0 processes are unconfined but have a profile defined.
```

Modifying Policy and Restarting Apache
======================================

Usually it is enough simply to reload the policy and then restart Apache like so:

```
 $ sudo apparmor_parser -r /etc/apparmor.d/usr.lib.apache2.mpm-prefork.apache2
 $ sudo /etc/init.d/apache2 restart
```

Sometimes this is not enough (e.g, after removing access rules), and
the best method is to bring down Apache fully, unload and then load
the policy, then bring Apache up. This should be done before putting
the machine and its policy into production. For example:

```
 $ sudo /etc/init.d/apache2 stop
 $ sudo apparmor_parser -R /etc/apparmor.d/usr.lib.apache2.mpm-prefork.apache2
 $ sudo apparmor_parser -a /etc/apparmor.d/usr.lib.apache2.mpm-prefork.apache2
 $ sudo /etc/init.d/apache2 start
```
