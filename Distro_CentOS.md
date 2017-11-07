Experimental AppArmor on CentOS 5.5, x86\_64
--------------------------------------------

Warning! This is an experimental (RC) version of the kernel with
experimental AppArmor patch in it, use at your own risk!

### Kernel

Currently we ignore the [CentOS-specific
instructions](http://wiki.centos.org/HowTos/Custom_Kernel), we build
a simple vanilla kernel instead.

#### Obtaining

We need GIT. Either grab it from the
[RPMForge](http://wiki.centos.org/AdditionalResources/Repositories/RPMForge)
repository (yum install git) or if that doesn't work, then:

```
 rpm -i http://dag.linux.iastate.edu/dag/redhat/el5/en/x86_64/rpmforge/RPMS/git-1.7.1-3.el5.rf.x86_64.rpm http://dag.linux.iastate.edu/dag/redhat/el5/en/x86_64/rpmforge/RPMS/perl-Git-1.7.1-3.el5.rf.x86_64.rpm
```

Now we can check out the kernel:

```
 mkdir -p ~/apparmor/ && cd ~/apparmor/
 git clone git://git.kernel.org/pub/scm/linux/kernel/git/jj/apparmor-dev.git
 cd apparmor-dev/
 git checkout --track -b AA2.5-2.6.33 origin/AA2.5-2.6.33
```

#### Building

```
 cd ~/apparmor/apparmor-dev/
```

See if we can reuse the existing kernel configuration
(CONFIG\_IKCONFIG=y, CONFIG\_IKCONFIG\_PROC=y):

```
 cp /proc/config.gz ./ && gzip -d config.gz
```

Tweak the kernel, enable AppArmor and CONFIG\_SYSFS\_DEPRECATED\_V2:

```
 yum install ncurses-devel
 make menuconfig
```

“Security options” ---&gt; “AppArmor support”.

**Warning!** To boot CentOS 5.5 we have
to switch on the old init tools support:
[CONFIG\_SYSFS\_DEPRECATED\_V2=Y](http://serverfault.com/questions/108189/kernel-upgrade-centos-5-3-mount-could-not-find-filesystem-dev-root).

#### Installing

```
 cd ~/apparmor/apparmor-dev/
 yum install rpm-build
 make rpm
 rpm -i /usr/src/redhat/RPMS/x86_64/kernel-2.6.33-1.x86_64.rpm
 mkinitrd -f /boot/initrd-2.6.33.img 2.6.33
```

Try booting the new kernel with kexec:

```
 yum install kexec-tools
 kexec -l /boot/vmlinuz-2.6.33 --initrd=/boot/initrd-2.6.33.img --append=“ro root=LABEL=/ noapic”
 kexec -e
```

Edit “/boot/grub/grub.conf” and add:

```
 title AppArmor(2.6.33-1)
   root (hd0,0)
   kernel /vmlinuz-2.6.33 ro root=LABEL=/ noapic
   initrd /initrd-2.6.33.img
```

If this is your first grub.conf title, it makes sense to add
 
```
 fallback=1
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

For CentOS we will be building the AppArmor tools from source.

#### Necessary Perl packages

AppArmor tools depend on these additional Perl packages which we will
let CentOS to maintain:

```
 yum install perl-libxml-perl
```

We also need Term::ReadKey, but it isn't available
in the default CentOS install, perhaps you have it from
[RPMForge](http://wiki.centos.org/AdditionalResources/Repositories/RPMForge)
or some other repository:

```
 yum whatprovides “*/perl(Term::ReadKey)”
 yum whatprovides “*/perl(Locale::gettext)”
 yum whatprovides “*/perl(RPC::XML)”
```

with RPMForge it is:

```
 yum install perl-TermReadKey
 yum install perl-Locale-gettext
 yum install perl-RPC-XML
```

otherwise just grab it from the nearest RedHat repository:

```
 rpm -i http://dag.linux.iastate.edu/dag/redhat/el5/en/x86_64/rpmforge/RPMS/perl-TermReadKey-2.30-3.el5.rf.x86_64.rpm
 rpm -i http://dag.linux.iastate.edu/dag/redhat/el5/en/x86_64/rpmforge/RPMS/perl-Locale-gettext-1.05-1.el5.rf.x86_64.rpm
 rpm -i http://dag.linux.iastate.edu/dag/redhat/el5/en/x86_64/rpmforge/RPMS/perl-XML-Parser-2.36-1.el5.rf.x86_64.rpm
 rpm -i http://dag.linux.iastate.edu/dag/redhat/el5/en/x86_64/rpmforge/RPMS/perl-RPC-XML-0.71-1.el5.rf.noarch.rpm
```

#### Fetch and build

Make sure the necessary build tools and libraries are installed:

 yum install bison gcc-c++ tetex-latex gettext-devel

We need a version of flex with “yypop\_buffer\_state” (the version from “yum install flex” is too old). Grab the fresh version from http://flex.sourceforge.net/:

```
 mkdir -p ~/apparmor/ && cd ~/apparmor/
 wget “http://downloads.sourceforge.net/project/flex/flex/flex-2.5.35/flex-2.5.35.tar.bz2"
 tar -xjf flex-2.5.35.tar.bz2
 cd flex-2.5.35/
 ./configure && make && make install && echo okay
 ```

Fetch a stable tools build [from launchpad](https://launchpad.net/apparmor)

```
 mkdir -p ~/apparmor/ && cd ~/apparmor/
 wget http://launchpad.net/apparmor/2.5/2.5.1/+download/apparmor-2.5.1.tar.gz
 tar -xzf apparmor-2.5.1.tar.gz
```

Build parser:

```
 cd ~/apparmor/apparmor-2.5.1/parser/
 make LEX=/usr/local/bin/flex
 make install
```

Build apparmor utils:

```
 cd ~/apparmor/apparmor-2.5.1/utils/
 make && make install && echo okay
```

Create profiles directory:

```
 cd ~/apparmor/apparmor-2.5.1/profiles/
 make install
```

#### Startup

Manual restart:

```
 /etc/init.d/apparmor restart
```

Automatic startup:

```
 cd /etc/init.d/
 chkconfig --add apparmor
```

#### Checking

The `aa-status` tool now should print a list of known and used
profiles, like this:

```
 apparmor module is loaded.
 25 profiles are loaded.
 25 profiles are in enforce mode.
    /bin/ping
    /sbin/klogd
    /sbin/syslog-ng
    /sbin/syslogd
    /usr/lib/apache2/mpm-prefork/apache2
    /usr/lib/apache2/mpm-prefork/apache2//DEFAULT_URI
    /usr/lib/apache2/mpm-prefork/apache2//HANDLING_UNTRUSTED_INPUT
    /usr/lib/apache2/mpm-prefork/apache2//phpsysinfo
    /usr/lib/dovecot/deliver
    /usr/lib/dovecot/dovecot-auth
    /usr/lib/dovecot/imap
    /usr/lib/dovecot/imap-login
    /usr/lib/dovecot/managesieve-login
    /usr/lib/dovecot/pop3
    /usr/lib/dovecot/pop3-login
    /usr/sbin/avahi-daemon
    /usr/sbin/dnsmasq
    /usr/sbin/dovecot
    /usr/sbin/identd
    /usr/sbin/mdnsd
    /usr/sbin/nmbd
    /usr/sbin/nscd
    /usr/sbin/ntpd
    /usr/sbin/smbd
    /usr/sbin/traceroute
 0 profiles are in complain mode.
 2 processes have profiles defined.
 0 processes are in enforce mode :
 0 processes are in complain mode.
 2 processes are unconfined but have a profile defined.
    /sbin/klogd (2282)
    /sbin/syslogd (2278)
```

#### Tuning logs

Audit data by default is dropped into /var/log/messages via
syslogd. That way, the data is severely capped by the kernel in order
not to overload the messages log. To make audit data usable with
AppArmor we should install auditd and tune it to keep large amounts
of data:

```
 yum install audit
 joe /etc/audit/auditd.conf # num_logs = 2, max_log_file = 200
 /etc/init.d/auditd restart
```
