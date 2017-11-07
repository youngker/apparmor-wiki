**DRAFT**

Overview
========

If a portion of a confined program needs different access permissions
than other portions, the program can “change hats” via AppArmor's
change\_hat() mechanism to a sub-profile, or hat. The mod\_apparmor
Apache module uses change\_hat() to offer more fine-grained confinement
of dynamic elements within Apache, such as individual php and perl
scripts, while still allowing the performance benefits of using
mod\_php and mod\_perl.

To use mod\_apparmor with Apache, ensure you have a functioning
AppArmor using 'aa-status' and that mod\_apparmor is loaded into Apache
(using tools such as a2enmod and yast or manual editing of the Apache
configuration files), then restart Apache.

URI Requests
============

Once mod\_apparmor is loaded within Apache, all requests to Apache
will cause mod\_apparmor to attempt to change into a hat named by
the URI that is defined in /etc/apparmor.d/. For example:

```
 $ cat /etc/apparmor.d/usr.lib.cgi-bin.some.cgi
 #include <tunables/global>
 /usr/lib/cgi-bin/some.cgi {
   ...
 }
```

If no such hat is found, it will attempt to use the DEFAULT\_URI hat,
if it exists. If this hat does not exist, mod\_apparmor will fall
back to using the global Apache profile. Most static web pages can
simply make use of the DEFAULT\_URI hat.

Additionally, to allow even more control, before any requests come
in to Apache, mod\_apparmor will attempt to change hat into the
HANDLING\_UNTRUSTED\_INPUT hat while Apache is doing the initial
parsing of a given http request, before the request is given to a
specific handler (like mod\_php) for processing. This hat can be
used to provide an even stricter confinement while processing the
untrusted input. Eg:

```
 $ cat /etc/apparmor.d/usr.sbin.apache2
 #include <tunables/global>
 /usr/sbin/apache2 {
   #
   # Global Apache policy
   #

   #include <abstractions/base>
   #include <abstractions/nameservice>

   capability kill,
   capability net_bind_service,
   capability setgid,
   capability setuid,
   capability sys_tty_config,

   /      r,
   /**/   r,
   /var/www/** r,

   #
   # Separate hat policies
   #
   ^DEFAULT_URI {
     ...
     /      r,
     /**/   r,
     /var/www/** r,
   }

   ^HANDLING_UNTRUSTED_INPUT {
     ...
     /      r,
     /**/   r,
   }

   ^foo {
     ...
     /usr/lib/cgi-bin/foo.gci ixr,
   }

   ^bar {
     ...
     /var/lib/bar/bar.py{,c} ixr,
   }
 }
```

Because defining hats for every URI/URL often becomes tedious,
mod\_apparmor provides the AAHatName and AADefaultHatName Apache
configuration options.

#### AAHatName

AAHatName allows you to specify a hat to be used for a given Apache
\<Directory\>, \<DirectoryMatch\>, \<Location\> or \<LocationMatch\>
directive (see the Apache documentation for more details). Note
that mod\_apparmor behavior can become confused if \<Directory\*\>
and \<Location\*\> directives are intermingled and it is recommended
to use one type of directive. If the hat specified by AAHatName does
not exist in the Apache profile, then it falls back to the behavior
described above.

#### AADefaultHatName

AADefaultHatName allows you to specify a default hat to be used for
virtual hosts and other Apache server directives, so that you can have
different defaults for different virtual hosts. This can be overridden
by the AAHatName directive and is checked for only if there isn't a
matching AAHatName or hat named by the URI. If the AADefaultHatName
hat does not exist, it falls back to the DEFAULT\_URI hat if it exists
(as described above).

Using mod\_apparmor
===================

Due to the flexibility of mod\_apparmor, it might be unclear what
mod\_apparmor is doing behind the scenes. Put simply, on each URI
request, mod\_apparmor will:

-   change\_hat() into ^HANDLING\_UNTRUSTED\_INPUT (if it exists) when performing the initial parsing of the request
-   after performing the initial parsing of the request mod\_apparmor will:
    1.  try to change\_hat() into a matching AAHatName hat if it exists and applies, otherwise
    2.  try to change\_hat() into the URI itself, otherwise
    3.  try to change\_hat() into an AADefaultHatName hat if it has been defined for the server/vhost, otherwise
    4.  try to change\_hat() into the DEFAULT\_URI hat, if it exists, otherwise
    5.  fallback to the global Apache policy

Troublshooting
--------------

**TODO**: debugging options for seeing what AppArmor and Apache are doing

Examples
--------

See ['Using mod\_apparmor with Apache to confine web
applications'](mod_apparmor_example) for an example of how to use
AppArmor to confine web applications with a trusted Apache daemon.

Caveats
=======

mod\_apparmor() currently only supports apache2, and has only been
tested with the prefork MPM configuration. Threaded configurations of
Apache may not work correctly. Additionally, due to how change\_hat()
is implemented, if there is a bug it is theoretically possible for
a confined subprofile to break out of subprofile confinement (but
still be confined by the global Apache profile).
