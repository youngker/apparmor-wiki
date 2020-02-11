todo apparmor auditing

# Intro

AppArmor uses the Linux kernel's audit subsystem to audit events, but it does not require the use of auditd, allowing system administrators flexibility in their choice of an audit system. If auditd is used AppArmor integrates with it and its records will be recorded with other audit messages.

# auditd

Auditd is the userspace daemon responsible for logging the messages generated by the kernel audit subsystem. It is also responsible for loading the configuration options and syscall and file watch rules if they are going to be used.

[auditd](http://man7.org/linux/man-pages/man8/auditd.8.html)
[audit.rules](https://linux.die.net/man/7/audit.rules)

# Kernel Audit Subsystem

Audit log format is based around a key=value format. Each audit record will have a an audit type

  type=1300

and set a set of key=value fields.

[audit types](https://access.redhat.com/articles/4409591#audit-record-types-2)
[audit fields](https://github.com/bfuzzy/auditd-attack/wiki/Audit-Event-Fields)
type=1400 AVC
type=1107 USER_AVC

# configuration

AppArmor auditing will be affected by either auditd configuration, or if auditd the kernels printk configuration.

## auditd

link to auditd config docs

buffer size
rate limiting

filter rules

## no auditd

/proc/sys/kernel/printk_ratelimit


# AppArmor Userspace component logging

When an apparmor userspace component logs a message there are a few different ways it may appear in the logs dependent on what version of apparmor the component was written to use, and what kernel is in use.

# component logging via apparmor kernel module

In this case the apparmor userspace component logs as if it is coming directly from the apparmor kernel module. The type will be either AVC or 1400.


## component logging to audit directly

USER_AVC or 1107

 [529593.502230] audit: type=1107 audit(1578493652.435:1237): pid=10048 uid=103 auid=4294967295 ses=4294967295 msg='apparmor="DENIED" operation="dbus_signal"  bus="system" path="/org/freedesktop/UPower/devices/DisplayDevice" interface="org.freedesktop.DBus.Properties" member="PropertiesChanged" name=":1.58" mask="receive" pid=12886 label="/usr/lib/chromium-browser/chromium-browser" peer_pid=11285 peer_label="unconfined" info="No such file or directory" exe="/usr/bin/dbus-daemon" sauid=103 hostname=? addr=? terminal=?'

# comparison to Auditd syscall and file watch rules

It is recommended that AppArmor rules are used in place of auditd's file watch rules or when applicable syscall rules.

## Advantages

* lower overhead.
  * While AppArmor uses the audit subsystem it does not create the event overhead that audits syscall or file watch rules do. If a syscall or file watch rule is in place then the AppArmor audit record will be added to the event but if those rules are not in place the additional auditing information is not kept and the AppArmor message will stand on its own.
  * AppArmor policy is declarative and compiled. Auditd's syscall and file watch rules are processed linearly in order. This means that the more rules that are used the larger the overhead. AppArmor rules are compiled into a state machine so that the overhead for one or thousands of rules is the same.
* Can be targeted to applications and users: AppArmor allows auditing rules to be exist at the system level or targeted to specific applications, users or use cases. This allows auditing of events to be better targeted helping to reduce noise and overhead.
* AppArmor auditing can be tied to the integrity subsystem, allowing audit trails to trigger when the integrity is compromised.
* finer granularity - AppArmor rules allow for pattern matching enabling auditing to be better targeted than auditd's file watch rules.
  * AppArmor can watch for the creation of files and directories that don't exist, making it easier to audit creation of files.
* AppArmor will audit the true path of files accessed post relative path and symlink resolution. This makes tracking file related actions much easier.
* AppArmor will audit ```*at``` calls with a path for the descriptor. This makes it much easier to determine what is being done by execveat, fchmod etc.
* AppArmor has partial container support. While AppArmor's to the audit subsystem retains the limitation of a single autdit daemon (auditd) if the container orchastrator supports AppArmor namespaces, AppArmor rules can be defined for the container separate from the system. In addition system audit rules can be applied on the container at the same time as the container audit rules.
* AppArmor rules can do more than audit, they can also be used to deny access or even kill the task.
* Does NOT require audid (userspace daemon of the audit subsystem). When auditd is not present AppArmor audit messages will be sent to the kernel ring buffer allowing alternate userspace logging systems to be used.

## Disadvantages
* can't audit individual syscalls
* can't match on some of the conditionals that audit rules allow
* won't generate full syscall log trace unless syscall tracking enabled, via audit rule or ???