# Navigation
Return to manpage [Index](ManPages)


# NAME

mod\_apparmor - fine-grained AppArmor confinement for Apache

# DESCRIPTION

An AppArmor profile applies to an executable program; if a portion of
the program needs different access permissions than other portions,
the program can "change hats" via aa\_change\_hat(2) to a different role,
also known as a subprofile.  The mod\_apparmor Apache module uses the
aa\_change\_hat(2) mechanism to offer more fine-grained confinement of dynamic
elements within Apache such as individual php and perl scripts, while
still allowing the performance benefits of using mod\_php and mod\_perl.

To use mod\_apparmor with Apache, ensure that mod\_apparmor is configured to
be loaded into Apache, either via a2enmod, yast or manual editing of the
apache2(8)/httpd(8) configuration files, and restart Apache. Make sure that
apparmor is also functioning.

Once mod\_apparmor is loaded within Apache, all requests to Apache will
cause mod\_apparmor to attempt to change into a hat that matches the
ServerName for the server/vhost.  If no such hat is found, it will
first fall back by attempting to change into a hat composed of the
ServerName-URI (e.g. "www.example.com-/app/some.cgi").  If that hat
is not found, it will fall back to attempting to use the hat named
by the URI (e.g. "/app/some.cgi"). If that hat is not found, it will
fall back to attempting to use the hat DEFAULT\_URI; if that also does
not exist, it will fall back to using the global Apache profile. Most
static web pages can simply make use of the DEFAULT\_URI hat.

Additionally, before any requests come in to Apache, mod\_apparmor
will attempt to change hat into the HANDLING\_UNTRUSTED\_INPUT hat.
mod\_apparmor will attempt to use this hat while Apache is doing the
initial parsing of a given http request, before its given to a specific
handler (like mod\_php) for processing.

Because defining hats for every URI/URL often becomes tedious, mod\_apparmor
provides the AAHatName and AADefaultHatName Apache configuration options.

- **AAHatName**

    AAHatName allows you to specify a hat to be used for a given Apache
    <Directory>, <DirectoryMatch>, <Location> or
    <LocationMatch> directive (see the Apache documentation for more
    details). Note that mod\_apparmor behavior can become confused if
    <Directory\*> and <Location\*> directives are intermingled
    and it is recommended to use one type of directive. If the hat specified by
    AAHatName does not exist in the Apache profile, then it falls back to the
    behavior described above.

- **AADefaultHatName**

    AADefaultHatName allows you to specify a default hat to be used for
    virtual hosts and other Apache server directives, so that you can have
    different defaults for different virtual hosts. This can be overridden
    by the AAHatName directive and is checked for only if there isn't
    a matching AAHatName. The default value of AADefaultHatName is the
    ServerName for the server/vhost configuration.  If the AADefaultHatName
    hat does not exist, then it falls back to the behavior described above.

# URI REQUEST SUMMARY

When profiling with mod\_apparmor, it is helpful to keep the following order
of operations in mind:

On each URI request, mod\_apparmor will first aa\_change\_hat(2) into
^HANDLING\_UNTRUSTED\_INPUT, if it exists.

Then, after performing the initial parsing of the request, mod\_apparmor
will:

1. try to aa\_change\_hat(2) into a matching AAHatName hat if it exists and
applies, otherwise it will
2. try to aa\_change\_hat(2) into an AADefaultHatName hat, either the
ServerName (the default) or the configuration value specified by the
AADefaultHatName directive, for the server/vhost, otherwise it will
3. try to aa\_change\_hat(2) into the ServerName-URI, otherwise it will
4. try to aa\_change\_hat(2) into the URI itself, otherwise it will
5. try to aa\_change\_hat(2) into the DEFAULT\_URI hat, if it exists, otherwise it
will
6. fall back to the global Apache policy

# BUGS

mod\_apparmor() currently only supports apache2, and has only been tested
with the prefork MPM configuration -- threaded configurations of Apache
may not work correctly. For Apache 2.4 users, you should enable the mpm\_prefork
module.

There are likely other bugs lurking about; if you find any, please report
them at [https://gitlab.com/apparmor/apparmor/-/issues](https://gitlab.com/apparmor/apparmor/-/issues).

# SEE ALSO

apparmor(7), apparmor\_parser(8), aa\_change\_hat(2) and
[https://wiki.apparmor.net](https://wiki.apparmor.net).
