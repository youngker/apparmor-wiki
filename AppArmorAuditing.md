todo apparmor auditing

# Intro

AppArmor uses the Linux kernel's audit subsystem to audit events, but it does not require the use of auditd, allowing system administrators flexibility in their choice of audit system. If auditd is used AppArmor integrates with it and its records will be recorded with other messages.

# Kernel Audit Subsystem

# configuration

## auditd

type=1400 AVC
type=??? USER_AVC

## no auditd

/proc/sys/kernel/printk_ratelimit


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
