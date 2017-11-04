What to look for
================

Some times AppArmor causes things to fail. If you suspect AppArmor
is the cause of your problem, there are some basic things to look for.

Is AppArmor enabled?
--------------------

First check AppArmor's status by running the following command as root:

-   aa-status

This will display AppArmor status output similar to the following:

```
    apparmor module is loaded.
    13 profiles are loaded.
    12 profiles are in enforce mode.
       /usr/lib/connman/scripts/dhclient-script
       /usr/share/gdm/guest-session/Xsession
       /usr/bin/evince-previewer
       /usr/sbin/tcpdump
       /usr/lib/cups/backend/cups-pdf
       /usr/bin/evince-thumbnailer
       /sbin/dhclient3
       /usr/bin/evince
       /usr/bin/virt-aa-helper
       /usr/sbin/cupsd
       /usr/sbin/libvirtd
       /usr/lib/NetworkManager/nm-dhcp-client.action
    1 profiles are in complain mode.
       /bin/foobash
    3 processes have profiles defined.
    3 processes are in enforce mode :
       /sbin/dhclient3 (3043) 
       /usr/sbin/libvirtd (2370) 
       /usr/sbin/cupsd (2425) 
    0 processes are in complain mode.
    0 processes are unconfined but have a profile defined.
```

AppArmor is designed to only restrict applications for which there
are profiles defined so if AppArmor is not enabled or no profiles are
loaded it is unlikely AppArmor is causing the problem. While unlikely,
a kernel bug could be present and you may want to check for kernel
Oopses (described below).

While 'aa-status' is the preferred method, you may also run the
following commands as root, with different output:

-   /etc/init.d/apparmor status
-   ls /sys/kernel/security/apparmor
-   cat /sys/module/apparmor/parameters/enabled
-   rcapparmor status \# SuSE only

Messages in the Log files
-------------------------

If AppArmor is enabled, the next thing to check is your system logs
for reject, learning and info messages from AppArmor. AppArmor will
[usually](Message_quieting) output messages when it is interacting
with an application and if there are AppArmor denied messages, this
could be the cause of the problem.

The location of the messages will depend on how your systems auditing
is setup but are usually in one or more of the following locations:

-   /var/log/kern.log
-   /var/log/messages
-   /var/log/audit/audit.log (only if auditd is installed)

A typical denied message looks like the following:

    `type=APPARMOR\_ALLOWED msg=audit(1212396827.141:861): operation=“inode\_unlink” requested\_mask=“w” denied\_mask=“w” name=“/tmp/orbit-jj/linc-e6e-0-2acdd337d87e5” pid=3694 profile=“/usr/bin/foo”`

Notice the 'denied\_mask' in the above log entry. auditd will use
'DENIED' instead. In general, you can use one of (as root):

-   grep -i denied /var/log/kern.log
-   grep -i denied /var/log/messages
-   grep -i denied /var/log/audit/audit.log (if using auditd)

If there are no denied messages from AppArmor, it is unlikely
AppArmor is causing the problem. Please note that AppArmor logs
loading, replacing and removing of policy. The messages for loading
of policy on boot as well as when modifying policy are normal and do
not indicate a problem with your AppArmor policy.

### What if there are no log messages but there should be?

While AppArmor usually logs every reject and learning event, it can be
configured so that auditing of some or all messages are disabled. To
see if this is the case you can examine the module's audit settings
and the profiles.

#### Dropped audit messages

The audit daemon (if installed) or syslog dmesg can drop apparmor
messages if the are occurring at too high of a rate.

##### Adjusting Auditd

???

##### Adjust printk rate limiting

??? how to tell printk is dropping apparmor messages ????

If auditd is not installed then apparmor message dropping is controlled by printk\_ratelimiting. This can be adjusted by changing the value of

` /proc/sys/kernel/printk_ratelimit`

to disable printk ratelimiting you can do

` echo 0 > /proc/sys/kernel/printk_ratelimit`

#### Module audit settings

To view the modules audit mode from a command prompt do

      sudo cat /sys/module/apparmor/parameters/audit

It can return one of the following valuse

-   normal - module auditing is not doing any special overrides
-   quiet\_denied - module is quieting all denied access (reject) messages
-   quiet - module is quieting all messages
-   noquiet - module is overriding profile rules that quiet messages
-   all - output all audit for all permission requests (this includes things that were allowed). This is very noisy if an AppArmor profile is in use.

To change the module audit mode, echo one of the above values into the audit parameter.

```
 sudo sh -c 'echo -n "all" > /sys/module/apparmor/parameters/audit'
```

### Profile audit settings

AppArmor will only audit tasks confined by AppArmor profiles and has
four basic audit modes (enforce, complain, audit, kill).

-   Enforce mode - only status events (profile loads, ...) and events that cause rejects generate audit messages
-   Complain mode - like Enforce mode except it changes the message type from denied to allowed and causes all AppArmor denied events to succeed as if AppArmor was not confining the task (in this mode it is only logging unknown behavior)
-   Audit mode - outputs a log message for each event mediated by the AppArmor module whether it is allowed or rejected
-   Kill mode - outputs a log message for any event that AppArmor denies and then it kills the task

Profiles can contain flags that influence what AppArmor audits using
several methods:

-   Deny rule - any rule with the deny prefix will cause quieting of rejects matching the rule. Typically used when rejects are known and should not be filling up the audit logs. This behavior can be overridden by adding the audit prefix to the deny rule
-   Audit rule - prefixing audit to any rule causes matching events to be logged
-   Complain mode profile. Profiles can be put in complain by:
    -   placing a file in the /etc/apparmor.d/force-complain/ directory - files (symlinks by convention) in this directory matching the profile filename in /etc/apparmor.d will be loaded in complain mode by the AppArmor init scripts
    -   setting the 'complain' flag in the profile (deprecated); e.g. `/bin/foobash flags=(complain) { ...`
    -   loading the profile manually with `apparmor\_parser -C/`

The status of complain mode profiles can be checked by running the
`aa-status` command and examining its output for profiles in complain
or quiet mode. For example:

```
    1 profiles are in complain mode.
      /bin/foobash
```

Examining a task's confinement
------------------------------

In debugging AppArmor problems, it might be useful to look at the
confinement of a process. This can be viewed with either:

-   ps -Z
-   sudo cat /proc/<pid>/attr/current

Eg:

```
    $ ps -Z 2301
    LABEL                             PID TTY      STAT   TIME COMMAND
    /sbin/dhclient                   2301 ?        S      0:00 /sbin/dhclient -d -4
    $ sudo cat /proc/2301/attr/current
    /sbin/dhclient (enforce)
```

Kernel Oopses
-------------

AppArmor prefixes its functions with either apparmor\_ or aa\_. If
functions with these prefixes show up in an oops back trace then
AppArmor may be causing a problem. Please copy or photograph the Oops
message and [submit a bug report](submit_a_bug_report).

If you think the kernel AppArmor code is causing a problem and there
is no Oops, you can try booting with the kernel parameter:

```
 apparmor=0
```

This completely disables AppArmor so that only the initial AppArmor
registration test is run. If your system boots with this flag and
the problem persists then it is not caused by AppArmor.

Debugging an AppArmor problem
=============================

Updating the policy
-------------------

By far, the best course of action is to examine denied messages in
the logs, understand why they occurred, and then adjust the profile
as necessary. To modify policy, you can either (as root):

-   use `aa-logprof`
-   edit the offending profile in /etc/apparmor.d/ directly, then reload the profile with 'apparmor\_parser -r /etc/apparmor.d/<profile>'

In some cases you may need to restart the confined program after adjusting policy.

**IMPORTANT:** while you can add more access and use '-r' to replace
the policy and have it apply to an already confined process, you
cannot remove access by replacing the profile. In this case you need to
(as root):

1.  apparmor\_parser -R /etc/apparmor.d/<profile> \# remove the profile
1.  apparmor\_parser -a /etc/apparmor.d/<profile> \# load (add) the profile
1.  restart the confined application

Put profile in complain mode
----------------------------

If for some reason updating the policy is not an option, putting a
profile into complain mode should cause any confined tasks to act as
if AppArmor was not mediating them, logging policy violations along
the way. To temporarily put a profile in complain mode until the next
reboot or reload of policy (as root):

-   manually reload the profile with 'apparmor\_parser -Cr /etc/apparmor.d/<profile>'

To permanently put a profile in complain mode, choose one of (as root):

-   aa-complain <profile_name>
-   add a symlink to the profile in /etc/apparmor.d/force-complain/ and reload policy

Disabling the profile
---------------------

If for some reason updating the policy is not an option, unloading
the profile from the kernel will cause any previously confined tasks
to run unconfined. To temporarily disable a profile until the next
reboot or reload of policy (as root):

-   unload the profile with 'apparmor\_parser -R /etc/apparmor.d/<profile>'

To permanently disable a profile (as root):

-   unload the profile with 'apparmor\_parser -R <profile_name>'
-   add a symlink to the profile in /etc/apparmor.d/disable/ or remove the offending profile from /etc/apparmor.d/

Profiles can also be removed by performing (as root):

-   apparmor\_parser -R <profile_name>
-   echo -n “<profile_name>” &gt; /sys/kernel/security/apparmor/.remove

Unload all profiles
-------------------

You can also unload all AppArmor policy (not recommended or typically
needed) by performing (as root):

-   /etc/init.d/apparmor teardown \# AppArmor 2.5.1+
-   /etc/init.d/apparmor stop \# AppArmor 2.5 and lower
-   rcapparmor stop \# SuSE only

