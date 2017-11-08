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
-   Use different parts of the application. Eg, in the case of Evolution, you will want to test email, addressbook and calendars

Basic process
-------------

1.  If not using auditd, temporarily disable kernel rate limiting on logs:

    ```
    # sysctl -w kernel.printk_ratelimit=0
    ```

2.  Start aa-genprof in a terminal. E.G.:

    ```
    $ sudo aa-genprof <path to executable>
    ```

3.  Execute your test plan (in another terminal if it is a terminal application)
4.  Go back to aa-genprof, and follow the instructions for updating policy
5.  Put the profile into complain mode and use aa-logprof to fine-tune the profile. The better the application was exercised to begin with, the less fine-tuning is required
6.  If the profile is in complain mode, put the profile into enforce mode with aa-enforce
7.  Monitor the logs (optionally using aa-notify) and adjust the profile manually as necessary

### Caveats

Currently the tools do not properly utilize variables such as @{PROC},
and @{HOME}, so you may want to adjust the profile after to use
abstractions that the tools could not discover.

It is important to note that aa-logprof will only detect complaints
when the application is exercised with the profile in complain mode. It
will not pick up denials from an enforcing profile (confirmed on
2.5.1 - 2.7.0).

Example: confining Evolution
----------------------------

1.  Disable kernel rate limiting:

    ```
    $ sudo sysctl -w kernel.printk_ratelimit=0
    ```

2.  Start aa-genprof in a terminal. You will be prompted to access the profile repository. For now, disable it. Eg:

    ```
    $ sudo aa-genprof /usr/bin/evolution
    ```

3.  Execute the test plan (see above). In this case:
    -   Start Evolution
    -   Create an email account with the wizard
    -   Get email
    -   Send email
    -   Print an email
    -   Delete an email
    -   Close evolution
    -   Add a contact
    -   Add a task
    -   Add a memo
    -   Add a calendar entry
    -   Access a web calendar
    -   use evolution --force-shutdown
4.  go back to aa-genprof, and follow the instructions for updating policy:
    1.  First, (S)can system logs
    2.  Then, (F)inish
5.  Put the profile into complain mode using 'aa-complain /etc/apparmor.d/usr.bin.evolution' then exercise the application more to fine-tune the profile
6.  Use aa-logprof to fine-tune the profile. The better the application was exercised to begin with, the less fine-tuning is required
7.  When satisfied, put the profile into enforce mode with aa-enforce
8.  Monitor the logs and adjust the profile manually as necessary

After a pass with aa-genprof/aa-logprof in complain mode and then more fine-tuning with aa-logprof, the resulting profile is (with several new rules added for globbing):

```
 # Last Modified: Mon Dec 20 21:43:45 2010
 #include <tunables/global>

 /usr/bin/evolution {
   #include <abstractions/audio>
   #include <abstractions/base>
   #include <abstractions/fonts>
   #include <abstractions/gnome>
   #include <abstractions/nameservice>
   #include <abstractions/python>

   deny /boot/initrd.img-2.6.32-26-generic r,
   deny /boot/vmlinuz-2.6.32-26-generic r,
   deny /etc/python2.6/sitecustomize.py r,
   deny owner /home/jamie/.bash_history r,
   deny owner /home/jamie/.bash_logout r,
   deny owner /home/jamie/.bashrc r,
   deny owner /home/jamie/.profile r,
   deny owner /home/jamie/.sudo_as_admin_successful r,
   deny owner /home/jamie/.xsession-errors r,

   /**/ r,
   /etc/timezone r,
   owner /home/*/.ICEauthority r,
   owner /home/*/.camel_certs/ w,
   owner /home/*/.camel_certs/* rw,
   owner /home/*/.config/enchant/ rw,
   owner /home/*/.config/enchant/* rwk,
   owner /home/*/.config/user-dirs.dirs r,
   owner /home/*/.esd_auth r,
   owner /home/*/.evolution/** rwlk,
   owner /home/*/.gnome2/accels/evolution rw,
   owner /home/*/.goutputstream-* rw,
   owner /home/*/.gtk-bookmarks r,
   owner /home/*/.pulse-cookie r,
   owner /home/*/.recently-used.xbel* rw,
   owner /home/*/Public/* w,
   /proc/filesystems r,
   /usr/bin/evince Ux,
   /usr/lib/evolution/2.28/killev cx,
   /usr/share/enchant/* r,
   /usr/share/evolution-data-server-*/** r,
   /usr/share/evolution/** r,
   /usr/share/gtkhtml-*/** r,
   /usr/share/hunspell/* r,
   /usr/share/xml/iso-codes/* r,
   /var/lib/dbus/machine-id r,

   profile /usr/lib/evolution/2.28/killev {
     #include <abstractions/base>
     #include <abstractions/gnome>
     #include <abstractions/nameservice>

     capability sys_ptrace,

     /**/ r,
     /bin/dash rix,
     /proc/*/cmdline r,
     /proc/*/stat r,
     /proc/filesystems r,
     /usr/bin/killall rix,
   }
 }
```

The tools did not pick up a few abstractions that use variables, such as:

-   base: @{PROC}/filesystem
-   X: @{HOME}/.ICEauthority r,
-   audio: owner @{HOME}/.pulse-cookie rwk,
-   gnome: @{HOME}/.gtk-bookmarks r,
-   freedesktop.org: @{HOME}/.recently-used.xbel\* rw

Also, there are a few 'deny' rules that aren't strictly required. These
could be hand edited, but the above demonstrates that while the tools
did not generate an optimized profile, they did generate a working
profile for Evolution. While the above works for the test cases given,
it should not be considered a complete profile. Often you need to use
the application for several days or more to be sure there aren't any
profile bugs remaining.

Modifying an existing profile
=============================

-   Monitor the system for AppArmor denials
    -   dmesg, /var/log/kern.log, /var/log/messages, etc
    -   aa-notify
-   Run aa-logprof in a terminal to update the policy, as necessary

**NOTE:** the tools currently don't have a way to skip a rule at the
moment, so the may add in more 'deny' rules than you might want. Also,
the tools don't currently examine the 'Last Modified:' date in the
profile so anything in the current logs since the last rotation will
keep showing up in aa-logprof (which can be a problem if using an
old logfile with a profile that uses abstractions with variable names
that aa-logprof won't recognize). The current workaround is to either
hand edit or extract the logs from where you want aa-logprof to look,
and then use `aa-logprof -f <logfile>`.
