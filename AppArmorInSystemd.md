WARNING
=======

This documentation is a wip, and currently is just a dumping ground
to collect bits and pieces of info in one place.

Introduction
============

Loading apparmor policy
=======================

Systemd v246 added the ability to load apparmor policy cache during early boot.
Requirements
- cache must be in a location available during early boot, eg. /etc/apparmor.d/cache or /lib/apparmor/cache.  Cache in /var/cache/apparmor/ can NOT be used.
- cache for the specific kernel being booted must be compiled before boot/reboot into said kernel
- ???? setup


The restart problem
===================

Restarting the apparmor unit should be avoided.

Systemd handles restart as a stop followed by a start. Unfortunately
this removes all apparmor policy from the kernel and results in all
tasks entering the unconfined state. The start will then load new
policy however all tasks in the system will remain unconfined, only
tasks start after the start operation will gain the new confinement.

Using systemd to set the apparmor profile/label
===============================================

It is possible to have systemd specify the apparmor confinement a
service should start with. To do this the following stanza is added
to the systemd unit file

```
 AppArmorProfile=<confinement>
```

where <confinement> is any profile name, or stack of profile names.

```
 AppArmorProfile=/bin/foo
```

or

```
 AppArmorProfile=:ns1://unconfined
```

or

```
 AppArmorProfile=/bin/foo//&bar
```

or

```
 AppArmorProfile=/bin/foo//&:ns1://unconfined
```

Care must be taken that the referenced profile(s) have already been
loaded into the kernel, and if a policy namespace is specified or
the unit will fail. The stanza can be proceeded by a **-** to ignore
failures in setting the profile confinement.

```
 AppArmorProfile=-bad_profile
```

If apparmor is not enabled the AppArmorProfile stanza will be ignored
and the unit will proceed as if AppArmorProfile was never specified.

[|systemd manual](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#)




Notes from twb figure out where to put them

 ```
#!/usr/bin/python3

# GOAL: systemd provides a large number of handy lockdown options, but
#       they are all opt-in on a per-unit basis.
#       There's no way to apply lockdown HOST-WIDE, and then
#       opt-out in the handful of units that break.
#
#       As a quick-and-dirty workaround, enumerate all the units we can find, and patch them.
#       It ends up being something like this:
#
#       * add [Service] BlockBadThing=yes to every .service unit, the
#       * add [Service] BlockBadThing=no to just bad-thinger.service and bad-thinger-helper.sevice
#
#       BENEFIT: if some new unit appears and doesn't opt out,
#       it will FAIL SAFE (not FAIL OPEN).
#       If that breaks the daemon, users will complain and
#       we'll make an INFORMED DECISION about whether to whitelist it.
#
# NOTE: we run as a one-off, BEFORE systemd is up, so
#
#       * we can't "systemctl list-units --all";
#       * we can't "systemctl list-unit-files --all";
#       * we can't see any units created in /run by generators,
#         including anything generated from sysvinit units.

import argparse
import fnmatch                  # WARNING: **NOT IDENTICAL** to fnmatch(3)
import glob
import os
import subprocess

_OUR_FILENAME = 'prisonpc-systemd-lockdown.conf'
_OUR_RULES = [
    ('[Service]', ['*.service']),

    ######################################################################
    ### ProtectSystem=yes    — ro /usr, /boot.
    ### ProtectSystem=full   — ro /usr, /boot, /etc.
    ### ProtectSystem=strict - ro / & rw /proc, /sys, /dev
    ### ReadWritePaths=/opt  - rw access to /opt — path must EXIST IN ADVANCE,
    ###                        e.g. /var/log but not /run/otherdaemon or /dev/foo!
    ###                        Can be "stacked" over multiple lines, like ExecStart=.
    ## Version #1: this works!
    # ('ProtectSystem=yes', ['*.service']),
    ## Version #2: this works! (NOTE: =full is more restrictive than =yes).
    # ('ProtectSystem=full', ['*.service']),
    # ('ProtectSystem=yes', [
    #     # Xsetup needs write access to /etc/groups.
    #     # session-snitch needs write access to /dev/watchdog.
    #     'xdm.service', 'display-manager.service',
    #     # Creates /etc/ssh/ssh_host_*_key at boot time, to make SSH work.
    #     'keygen.service']),
    ## Version #3: this works! (NOTE: =strict is more restrictive than =full).
    ('ProtectSystem=strict', ['*.service']),
    # tmpfiles tweaks files in many places, including e.g. /forcefsck and /etc/mtab.
    # Therefore only restrict its access to /usr and /boot.
    ('ProtectSystem=yes', ['systemd-tmpfiles-setup.service']),
    # ProtectSystem=<anything> breaks /run/user/12345 by making systemd-logind's tmpfs mount (on that path) invisible in other namespaces (such as xdm).
    # FIXME: in systemd v239+, this moves to user-runtime-dir@.service, and systemd-logind can be locked down.
    ('ProtectSystem=no', ['systemd-logind.service']),
    ('ProtectSystem=full', [
        # FIXME: determine *why* these break with =strict.
        # FIXME: instead of downgrading to =full, whitelist specific ReadWritePaths=.
        'systemd-journald.service',
        'systemd-udevd.service', 'udevd.service',
        'udisks2.service',
    ]),
    ('ReadWritePaths=/run/', [
        'systemd-user-sessions.service',  # for /run/nologin
        'unscd.service',    # for /run/nscd/socket & /run/.nscd_socket
        'kmod-static-nodes.service',  # for /run/tmpfiles.d/kmod.conf
        'nfs-config.service',         # for /run/sysconfig/nfs-utils
        'rpcbind.service',            # for /run/rpcbind.lock
        'rsyslog.service', 'syslog.service',  # for /run/rsyslogd.pid
        'ssh.service',  # for /run/sshd.pid (FIXME: PidFile=none in /etc/ssh/sshd_config)
        'systemd-update-utmp-runlevel.service',  # for /run/utmp
        'systemd-update-utmp.service',           # for /run/utmp
    ]),
    ('ReadWritePaths=/etc/ssh/', [
        'keygen.service']),  # for /etc/ssh/ssh_*_host_key
    ('ReadWritePaths=/var/log/', [
        'systemd-update-utmp.service',  # for /var/log/wtmp
        'systemd-update-utmp-runlevel.service',  # for /var/log/wtmp
    ]),
    # NOTE: because we disable systemd-logind, PROBABLY any lockdown
    # we apply to xdm.service will also implicitly affect EVERYTHING
    # the inmate user does.
    #
    # We disable systemd-logind because (apart from anything else) it
    # allows persistent "cron job" equivalents in
    # ~p123/.config/systemd/user, which make us nervous.  Maybe we can
    # defeat SOME of that without disabling it entirely?  Is the win
    # big enough to even try?
    # FIXME: I tried to specifically whitelist /dev/watchdog and /etc/group, and I got back this error:
    #           systemd[473]: xdm.service: Failed at step NAMESPACE spawning /usr/bin/xdm: No such file or directory
    #        possibly this is because xdm.service starts before dev-watchdog.device is created.
    # FIXME: use PrivateTmp=yes ?  Possibly derps with user session being "under" xdm.
    #           xdm[523]: (EE) Could not create lock file in /tmp/.tX0-lock
    # THIS unit is starting to sound like a job for Apparmor:
    #           xdm[499]: (EE) Cannot open log file "/var/log/Xorg.0.log"
    #           xdm error (pid 541): cannot make authentication directory /var/lib/xdm/authdir: Read-only file system
    ('ReadWritePaths=/etc/ /dev/ /run/ /tmp/ /var/log /var/lib/xdm', ['xdm.service', 'display-manager.service']),
    ## FIXME: in /etc/X11/xdm/xdm-config set "DisplayManager.pidFile: /run/xdm/xdm.pid"
    ##        in xdm.service put RuntimeDirectory=xdm
    ##        in xdm.service remove ReadWritePaths=/run/
    ('RuntimeDirectory=xdm', ['xdm.service', 'display-manager.service']),

    ######################################################################
    ### ProtectHome=yes       — /home is empty
    ### ProtectHome=read-only — /home is ro
    ### ProtectHome=tmpfs     — /home is empty AND ro???
    ###
    ### FIXME: this has some kind of race condition in systemd-udev-trigger.service, which breaks EVERYTHING.
    ###
    ###          Process: 421 ExecStart=/bin/udevadm trigger –type=subsystems –action=add (code=exited, status=226/NAMESPACE)
    ###
    ###        The only things we're doing are ProtectSystem=strict ProtectHome=read-only.
    ###        Perhaps /home doesn't exist when that unit runs?
    ('ProtectHome=yes', ['*.service']),
    ('ProtectHome=no', [
        'systemd-tmpfiles-setup.service',  # it wants to chmod 755 /home itself (we prefer 711).
        'ssh.service',                     # for CLI users
        'xdm.service', 'display-manager.service',  # for GUI users
        'systemd-logind.service',  # for /run/user/0 (mount & umount)
        # FIXME: some kind of race condition makes ProtectHome=yes get a 226/NAMESPACE error here.  Why?
        'systemd-udev-trigger.service',
    ]),

    # With this, the GUI is "working", but we're seeing this suckiness in ~s123/.xsession-errors:
    #
    #     fuse: bad mount point `/run/user/10243/gvfs': Permission denied
    #
    #     (chromium:1027): dconf-CRITICAL **: unable to create directory '/run/user/10243/dconf': Permission denied.  dconf will not work properly.
    #     XDG_RUNTIME_DIR (/run/user/10243) is not owned by us (uid 10243), but by uid 0! (This could e g happen if you try to connect to a non-root PulseAudio as a root user, over the native protocol. Don't do that.)
    #
    #     [1027:1101:0820/174930.152868:ERROR:simple_backendᵢmpl.cc(128)] Failed to create directory: /run/user/10243/chromium-cache/Default/Cache
    #
    ('NoNewPrivileges=yes', ['*.service']),
    ('NoNewPrivileges=no', [
        'xdm.service', 'display-manager.service'  # for chromium (which is setuid root for sandboxing) ☹
    ]),

]

# Ref. https://manpages.debian.org/stretch/systemd/systemd.unit.5.en.html#SYNOPSIS
_SYSTEMD_UNIT_TYPES = 'service socket device mount automount swap target path timer slice scope'.split()


def main(args):
    if args.image_name is None:
        assert os.path.isfile('/etc/debian_chroot'), "Not in an SOE build environment"
        # We're in a chroot, confirm it's a PrisonPC SOE build environment!
        with open('etc/debian_chroot') as f:
            assert f.read().startswith('bootstrap')
        os.chdir('/')           # NOTE: important for dir_path later!

        all_units = sorted(set(
            p
            for d in ['etc/systemd/system',
                      'lib/systemd/system']
            for p in os.listdir(d)
            if any(p.endswith('.' + x) for x in _SYSTEMD_UNIT_TYPES)))
    else:
        os.chdir('/srv/netboot/images')
        os.chdir(args.image_name)
        os.makedirs('site.dir', mode=0o755, exist_ok=True)
        os.chdir('site.dir')
        # Cleanup anything from previous runs
        for path in glob.glob('etc/systemd/system/*.d/prisonpc-systemd-lockdown.conf'):
            os.remove(path)

        all_units = sorted(set(
            p
            for line in subprocess.check_output(
                ['unsquashfs', '-ls', '../filesystem.squashfs',
                 'etc/systemd/system',
                 'lib/systemd/system'],
                universal_newlines=True).splitlines()
            for p in [os.path.basename(line.strip())]
            if any(p.endswith('.' + x) for x in _SYSTEMD_UNIT_TYPES)))

    for content, patterns in _OUR_RULES:
        assert isinstance(patterns, list)
        for unit in all_units:
            if any(fnmatch.fnmatch(unit, pattern)
                   for pattern in patterns):
                dir_path = 'etc/systemd/system/{}.d'.format(unit)  # NOTE: relative path
                os.makedirs(dir_path, mode=0o755, exist_ok=True)
                with open(os.path.join(dir_path, _OUR_FILENAME), 'a') as f:
                    print(content, file=f)


if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument('--image-name', help='use on an SOE already in /srv/netboot/images')
    main(parser.parse_args())
 ```

