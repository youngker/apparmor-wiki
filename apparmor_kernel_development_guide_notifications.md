# Related Documents
- [Compiler Development Guide](apparmor_compiler_development_guide)
- [Library Development Guide](apparmor_library_development_guide)
- [Profile Tool Guide](apparmor_profile_tools_guide)
- [Kernel Development Guide](apparmor_kernel_development_guide)
  - [Notifications](apparmor_kernel_development_guide_notifications)
- [Policy Development Guide](aparmor_policy_development_guide)

# Introduction

Notifications are how the apparmor kernel message can send event based messages to userspace daemons.

# Types of Notifications

There are different types of notifications

- multicast policy changes
- multicast complain
- unicast prompt/reply

# Scope of Notification
Namespace

Hierarchical

Limiting scope

# Notifications

Notifications are based on apparmor audit messages. However instead of being converted to a text format they are passed in machine native binary structure.

AppArmor can not sleep in the majority of the LSM hooks due to locking. Even if the LSM hook allows the task to sleep, apparmor's internal locking (even just rcu_read_lock) prevents sleeping within code. To avoid having to deal with the locking issues, notifications are delayed until hook exit.

Each hook that allows notifications gets a aa_notify struct

  DEFINE_NOTIFY(name, ...)

And at hook exit

  DO_NOTIFY(name, ...)

If a notification is determined to be needed the audit message is queued up on the aa_notify struct. And it is handled in DO_NOTIFY at the end of the hook. This allows us to avoid apparmor internal locking and also to group multiple notifications into a single message.

## Audit messages and allocations

The queue of audit messages requires that they be allocated via heap memory, but we also don't want the regular audit path to fail. ???

It does however require that audit messages are NOT allocated from the stac




profile flags
rule flags

multiple notification
- profile loads
- per profile in stack

apparmor audit msg -> audit msg cache -> dedup -> copy msg -> insert in msg cache -> send to audit subsystem
                   |                          |            |
                   |                           -> drop msg |
                   |                                        -> send to audit subsystem
                   -> notify queue -> insert queue on waiting

multicast
- policy changes
- complain messages
prompting
- synchronous
- asynch + ptrace and restart

prompt failing
- interrupts
- time outs

how to keep alive
restarting request

down grading prompt

## ???
audit structs


## user space api

### include file

### setting up notifications

open file
use ioctl to set notification
wait for notifications
read file
- reply

link to userspace api provided by library.

## ???





