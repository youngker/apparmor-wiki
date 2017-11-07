In Stock Debian
---------------

AppArmor should be available out of the box in the latest Debian
distros. Please see <http://wiki.debian.org/AppArmor>

To enable the AppArmor in the Debian kernel, add
“security=apparmor” to the kernel parameters, like this:

```
 sed -i -e 's/GRUB_CMDLINE_LINUX_DEFAULT="/&security=apparmor /' /etc/default/grub
```

This sed command results in the following
[/etc/default/grub](http://www.gnu.org/software/grub/manual/grub.html#Configuration)
line on my system:

```
 GRUB_CMDLINE_LINUX_DEFAULT=“security=apparmor quiet”
```

Then run

```
 update-grub
```

Experimental AppArmor on Debian Jessie amd64
--------------------------------------------

### Kernel

#### Obtaining

```
 mkdir -p ~/apparmor/ && cd ~/apparmor/
 wget https://www.kernel.org/pub/linux/kernel/v3.x/linux-3.10.2.tar.xz
 tar -xJf linux-3.10.2.tar.xz
 cd linux-3.10.2/
```

#### Building

```
 cd ~/apparmor/linux-3.10.2/
```

See if we can reuse the existing kernel configuration
(CONFIG\_IKCONFIG=y, CONFIG\_IKCONFIG\_PROC=y):

```
 cp /proc/config.gz ./ && gzip -d config.gz
```

Tweak the kernel, enable AppArmor:

```
 apt-get install libncurses-dev
 make menuconfig
```

“Security options” ---&gt; “AppArmor support”, “Enable AppArmor 2.4 compatability”

#### Installing

```
 aptitude install dpkg-dev bc
 cd ~/apparmor/linux-3.10.2/
 make deb-pkg
 dpkg -i ../linux-firmware-image_{version}.deb
 dpkg -i ../linux-headers-{version}.deb
 dpkg -i ../linux-image-{version}.deb
```

If the kernel is installed on another host, then symlinks for DKMS should be fixed.

```
 rm /lib/modules/{version}/build; ln -s /usr/src/linux-headers-{version} /lib/modules/{version}/build
 rm /lib/modules/{version}/source; ln -s /usr/src/linux-headers-{version} /lib/modules/{version}/source
```

About `dpkg -i ../linux-libc-{version}.deb`:
*/usr/include/x86\_64-linux-gnu/asm* seems to be missing from latest
linux-libc-{version}.deb. If you installed linux-libc-{version}.deb,
you can downgrade to Debian version with `aptitude install linux-libc-dev=3.0.0-3`.

Finally:

```
 update-grub
```

#### Checking

Reboot under new kernel:

```
 /sbin/shutdown -r now
```

or

```
 reboot
```

Now see if AppArmor is loaded and enabled (should print “Y”):

```
 cat /sys/module/apparmor/parameters/enabled
```

### Tools

```
 aptitude install apparmor apparmor-profiles
 /etc/init.d/apparmor restart
```

#### Checking

```
 cat /var/log/audit/audit.log | grep apparmor_parser
```

should display something like

```
 type=AVC msg=audit(1316949034.097:108): apparmor=“STATUS” operation=“profile_load” name=“/bin/ping” pid=5207 comm=“apparmor_parser”
```

#### Tuning logs

Audit data by default is dropped into /var/log/messages via
rsyslogd. That way, the data is severely capped by the kernel in
order not to overload the messages log. To make audit data usable
with AppArmor we should install auditd and tune it to keep large
amounts of data:

```
 apt-get install auditd
 sed -i -re 's/max_log_file = [0-9]+/max_log_file = 200/' /etc/audit/auditd.conf
 /etc/init.d/auditd restart
```
