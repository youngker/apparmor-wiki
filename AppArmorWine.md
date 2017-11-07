Wine is an API compatibility layer that allows Windows applications to
run on Linux. AppArmor can be used to control which programs it can
run and what resources they can use when run. The specifics of the
confinement will vary depending on the distro, installation config
options and desired confinement granularity, thus the following only
provides some basics and guidelines on how wine applications can be
confined with AppArmor.

About wine applications
=======================

Wine applications on Linux can be launched in two different ways
either directly via invoking

```
 wine <program> ...
```

or via a binfmt\_misc handler, using just the program name.

```
 <program> ...
```

For apparmor the method via which the application is launched is
important and will determine the strategy that can be used to confine
the wine application. If the wine application is launched via its name
(binfmt\_misc handler) then either confinement strategy is possible,
but if the application is invoked using the wine command directly
then only the shared profile strategy is possible. This is because at
this time AppArmor does not provide a way to change profile attachment
based on executable parameters.

Regardless of the method of invocation, the execution follows the
same basic pattern.

1.  The wine program determines the threading model and invokes the preloader
    -   If invoked via wine this will appear as the wine executable
    -   If invoked via the program name (binfmt\_misc handler) then this will appear as the program name as the executable
2.  The preloader sets up the mapping for the executable and sets up the wineserver connection (launching it if necessary).
3.  The application is then run

There are a few important points to note:

-   The binfmt\_misc handler allows attaching to applications by executable name, thus allowing for a separate profile for each executable
-   The wine-preloader and wineserver programs access the Windows execute files by requesting the read access permission. They do not map the files as executable binaries nor do they exec the application.

For more information of wine start up see [the wine
website](http://www.winehq.org/docs/winedev-guide/c2729).

Strategies for confining
========================

Because of how wine is structured there are three strategies that
can be used to confine the Windows applications that are run under
it. All programs can be run under a single wine-loader, and wineserver
profile. Each program can be run under its own profile, or a hybrid
of the two can be used in some circumstances.

Running all programs under the wine-loader, and wineserver profiles
-------------------------------------------------------------------

Because of how wine executes programs they all end up running under
the wine-preloader, interacting with the wineserver, if the goal
is to just control the resources used by Windows applications and
separate them from the Linux application then making a profile for
the wineserver and wine-loader is the way to go. All wine applications
will run under and share these common profiles.

The wineserver and wine-loader can either be specified as separate
profiles like

```
 #Example pruned wine-preloader profile, needs to be further expanded to be functional
 #include <tunables/global>
 /usr/bin/wine-preloader {
   #include <abstractions/base>
   #include <abstractions/fonts>
   #include <abstractions/nameservice>
   #include <abstractions/ubuntu-gnome-terminal> 
    /usr/bin/wine-preloader rix,
   /usr/bin/wineserver px,
   /usr/bin/wine mr,
   /usr/lib{,32,64}/** mr,
    /usr/share/wine/fonts/ r,
   /usr/share/wine/fonts/** r,
   /usr/share/wine/wine.inf r,
   /etc/fstab r,
    /home/** r,
    /opt/wine/.wine/** r,
  }
  /usr/bin/wineserver {
   #include <abstractions/base>
   #include <abstractions/nameservice>
    /usr/bin/wineserver r,
    /tmp/.wine-*/server-*/ r,
   /tmp/.wine-*/server-*/* wk,
    /home/*/ r,
   /home/*/Desktop/ r,
    /home/** r,
   /home/*/.wine/* rw,
   “/opt/wine/.wine/*/Program Files/**.exe/” r,
 }
```

The wineserver and wine-preloader profiles can be combined but this
is not recommended as the server while it accesses many of the same
files has a different role than the application.

Running programs under their own profile
----------------------------------------

When applications are launched via their name (via the binfmt\_misc
handler), the application can be confined with its own profile. This
done by making a profile with that attaches to the executable windows
program name and uses an ix transition for the wine-preloader. This
will keep the programs execution within the defined profile.

```
 profile “/opt/wine/.wine/drive_c/Program Files/WinRAR/WinRAR.exe” {
   #include <abstractions/base>
   #include <abstractions/fonts>
   #include <abstractions/nameservice>
   #include <abstractions/ubuntu-gnome-terminal>
    deny network,         # abstractions are allowing network, deny it
    /usr/bin/wine-preloader rix,
   /usr/bin/wineserver rpx,
    /usr/bin/wine mr,
   /usr/lib{,32,64}/** mr,
    / r,
   /etc/fstab r,
   /proc/scsi/scsi r,
    /usr/share/wine/fonts/ r,
   /usr/share/wine/fonts/** r,
   /usr/share/wine/wine.inf r,
    /tmp/.wine-*/server-*/ r,
   /tmp/.wine-*/server-*/* wk,
    /home/** r,
   /home/*/.wine/ r,
   /home/*/.wine/* rw,
   /home/*/.wine/**/ r,
   /home/*/.wine/*/windows/*.ini r,
   /home/*/.wine/*/windows/system32/*.dll r,
   /home/*/.wine/*/windows/system32/*.exe r,
   /home/*/.wine/*/windows/system32/drivers/*.sys r,
   /home/*/.wine/*/windows/system32/*.drv r,
    /home/*/.wine/.update-timestamp rw,
    “/home/*/.wine/*/users/*/Application Data/WinRAR/version.dat” r,
   /home/*/.wine/*/windows/winsxs/manifests/x86_microsoft.windows.common-controls_*_none_deadbeef.manifest r,
    “/home/*/.wine/*/Program Files/Internet Explorer/iexplore.exe” r,
   /home/*/.wine/*/windows/hh.exe r,
    /opt/wine/.wine/**/ r,
   “/opt/wine/.wine/*/Program Files/WinRAR/WinRAR.exe” r,
 }
```

If ix is not used the profile will be very small consisting of not
more than an exec rule to the pre-loader

```
 profile “/opt/wine/.wine/drive_c/Program Files/WinRAR/WinRAR.exe” {
   /usr/bin/wine-preloader rpx,
 }
```

which will result in needing to use a shared pre-loader profile to
confine all wine executed windows applications.

It is important to note that the programs profile must be selective
with granting read access to program files, as wine can execute
a windows application, if it has read access without any **m**
or **x** access to the windows program executable.

When using preloader profiles it is generally desirable that the
wineserver remain in its own profile, as it can be shared by multiple
executables, with the first one launching it if it is not found. If it
does not run under its own profile, the confinement on the wineserver
will be inconsistent (it will depend on which application launched
it). The only situation where it is safe to fold the wineserver
profile into the application profile is when access to only a single
application is desired.

If per application profiles are being used it is desirable to block
execution methods that can circumvent application confinement. This
means that either the user must be confined by a profile blocking
access to wine and wine-preloader or that wine and wine-preloader
should be confined with a trap profile.

Blocking execution with trap profiles
=====================================

Regardless of which method of confinement is chosen it may be desirable
to block execution of some, or all windows applications that don't
have a profile. This can be done via a combination of profile rules
and trap profiles. Profile rules will need to block read access to
the given application, and exec rules can block direct wine program
execution from linux commands.

A trap profile is a profile that attaches to an application but has
no permissions, thus causing the execution of the application. This
can be thought of a

The trap profiles that will be needed depends on which confinement
strategy is chosen. If per application profiles are used then it
makes sense to block invocation of applications via wine and maybe
all applications that don't have profiles.

To block invocation via wine, ie.

```
 wine program [arguments ... ]
```

create a wine trap profile

```
 #trap calls to wine
 /usr/bin/wine { }
```

To block execution of any wine program that doesn't have a profile,
create a trap profile for wine-preloader

```
 #trap execution of all windows programs without their own profile
 /usr/bin/wine-preloader { }
```

If a shared wine-preloader is being used blocking execution of
applications becomes harder. Execution from within the application
can only be blocked by denying read access to the desired program
binaries. Deny rules, or not allowing broad read access to locations
with executables from with in the wine-preloader profile will
achieve this.

Blocking direct execution of some applications from unconfined can
be done via targeted trap profiles, ie.

```
 “/home/*/.wine/*/Program Files/Internet Explorer/iexplore.exe” { }
```

Or alternately a generic trap profile can be used in combination with
a stub per application profile. The stub profile is present only to
override the generic trap profile and then transitions immediately
to the wine-preloader profile.

```
 #block all windows executables
 /**.exe { }

 profile “/opt/wine/.wine/drive_c/Program Files/WinRAR/WinRAR.exe” {
   /usr/bin/wine-preloader rpx,
 }
```
