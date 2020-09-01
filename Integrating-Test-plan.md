# WARNING: this document is a wip

# Introduction

The following guide is to help users and distros setup integrate and test AppArmor. The test plan has its origins in the integration test plan that [Ubuntu](https://wiki.ubuntu.com/Process/Merges/TestPlans/AppArmor) uses before merging a new version of AppArmor into their distribution.

The test plan is broken into sections, each dealing with a component or interaction that can have interaction with apparmor that may need testing. Unlike the Ubuntu test plan reference Ubuntu specific packaging this guide use the upstream build system, as a generic base. Distros are welcome to edit and or send patches to add Distro specific information or references to their own documentation.

# Sections
* [apparmor userspace](???)
* [apparmor kernel](???)
* [apparmor regression testing](???)
* [dbus](???)
* [snapd](???)
* [lxd](???)
* [lxc](???)
* [libvirt/libvirt-lxc](???)
* [docker.io](????)
* [apache](???)
* [pam](???)
* [gnome](???)
* [KDE](???)
* [systemd](???)
* [R](???)
* [system tools - top, ps, ...](???)
* LSM Stacking - interaction with other LSMs
* [ros2](???)
* 
* other things that have integrated apparmor apis

TODO split apart update/rewrite everything below

Test Plan
Common tests
APPARMOR
Have an up to date Ubuntu Desktop and/or Server VM
Install freshly built packages that are needed for landing and reboot
Eg:
devel: copy_sppa_to_repos --arch=i386,amd64 --include-devel --ppa=ubuntu-security-proposed/ppa apparmor

Verify the system comes up and has networking (dhclient profile)
Verify the output of aa-status. It should report:

many profiles loaded (eg, 20 or more)
many profiles in enforce mode (eg, 20 or more)
0 profiles in complain mode (unless libreoffice-common, apparmor-profiles, or some other special package is installed)
some process should have a profile defined
some process should be in enforce mode (the same number as '4', above)
0 processes in complain mode (unless libreoffice-common, apparmor-profiles, or some other special package is installed)
0 processes are unconfined but have a profile defined
Verify cache files have no errors
18.10 and lower:

$ for i in /etc/apparmor.d/cache /var/cache/apparmor ; do echo "= $i =" ; for j in $i/* ; do echo -n "$j: " ; sudo apparmor_parser -B -r $j && echo pass || echo FAIL ; done ; done | grep FAIL
19.04 and higher:

$ export AACACHEDIR=$(sudo apparmor_parser --print-cache-dir)
$ for i in $(sudo ls -1 "$AACACHEDIR") ; do echo -n "$i: " ; sudo apparmor_parser -B -r "$AACACHEDIR/$i" && echo pass || echo FAIL ; done | grep FAIL
Verified the cache is used
18.10 and lower:

$ sudo rm -rf /etc/apparmor.d/cache/*
$ sudo /sbin/apparmor_parser --remove /etc/apparmor.d
$ sudo /sbin/apparmor_parser --write-cache --add /etc/apparmor.d
$ ls /etc/apparmor.d/cache
sbin.dhclient                            usr.lib.libreoffice.program.xpdfimport
usr.bin.evince                           usr.lib.snapd.snap-confine.real
...
$ sudo /sbin/apparmor_parser --remove /etc/apparmor.d
$ sudo /sbin/apparmor_parser -k --write-cache --add /etc/apparmor.d 2>&1 | grep -i miss # should be no misses
$
19.04 and higher:

$ sudo rm -rf /var/cache/apparmor/*
$ sudo /sbin/apparmor_parser --remove /etc/apparmor.d
$ sudo /sbin/apparmor_parser --write-cache --add /etc/apparmor.d
$ sudo ls /var/cache/apparmor/*
nvidia_modprobe  usr.bin.evince  usr.lib.snapd.snap-confine.real  usr.sbin.cupsd
sbin.dhclient    usr.bin.man     usr.sbin.cups-browsed            usr.sbin.ippusbxd
$ sudo /sbin/apparmor_parser --remove /etc/apparmor.d
$ sudo /sbin/apparmor_parser -k --write-cache --add /etc/apparmor.d 2>&1 | grep -i miss # should be no misses
$
INTEGRATION
Verify Ubuntu Desktop and/or Server works by performing basic login testing - eg, verify networking, verify browser launches, verify apt-get works

QRT
Run QRT/scripts/test-apparmor.py on Ubuntu Desktop/Server (Note: in the exceptional case when there are temporary new expected failures, be sure to update test-apparmor.py for these to not block kernel team processes):

$ git clone https://git.launchpad.net/qa-regression-testing
$ cd qa-regression-testing
$ ./scripts/make-test-tarball ./scripts/test-apparmor.py
# To run, copy /tmp/qrt-test-apparmor.tar.gz to the target system, then do:
$ tar -zxf qrt-test-apparmor.tar.gz
$ cd ./qrt-test-apparmor
$ sudo ./install-packages test-apparmor.py
$ sudo ./test-apparmor.py -v
LXC
Verify lxc container starts with new AppArmor on Ubuntu Desktop/Server:

~$ sudo apt-get install lxc lxc-templates
~$ sudo lxc-create -t ubuntu -n CN  # or: sudo MIRROR=http://<mirror>/ubuntu lxc-create ...
~$ sudo lxc-start -n CN # later versions (eg 15.04)  may not start in a console
...
Ubuntu Trusty Tahr (development branch) CN console

CN login: ubuntu
Password:
...
Run a few external commands:

$ sudo lxc-ls
CN
$ sudo lxc-info --name CN
Name:           CN
State:          RUNNING
PID:            24354
IP:             10.0.3.153
CPU use:        1.80 seconds
BlkIO use:      12.18 MiB
Memory use:     20.58 MiB
KMem use:       0 bytes
Link:           vethYD8QMX
 TX bytes:      2.90 KiB
 RX bytes:      6.77 KiB
 Total bytes:   9.67 KiB
$ sudo lxc-console --name CN
Connected to tty 1
Type <Ctrl+a q> to exit the console, <Ctrl+a Ctrl+a> to enter Ctrl+a itself

Ubuntu Utopic Unicorn (development branch) CN tty1

CN login:
...
$ sudo lxc-attach --name CN uptime
 22:29:49 up  1:10,  1 user,  load average: 0.06, 0.31, 0.58
Verify apparmor profiles are in use:

$ sudo aa-status | grep ') lxc'
   /usr/sbin/agetty (871) lxc-container-default-cgns
   /usr/lib/systemd/systemd (32182) lxc-container-default-cgns
...
When done, shut it down with (outside the container (tests lxc-start still works to control the container)):

$ sudo lxc-stop -k -n CN
Verify apparmor profiles are no longer in use:

$ sudo aa-status | grep ') lxc'
$
And, finally, destroy it:

$ sudo lxc-destroy -n CN
LXD
Verify lxd container starts with new AppArmor on Ubuntu Desktop/Server:

deb (deprecated on 18.10 and higher):

$ sudo apt-get install lxd
$ newgrp lxd
snap:

$ sudo apt-get remove --purge lxd lxc
$ sudo snap install lxd
$ sudo adduser `id -un` lxd
$ newgrp lxd
$ sudo lxd init # use defaults
Once lxd is installed:

$ . /etc/profile.d/apps-bin-path.sh # in case /snap/bin is not in your PATH
$ lxc image list images:
...
|                                 | 0d4bfe75bd0d | yes    | Ubuntu trusty (amd64) (20160321_03:49)    | x86_64  | 75.60MB  | Mar 21, 2016 at 4:19am (UTC)  |
...

$ lxc launch ubuntu: ubuntu-64
...
Creating ubuntu-64
Retrieving image: 100%
Starting ubuntu-64

$ lxc list
+-----------+---------+-------------------+------+------------+-----------+
|   NAME    |  STATE  |       IPV4        | IPV6 |    TYPE    | SNAPSHOTS |
+-----------+---------+-------------------+------+------------+-----------+
| ubuntu-64 | RUNNING | 10.0.3.181 (eth0) |      | PERSISTENT | 0         |
+-----------+---------+-------------------+------+------------+-----------+

$ lxc info ubuntu-64
Name: ubuntu-64
Architecture: i686
Created: 2016/03/22 17:55 UTC
Status: Running
Type: persistent
Profiles: default
Pid: 2612
Processes: 8
Ips:
  eth0: inet    10.0.3.181      vethIKDBKR
  eth0: inet6   fe80::216:3eff:fe88:59b7        vethIKDBKR
  lo:   inet    127.0.0.1
  lo:   inet6   ::1

$ sudo aa-status | grep ') lxd-ubuntu-64'
   /usr/lib/systemd/systemd (14348) lxd-ubuntu-64_</var/snap/lxd/common/lxd>//&:lxd-ubuntu-64_<var-snap-lxd-common-lxd>:unconfined
   /usr/lib/systemd/systemd-journald (14464) lxd-ubuntu-64_</var/snap/lxd/common/lxd>//&:lxd-ubuntu-64_<var-snap-lxd-common-lxd>:unconfined

$ lxc exec ubuntu-64 /bin/bash
root@ubuntu-64:~# ls
root@ubuntu-64:~# uptime
 17:58:40 up 3 min,  0 users,  load average: 0.01, 0.06, 0.05
root@ubuntu-64:~# aa-status # AppArmor stacking works
apparmor module is loaded.
15 profiles are loaded.
15 profiles are in enforce mode.
   /sbin/dhclient
   /usr/bin/lxc-start
...
root@ubuntu-64:~# exit

$ lxc exec ubuntu-64 ps
  PID TTY          TIME CMD
 1552 ?        00:00:00 ps

# pull/push files
$ lxc file pull ubuntu-64/etc/hostname .
$ lxc file push hostname ubuntu-64/tmp/hostname
$ lxc exec ubuntu-64 -- cat /tmp/hostname
ubuntu-64

$ lxc stop ubuntu-64

$ sudo aa-status | grep ') lxd-ubuntu-64' # profiles unloaded
$

$ lxc delete ubuntu-64
LIBVIRT
Install libvirt and ensure that the libvirtd group is part of the current session:

$ sudo apt-get install libvirt-bin # libvirt-daemon on more recent Ubuntu releases
$ newgrp libvirtd
Follow setup instructions in $QRT/notes_testing/libvirt/README
Verify qemu/kvm libvirt VMs start under confinement (verify with sudo aa-status) with new AppArmor on Ubuntu Desktop/Server by using QRT/scripts/test-libvirt.py (note: there are some failures unrelated to apparmor, so do a baseline run before upgrading to compare)

Verify libvirt-lxc VMs start with new AppArmor on Ubuntu Desktop/Server by following SergeHallyn_libvirtlxc

IMPORTANT: The instructions linked to above will not work until bug #1445611 is fixed.

DOCKER
Verify docker.io (need at least 1.2) containers with new AppArmor on Ubuntu Desktop/Server:

$ sudo apt-get install docker.io # should not have libvirt or lxc co-installed

$ sudo docker pull ubuntu:bionic
...
6e1bee0f8701: Pull complete 
Digest: sha256:d019bdb3ad5af96fa1541f9465f070394c0daf0ffd692646983f491ce077b70f
Status: Downloaded newer image for ubuntu:bionic

$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              bionic              94e814e2efa8        44 hours ago        88.9MB

$ sudo docker run ubuntu:bionic uptime
 20:31:21 up 1 min,  0 users,  load average: 0.09, 0.06, 0.03
...

$ sudo aa-status|grep docker
   docker-default

$ sudo docker run -i -t ubuntu:bionic /bin/sh
# ps
  PID TTY          TIME CMD
    1 ?        00:00:00 sh
    7 ?        00:00:00 ps
At this point, an interactive shell is running in the terminal. In another, try a couple of operations:

$ sudo aa-status|grep docker
   docker-default
   /usr/bin/dash (24201) docker-default

$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
3acc3d52bca2        ubuntu:bionic       "/bin/sh"           43 seconds ago      Up 42 seconds                           nostalgic_ramanujan

$ ps -Z 24201
LABEL                             PID TTY      STAT   TIME COMMAND
docker-default (enforce)        24201 pts/0    Ss+    0:00 /bin/sh

$ sudo docker inspect 3acc3d52bca2
[
    {
        "Id": "3acc3d52bca28aa40da695781bb7a1195c3c4d5d821c350e272f4ed9d8582271",
        "Created": "2019-03-13T20:10:33.671935431Z",
        "Path": "/bin/sh",
        "Args": [],
        "State": {
            "Status": "running",
...
In the terminal running 'sh', now exit:

# exit

$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

$ sudo aa-status|grep docker
   docker-default
SNAPD
Verify snappy works ok:

$ sudo apt-get install snapd
$ sudo snap install hello-world

$ sudo aa-status | grep snap
   /snap/core/6623/usr/lib/snapd/snap-confine
   /snap/core/6623/usr/lib/snapd/snap-confine//mount-namespace-capture-helper
   /usr/lib/snapd/snap-confine
   /usr/lib/snapd/snap-confine//mount-namespace-capture-helper
   snap-update-ns.core
   snap-update-ns.hello-world
   snap.core.hook.configure
   snap.hello-world.env
   snap.hello-world.evil
   snap.hello-world.hello-world
   snap.hello-world.sh

$ . /etc/profile.d/apps-bin-path.sh # in case /snap/bin is not in your PATH
$ hello-world.evil
Hello Evil World!
This example demonstrates the app confinement
You should see a permission denied error next
/snaps/hello-world.canonical/6.0/bin/evil: 9: /snaps/hello-world.canonical/6.0/bin/evil: cannot create /var/tmp/myevil.txt: Permission denied

$ hello-world.sh
Launching a shell inside the default app confinement. Navigate to your
app-specific directories with:

  $ cd $SNAP
  $ cd $SNAP_DATA
  $ cd $SNAP_USER_DATA

bash-4.3$ 
cat /etc/fstab
cat: /etc/fstab: Permission denied
bash-4.3$ exit

$ sudo snap install snappy-debug
$ snappy-debug.scanlog --only-snap=hello-world
= AppArmor =
Time: Mar 13 15:17:16
Log: apparmor="DENIED" operation="mknod" profile="snap.hello-world.evil" name="/var/tmp/myevil.txt" pid=28941 comm="evil" requested_mask="c" denied_mask="c" fsuid=1000 ouid=1000
File: /var/tmp/myevil.txt (write)

= AppArmor =
Time: Mar 13 15:17:42
Log: apparmor="DENIED" operation="open" profile="snap.hello-world.sh" name="/etc/fstab" pid=29215 comm="cat" requested_mask="r" denied_mask="r" fsuid=1000 ouid=0
File: /etc/fstab (read)

$ sudo snap remove hello-world

$ sudo ls /var/cache/apparmor/* | grep hello-world # cache files are removed
$
Desktop only
DBUS
Run QRT/scripts/test-dbus.py on Ubuntu Desktop:

$ ./scripts/make-test-tarball ./scripts/test-dbus.py
# To run, copy /tmp/qrt-test-dbus.tar.gz to the target system, then log in through a graphical session and do:
$ tar -zxf qrt-test-dbus.tar.gz
$ cd ./qrt-test-dbus
$ sudo ./install-packages test-dbus.py
$ sudo ./test-dbus.py -v
# References
* [Ubuntu test plan](https://wiki.ubuntu.com/Process/Merges/TestPlans/AppArmor)
* Suse
* Debian
