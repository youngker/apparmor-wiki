Creating a new profile
======================

The process and tools have been tested on Ubuntu 10.04 LTS with
AppArmor 2.5.1.

Test plan
---------

In order to successfully profile a program so that it is usable for
you, you must exercise the program fully while using AppArmor in
complain mode. For example, this will typically include:

-   Starting the application
-   Stopping the application
-   Restarting the application
-   Go through the man page or help and use all command-line options
-   Use different parts of the application. For example, in the case
    of Empathy, you will want to create some accounts, connect to them,
    send messages, log messages, etc

Basic process
-------------

1.  If not using auditd, temporarily disable kernel rate limiting on logs:
    
    '''
    # sysctl -w kernel.printk_ratelimit=0
    '''

2.  Create a preliminary profile with tunables and any abstractions you know you will need
3.  Put the profile into complain mode with aa-complain
4.  Execute your test plan (in another terminal if it is a terminal application)
5.  Monitor the logs for AppArmor events
6.  Adjust the profile for these events
7.  Repeat steps 4-6 as necessary
8.  When satisfied, put the profile into enforce mode with aa-enforce
9.  Monitor the logs (optionally using aa-notify) and repeat steps 4-6 as necessary

Example: confining Empathy
--------------------------

In this example we will confine the chat program Empathy.

### Disable kernel rate limiting (optional)

```
 $ sysctl -w kernel.printk_ratelimit=0
```

### Create a preliminary profile

First create a preliminary profile, including the global preamble
(to pull in variables for HOME and use system aliases. Eg
/etc/apparmor.d/usr.bin.empathy:

```
#include <tunables/global>
/usr/bin/empathy {
}
```

Then optionally (though in general recommended) add any abstractions
or paths we know about. Eg, we know Empathy needs access to system
libraries, needs to connect over the network, and is a Gnome
application. So add:

```
 include <abstractions/nameservice>
 include <abstractions/gnome>
```

Looking at the gnome abstraction, we see it already pulls in the
base, fonts, freedesktop.org and X abstractions, so we don't have to
include them.

```
#include <tunables/global>
/usr/bin/evolution {
  #include <abstractions/nameservice>
  #include <abstractions/gnome>
  owner @{HOME}/ r,
}
```

### Put in complain mode

Run application in complain mode. Eg:

```
 $ sudo aa-comlain /etc/apparmor.d/usr.bin.empathy
```

### Test the application

-   Start empathy (eg Applications/InternetEmpathy Im Client in Ubuntu)
-   Create an account
-   Create another account
-   Test IM
-   Enable logging
-   Stop empathy
-   Using the empathy debugger

### Monitor logs

Monitor the logs be looking at/examining the output of any of:

-   dmesg
-   /var/log/kern.log
-   /var/log/messages
-   aa-notify

People develop different ways of monitoring by hand. When profiling an
application from scratch by hand, it is quite helpful to group like
accesses together. One way to do this is examining the logs after a
denial. Eg, a typical log entry might be:

```
 [ 7960.018062] type=1502 audit(1292885132.576:11110):  operation="rename_dest" pid=5279 parent=1 profile="/usr/bin/empathy" requested_mask="wc::" denied_mask="wc::" fsuid=1000 ouid=1000 name="/home/jamie/.config/Empathy/geometry.ini"
```

Examining that, we can use sed and sort to help out a bit:

```
 $ dmesg | grep 'profile="/usr/bin/empathy"' | sed 's/.* profile="[^ ]*" //' | sort -u
 requested_mask="c::" denied_mask="c::" fsuid=1000 ouid=1000 name="/home/jamie/.config/Empathy/"
 requested_mask="c::" denied_mask="c::" fsuid=1000 ouid=1000 name="/home/jamie/.config/Empathy/geometry.ini.OXXONV"
 requested_mask="c::" denied_mask="c::" fsuid=1000 ouid=1000 name="/home/jamie/.config/Empathy/geometry.ini.P1XONV"
 requested_mask="c::" denied_mask="c::" fsuid=1000 ouid=1000 name="/home/jamie/.gstreamer-0.10/"
 requested_mask="c::" denied_mask="c::" fsuid=1000 ouid=1000 name="/home/jamie/.gstreamer-0.10/registry.x86_64.bin.tmpOBLQNV"
 ...
```

Once the profile is in ok shape, then simply doing something like
the following while exercising the application is usually enough:

```
 $ tail -f /var/log/kern.log
```

### Modify the profile

Add access rules by editing the profile in /etc/apparmor.d. For
this example, after examining the AppArmor denials, edit
/etc/apparmor.d/usr.bin.empathy to have:

```
 #include <tunables/global>
 /usr/bin/empathy flags=(complain) {
   #include <abstractions/base>
   #include <abstractions/nameservice>
   #include <abstractions/audio>
   #include <abstractions/fonts>
   #include <abstractions/freedesktop.org>
   #include <abstractions/gnome>
   #include <abstractions/X>

   /usr/share/empathy/** r,
   /usr/share/adium/** r,
   /usr/share/telepathy/** r,
   /usr/share/xml/iso-codes/*.xml r,
   /var/lib/dbus/machine-id r,
   owner @{PROC}/[0-9]*/fd/ r,

   # dictionaries
   /usr/share/enchant/** r,
   /usr/share/myspell/** r,
   /usr/share/hunspell/** r,
   owner @{HOME}/.config/enchant/ r,
   owner @{HOME}/.config/enchant/** r,
   owner @{HOME}/.config/enchant/*.{dic,exc} wk,

   owner @{HOME}/.gstreamer-0.10/registry.x86_64.bin* rw,
   owner @{HOME}/.local/share/webkit/icondatabase/ r,
   owner /home/jamie/.local/share/webkit/icondatabase/*.db{,-journal} rwk,

   owner @{HOME}/ r,
   owner @{HOME}/.config/Empathy/ rw,
   owner @{HOME}/.config/Empathy/** rwkl,
   owner @{HOME}/.local/share/Empathy/ rw,
   owner @{HOME}/.local/share/Empathy/** rwkl,
   owner @{HOME}/Pictures/* r,

   # For account creation
   /usr/bin/empathy-accounts ix,
 }
```

After updating the profile, you must reload profile with
'apparmor\_parser -r'. Eg:

```
 $ sudo apparmor_parser -r /etc/apparmor.d/usr.bin.empathy
```

Please note if removing access from a profile, you should not use
'-r', but instead:

-   Unload the profile with 'apparmor\_parser -R /etc/apparmor.d/\<profile\>
-   Load the profile with 'apparmor\_parser -a /etc/apparmor.d/\<profile\>
-   Restart the application

### Fine-tune the profile

As necessary, monitor the logs, modify the profile and reload the
profile to fine-tune it for the access you require.

### Put in enforce mode

Put the application in enforce mode with aa-enforce. Eg:

```
 $ sudo aa-enforce /etc/apparmor.d/usr.bin.empathy
```

At this point the application should run confined and work. Because
different code paths of the application are exercised when a profile
is in enforce mode instead of complain mode, you still need to keep a
close eye on the logs for a while, and adjust the profile as necessary.

Profile maintenance
===================

As necessary, monitor the logs, modify the profile
/etc/apparmor.d/<profile> and reload the profile to fine-tune it for
the access you require. For example, after using the empathy profile
for a while then upgrading the OS, it ended up being this:

```
 #include <tunables/global>
 /usr/bin/empathy {
   #include <abstractions/nameservice>
   #include <abstractions/audio>
   #include <abstractions/gnome>
   #include <abstractions/launchpad-integration>

   # Helper applications
   /usr/bin/empathy-accounts ix,
   /usr/lib/empathy/empathy-auth-client ix,
   /usr/lib/empathy/empathy-av ix,
   /usr/bin/empathy-debugger Cx -> empathy_debugger,

   # For opening URLs
   #include <abstractions/ubuntu-browsers>
   deny @{PROC}/[0-9]*/fd/ r, # investigate

   /usr/share/adium/** r,
   /usr/share/empathy/** r,
   /usr/share/GConf/gsettings/empathy.convert r,
   /usr/share/glib-2.*/schemas/* r,
   /usr/share/telepathy/** r,
   /usr/share/xml/iso-codes/*.xml r,

   /var/lib/dbus/machine-id r,

   owner @{HOME}/ r,
   owner @{HOME}/.cache/telepathy/ rw,
   owner @{HOME}/.cache/telepathy/** rwkl,
   owner @{HOME}/.config/Empathy/ rw,
   owner @{HOME}/.config/Empathy/** rwkl,
   owner @{HOME}/.local/share/Empathy/ rw,
   owner @{HOME}/.local/share/Empathy/** rwkl,
   owner @{HOME}/.local/share/TpLogger/ rw,
   owner @{HOME}/.local/share/TpLogger/** rwkl,
   owner @{HOME}/.local/share/folks/ r,
   owner @{HOME}/.local/share/folks/relationships.ini r,
   owner @{HOME}/Pictures/ r,
   owner @{HOME}/Pictures/** r,

   # Possibly in abstractions?
   owner @{HOME}/.gstreamer-0.10/registry.x86_64.bin* rw,
   owner @{HOME}/.local/share/webkit/icondatabase/ r,
   owner @{HOME}/.local/share/webkit/icondatabase/*.db{,-journal} rwk,
   owner @{HOME}/.cache/dconf/user rw,
   owner @{HOME}/.config/dconf/user r,
   owner @{HOME}/.config/ibus/ r,
   owner @{HOME}/.config/ibus/** rwk,
   /usr/share/applications/mimeinfo.cache r,
   /usr/share/applications/*.desktop r,
   owner @{HOME}/.local/share/applications/defaults.list r,
   owner @{HOME}/.local/share/applications/mimeinfo.cache r,
   owner @{HOME}/.local/share/applications/mimeapps.list r,
   /usr/share/enchant/** r,
   owner @{HOME}/.config/enchant/ r,
   owner @{HOME}/.config/enchant/** r,
   owner @{HOME}/.config/enchant/*.{dic,exc} wk,
   /usr/share/{hunspell,myspell}/ r,
   /usr/share/{hunspell,myspell}/** r,

   profile empathy_debugger {
     #include <abstractions/base>
     #include <abstractions/nameservice>
     #include <abstractions/fonts>
     #include <abstractions/freedesktop.org>
     #include <abstractions/gnome>
     #include <abstractions/X>

     /usr/bin/empathy-debugger r,
     /usr/share/empathy/** r,

     /var/lib/dbus/machine-id r,
     /tmp/.X[0-9]*-lock r,

     owner @{HOME}/.config/Empathy/ r,
     owner @{HOME}/.config/Empathy/** rwkl,

     # For saved logs
     owner @{HOME}/*.log w,
     owner @{HOME}/{Desktop,Public,tmp}/*.log w,
     owner /tmp/*.log w,
   }
 }
```
