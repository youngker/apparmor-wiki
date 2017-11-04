AppArmor status
===============

AppArmor protection status can be inspected using both high-level
and low-level tools. The high-level tools use the low-level tools
to gather information, and most of the low-level tools require an
unconfined 'root' (uid 0) process. It may be possible to confine
these processes with file and capability privileges.

High level view
---------------

The 'aa-status' tool gives a high level status of AppArmor and applications it has profiles for (as root):

```
 # aa-status 
 apparmor module is loaded.
 11 profiles are loaded.
 11 profiles are in enforce mode.
    /usr/lib/connman/scripts/dhclient-script
    /usr/share/gdm/guest-session/Xsession
    /usr/bin/googleearth
    /usr/bin/evince-previewer
    /usr/sbin/tcpdump
    /usr/lib/cups/backend/cups-pdf
    /usr/bin/evince-thumbnailer
    /sbin/dhclient3
    /usr/bin/evince
    /usr/sbin/cupsd
    /usr/lib/NetworkManager/nm-dhcp-client.action
 0 profiles are in complain mode.
 2 processes have profiles defined.
 2 processes are in enforce mode :
    /usr/sbin/cupsd (1192) 
    /sbin/dhclient3 (22378) 
 0 processes are in complain mode.
 0 processes are unconfined but have a profile defined.
```

Realtime information
--------------------

Average desktop users may want to run the apparmor\_notify daemon which
displays a handy notification message similar to other notifications
on the system in whatever manner is most consistent with the window
manager/desktop environment. apparmor\_notify knows how to handle
the details below and does some intelligent rate limiting to avoid
notification floods.

Depending on your system's configuration of auditd and syslog, as
well as individual profile configuration, finding out about AppArmor
denials may be difficult. If you are running auditd, check the audit
logs. auditd will consume AppArmor denial messages before the more
common syslog logging mechanism, so be sure to check for auditd
when you're confused. auditd isn't a lossy mechanism, but it can be
inconvenient to work with. Consult its documentation for details.

The dmesg command will dump the kernel message buffer to standard out;
it is a simple ring-buffer, and potentially an AppArmor denial message
may be over-written by the time you try to find it. Also, the kernel
may perform rate limiting which could cause AppArmor denials to be
discarded. When debugging or developing a profile and not using auditd,
it is recommending you disable kernel rate limiting with (as root):

```
    # sysctl -w kernel.printk_ratelimit=0
```

Most systems configure syslog to read kernel messages and save them
into /var/log/messages, /var/log/kern.log, or some other similar
location. syslog also may use a ring buffer, but with much longer lag
times. Because syslogd is a process and reads from a kernel-supplied
ring buffer, there is a chance that under significant system load the
kernel's ring buffer may overwrite AppArmor denial messages before the
logging daemon can read it. You may also lose log messages when logging
to a remote host using the traditional UDP remote logging mechanism.

libapparmor supplies (or will supply) log-parsing tools that can
understand all the logging formats in order to make writing tools
simpler. It is planned that these tools will include desktop and
enterprise notifications to help save time and frustration when
debugging strange problems. For now, using 'dmesg' is the preferred
method when verifying if AppArmor is the cause of a misbehaving
confined application. See [AppArmor\_Failures](AppArmor_Failures
"wikilink") for details.

Information on running processes
--------------------------------

To discover which processes are confined, you can either use
'aa-status' as described above, or alternatively use the 'ps' command
(as root):

```
 # ps aux -Z | grep -v unconfined
 LABEL              USER   PID %CPU %MEM    VSZ   RSS TTY STAT START   TIME COMMAND
 /usr/sbin/cupsd    root  1192  0.0  0.0  75148  1260 ?   Ss   Jan15   0:00 /usr/sbin/c...
 /sbin/dhclient3    root 22378  0.0  0.0   6464  1088 ?   S    Jan23   0:00 /sbin/dhcli...
```

The raw data is also available via /proc (as root):

```
 # grep -L unconfined /proc/*/attr/current
 /proc/1192/attr/current
 /proc/22378/attr/current
```

Most sites that deploy AppArmor are especially interested in confining
applications that use the network. Because this is so common, the
aa-unconfined tool can help system administrators discover network
servers that may or may not be confined (as root):

```
 # aa-unconfined 
 883 /usr/sbin/avahi-daemon not confined
 883 /usr/sbin/avahi-daemon not confined
 1192 /usr/sbin/cupsd confined by '/usr/sbin/cupsd (enforce)'
 12375 /usr/sbin/pdns_recursor not confined
 12375 /usr/sbin/pdns_recursor not confined
 22378 /sbin/dhclient3 (/sbin/dhclient) confined by '/sbin/dhclient3 (enforce)'
 27260 /usr/sbin/dictd (dictd 1.11.1: 1/275) not confined
```

aa-unconfined uses netstat -nlp output to find programs accepting
connections and checks the /proc/<pid>/attr/current files of
each to discover if each is confined or not. aa-unconfined has a
--paranoid option that is probably too paranoid --- it shows the
status of every process on the system. (Which was useful before ps Z
support was available everywhere). A future release could support a
--mostly-paranoid option to search every process with an open network
socket, not just listening sockets.

Information from the kernel module
----------------------------------

AppArmor supports the kernel-standard securityfs mechanism; securityfs
is normally mounted on /sys/kernel/security, and the apparmor module
populates /sys/kernel/security/apparmor with a few control and
information files. The informational files are:

```
 $ ls -1 /sys/kernel/security/apparmor/
 features
 matching
 profiles
```

'features' and 'matching' provides feature and version information
that is used by apparmor\_parser to compile profiles for the running
kernel. 'profiles' provides a list of the profiles loaded into the
kernel as well as the per-profile enabled/complain/audit information.

Information from the persistent configuration files
---------------------------------------------------

You may also be curious about the profiles that AppArmor will load
on next boot. /etc/apparmor.d/ contains all the profiles, as well as
profile 'chunks' that are commonly shared between profiles. Some of
these 'chunks' are abstractions, which are essentially libraries of
access rules. On boot, the AppArmor init scripts will examine and load
the profiles in /etc/apparmor.d/, ignoring certain files such as vim
swap files and package manager generated files as well as profiles that
have a symbolic link in /etc/apparmor.d/disable/. Files with a symbloic
link in /etc/apparmor.d/force-complain/ are forced into complain mode.

If you want to verify the contents of a profile --- say, you want to
make sure the abstractions that are included in the profile do not
allow some ridiculous permissions --- you can invoke:

```
    $ apparmor_parser -Q --debug /etc/apparmor.d/usr.bin.firefox | head -10
    ----- Debugging built structures -----
    Name:       /usr/lib/firefox-4.0b7/firefox{,*[^s][^h]}
    Profile Mode:   Enforce
    Capabilities: net_bind_service
    --- Entries ---
    Mode:   r:r Name:   (/)
    Mode:   r:r Name:   (/**/)
    Mode:   rx:rx   Name:   (/bin/bash)
    Mode:   rx:rx   Name:   (/bin/dash)
    Mode:   rx:rx   Name:   (/bin/grep)
```

This listing shows the permissions granted when the user owns the
resource (file, directory, pipe, etc.) and when the user does not
own the resource.

Additionally, you can see the complete profile with all abstractions include with:

```
    $ apparmor_parser -p /etc/apparmor.d/usr.bin.firefox
    ...
    ##included <abstractions/audio>
    /dev/admmidi*   rw,
    /dev/adsp*      rw,
    /dev/aload*     rw,
    /dev/amidi*     rw,
    /dev/audio*     rw,
    /dev/dmfm*      rw,
    ...
```
