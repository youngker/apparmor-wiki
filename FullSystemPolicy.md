Introduction
============

AppArmor is often deployed using targeted policy where you apply
policy for applications that are considered security sensitive. On
these systems, most processes (including 'init') are running under the
'unconfined' profile with the security sensitive applications running
under their own profile.

AppArmor is also capable of being used for full system policy
where processes are by default not running under the 'unconfined'
profile. This might be useful for high security environments or
embedded systems.

Implementation
==============

Concepts
--------

Booting a Linux system consists of:

1.  telling the bootloader where to find the kernel and executing it
2.  optionally telling the kernel where to find an initramfs archive
3.  telling the kernel where to find the 'init' process and executing it

When booting an initramfs:

1.  the kernel extracts the filesystem in the initramfs archive into a memory filesystem then executes '/init' within the initramfs
2.  this will execute scripts, etc based on your distribution
3.  eventually, this will execute /sbin/init (or whatever you passed to init= on the kernel command line) and this is what is assigned PID 1

The idea is to integrate with the scripts that '/init' executes in
the initramfs environment to load policy into the kernel that will
apply to PID 1 on the system. Confining processes within the initramfs
environment is out of scope of this document.

To load policy from within the initramfs, you need at a minimum
3 things:

1.  a statically compiled apparmor\_parser
2.  securityfs mounted in the initramfs
3.  policy to load

In short, something like the following needs to happen in the
initramfs:

```
mount -t securityfs none /sys/kernel/security
echo “/sbin/init flags=(complain) {}” | /sbin/apparmor_parser -a
```

Distribution
------------

### Ubuntu

<https://wiki.ubuntu.com/Initramfs> has good information on the Ubuntu
initramfs setup.

1.  Copy a statically linked /sbin/apparmor\_parser into the initramfs
    (apparmor\_parser is statically linked by default in Ubuntu)
    by creating /etc/initramfs-tools/hooks/apparmor

    ```
#!/bin/sh
set -e
PREREQ=
prereqs()
{
    echo "$PREREQ"
}
case $1 in
    prereqs)
        prereqs
        exit 0
        ;;
esac
. /usr/share/initramfs-tools/hook-functions
copy_exec /sbin/apparmor_parser /sbin
    ```

2.   Make it executable:

     ```
     $ sudo chmod 755 /etc/initramfs-tools/hooks/apparmor
     ```

3.   Add a script to load policy into the kernel to
     /etc/initramfs-tools/scripts/init-bottom/apparmor (it could
     be somewhere else besides init-bottom, this just happens right
     before the real init is started):

     ```
     #!/bin/sh
     PREREQ=""
     prereqs()
     {
             echo "$PREREQ"
     }
     case $1 in
     prereqs)
             prereqs
             exit 0
             ;;
     esac

     mount -t securityfs none /sys/kernel/security
     echo "profile init-systemd /lib/systemd/systemd flags=(complain) {}" | /sbin/apparmor_parser -a
     echo "profile init-upstart /sbin/upstart flags=(complain) {}" | /sbin/apparmor_parser -a
     ```

4.   Make it executable:

     ```
     $ sudo chmod 755 /etc/initramfs-tools/scripts/init-bottom/apparmor
     ```

5.   Update the initramfs for the running kernel:

     ```
     $ sudo update-initramfs -u
     ```

6.   Reboot

While not explicitly tested on Debian, the above should also work on
Debian-based distributions.

Finishing up
------------

The above creates two stub profiles: one for systemd and one for
upstart (pick whichever you need for your system). On boot, these
are loaded prior to the systemd/upstart starting. You can verify
upon login with `ps auxZ|grep -v unconfined` that all non-kernel
processes are running under this profile.

At this point, create a real profile for your init process in
/etc/apparmor.d/init-systemd with whatever exec transitions make
sense for your environment:

```
profile init-systemd /lib/systemd/** flags=(complain) {
  ...
  /lib/systemd/* ix,
  /{,usr/}{,s}bin/* pix,
  /opt/** Px
  ...
}
```

Now, you can define the policy for each of the processes on your system
like normal (though you probably want to avoid Ux and ux rules). When
you are satisfied, add the 'init' policy to the initramfs using the
above techniques and load it instead of the stub policy so init is
fully confined upon launch.

Troubleshooting
===============

When working with full system policy, it can be easy to lock yourself
out of the system. Some troubleshooting techniques are:

-   boot with 'apparmor=0' on the kernel command line, make your
    changes, then reboot. This is quite helpful if your system policy
    locks you out of the system

-   adjust your initrd policy to use 'flags=(complain)', then
    reboot. This will allow you to see what would be denied by your
    initrd policy.

-   `ps Z` (eg, `ps auxZ`) is very handy to see at a glance what
    AppArmor label processes are running under. Since kernel processes
    typically are of the form `[something]` and they all show up as
    'unconfined', it might by useful to use `ps auxZ | grep -v '.\* $'` instead

-   examining the initramfs is useful and can be done with something like:

```
$ mkdir initramfs
$ cd initramfs
$ cat /path/to/your/initramfs | gunzip -cd | cpio -i
```

-   It is currently not possible to start a process under one label and
    then apply policy to it with a different label, but it is possible
    to start a process under a profile and then to replace the policy
    of that profile to have it apply to the running process. It can
    be useful to use stub policy for PID 1 that is replaced later in
    the boot process. Obviously, this means PID 1 and any processes
    it launches are running under this stub policy rather than full
    policy until the replacement occurs.

Caveats
=======

-   fd delegation won't work correctly when passing file descriptors
    from differently labelled processes (only 'unconfined -&gt;
    unconfined', 'unconfined -&gt; confined' and 'confined
    -&gt; same confined' currently works correctly). This is
    [LP \#1381706](https://launchpad.net/bugs/1381706) .

Further reading
===============

-   [Integrating AppArmor with PAM for login-based policies](Pam_apparmor_example)
-   [Using AppArmor for Role Based Access Control (RBAC)](AppArmorRBAC)
-   [Using AppArmor for Multilevel security (MLS)](AppArmorMLS)

