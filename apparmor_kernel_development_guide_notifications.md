# Related Documents
- [Compiler Development Guide](apparmor_compiler_development_guide)
- [Library Development Guide](apparmor_library_development_guide)
- [Profile Tool Guide](apparmor_profile_tools_guide)
- [Kernel Development Guide](apparmor_kernel_development_guide)
  - [Notifications](apparmor_kernel_development_guide_notifications)
- [Policy Development Guide](aparmor_policy_development_guide)

# Introduction

Notifications are how the apparmor kernel message can send event based messages to userspace daemons.

# Files involved
- apparmorfs.c: file based interface
- audit.c
- lib.c
- tc.c
- lsm.c: hooks
- include/audit.h


# Types of Notifications

There are different types of notifications

- broadcast policy changes
- broadcast complain
- unicast prompt/reply

# Specifying When notifications should occur

## Policy based notifications

Notifications are not directly specified in policy instead they are based on profile flags, and rule prefixes along with the presences of a listener that matches the event.

- complain: broadcast any allowed message to all listeners, instead of auditing
- prompt: send any allowed message to the first matching listener

 or a rule prefix.

- complain: as for the complain profile flag except only applies only to events that match the rule
- prompt: as for the prompt profile flag except only applies only to events that match the rule.

The profile flags apply to all actions that do NOT have a specific match in policy (explicit denies count as a match). The rule prefix applies similarly being lower priority than allow or deny rules, so in a situation with rule overlap it only applies to the part of the rule not covered by a regular allow or deny rule.


## State based notifications

These are notifications based on changes to internal state. Policy loads etc.


# Scope of Notification

Notifications listeners are opened against a specific namespace and can be used to listen to events within the namespace's [scope](AppArmorPolicyScope).

The set of notifications being listened on can be further refined by limiting the type of events being listened for and providing a dfa based filter at registration time that can limit notifications to specific profiles.

## Format of the dfa

regular dfa. permission are mapped to the type of notification



# Notification Listeners

- Userspace can open an fd to listen to notifications
- Userspace can specify the set of events it wants to listen to on the fd
  - the set of events received can be selected via a mask of notifiction types
  - a filter to limit notifications to specific profiles or namespaces
- The fd is used to receive notifications and reply to them when necessary
- notifications can be waited on via blocking I/O or select/poll
- multiple listeners can receive the same notification if it is broadcast
- interactive notifications go to a single listener



# Notification Message Life cycle

Notifications are based on apparmor audit messages. However instead of being converted to a text format they are passed in machine native binary structure.

## LSM hooks

AppArmor can not sleep in the majority of the LSM hooks due to locking. Even if the LSM hook allows the task to sleep, apparmor's internal locking (even just rcu_read_lock) prevents sleeping within code. To avoid having to deal with the locking issues, notifications are delayed until hook exit.

Each hook that allows notifications gets a aa_notify struct

  DEFINE_NOTIFY(name, ...)

And at hook exit

  do_notify(name, ...)

If a notification is determined to be needed the audit message is queued up on the aa_notify struct. And it is handled in do_notify at the end of the hook. This allows us to avoid apparmor internal locking and also to group multiple notifications into a single message.

## Waiting

- we use a wait_queue instead of a completion queue
  - as there maybe multiple tasks listening, that may wake up on the same event.

- events are at the policy ns level, but events are filtered per listener fd. Further more a listener may want to listen to events from multiple namespaces.
  - a single wait_queue per ns would force listeners to use multiple fds, and non-blocking I/O or polling
  - instead we use a single wait_queue per listener (fd)
    - listeners are added to a per ns list
    - events are checked against the listeners filter before waking the listener
    - listeners are then responsible for processing available events
      - events are queued at the ns with a unique id, not the listener



- each listener whos filter matches a notification will be woken/receive wakeup?



??? Instead of having a message on multiple queues there is a single queue of notifications per ns
- notifications removed when every listener has processed it OR timeout OR rate limiting drops it




# Audit messages and allocations

The queue of audit messages requires that they be allocated via heap memory, but we also don't want the regular audit path to fail.

Atm the initial audit struct is allocated on the stack, if an audit message is to go to notifications, a new audit structure is allocated from a kmem_cache of audit message objects. The kmem_cache provides some per cpu caching but also allows better use of memory for multiple audit messages in flight at one time. Allocating an audit buffer only if audit/notifications happen also ensures that allocations are kept out of the fast path. Note: the kmem_cache is also used for audit caching and dedup.

Going with dynamic audit buffers also requires that all audit data be stored in the audit message, both the common_audit_data and the apparmor_audit_data.

The shared preallocated buffers (used for file name, ..) can not be used in audit messages that will be queue for notifications as they must be able to live beyond the rcu locking range used to protect the buffer. When the audit message is allocated, and the data from the stack based audit message is copied in, references to the preallocated buffers are replaced by allocating and duping the the string, these buffers must be properly freed once the notification has been issued.





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

broadcast
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

# caching

Two different caches come into play with notifications.

## type cache

The type cache is used to cache permission lookups, when an object type is available. And is a generic cache to help accelerate permission lookup.

For notifications the type cache is used to reduce repeat prompt messages to userspace. It caches previous answers from userspace for a given prompt. However because we don't have a true type for the prompt we use a hash of the prompt parameters. That is unique and guaranteed not to overlap actual types in the cache.

The hash of the parameters is taken early to do a lookup in the cache, and then held (as metadata) unitl after the prompt is inserted in the cache.

Because the cache ages and sees replacement as new permissions are cached it is possible userspace will see the same prompt again if the profile doing the query is not replaced. Just how long this will take on average will depend on the size of the type cache.


## audit cache

The audit cache is used to dedup messages to the audit subsystem. This can greatly reduce the flood of messages that complain mode/learning mode generates. It is very similar to the type cache except it caches audit data objects, instead of permissions.

- complain/learning and policy change messages: are always checked against the audit cache.
- prompt messages: are only checked against the audit cache if the prompting results in an audited message.
 

needs to take cred ref, profile/label refs



# Notification Message Listeners (Daemons)

Userspace notification daemons follow a basic notification loop

- open ```notify``` file
- register event mask
- poll events (optional)
- read events
- reply to event if required

Listeners must be registered before a notification can be delivered to them. Notifications that have been sent before a listener has registered will never be received by the listener.

Notification listeners are per namespace.

There can be multiple listeners per namespace, and listeners may be subscribed to the same set of notifications.

Notification listeners should specify a buffer larger than the largest notification. The kernel is capable of delivering multiple notification per kernel round trip.

## information notifications

Many notifications are informational.

Informational notifications are broadcast style messages and will be sent to every listener that matches the notification.


## interactive notifications

Interactive notifications will only be sent to the first listener who's subscription matches.

Prompt notifications are interactive. There are two mechanism for dealing with them depending on where the notification occurs. The task can be slept at the hook point, or the task can be signaled and suspended. From an interactive notification policy POV these methods are the same and there is no way to specify one or the other.

From a listener POV it can choose to support one and not the other.

### Sleep task on notification

There are a few LSM hooks that allow sleep. In these cases sleep task on notification can be used. The task is suspended at the point of notification and a notification is sent to userspace. When the reply is received the task is woken up and continues.

The reply from userspace, can update the permission set and potentially change the results returned to for the syscall.

#### Timeouts

The notification has a timeout, and a response must be received before the timeout expires. It is possible to reply to a notification with a keep-alive reply to keep the notification active, while waiting for some other event in userspace.

There are a limited number of keep alive replies that can be applied before the notification will be canceled.


#### signals

Sleeping notifications are interruptable and which can cause
ERESTART??? to be returned.


#### Down grading

Sleep task notifications can be handled as a signal task notification if a listener capable of handling a sleep task notification is not available but a handler capable of dealing with signal task notifications is.

### Signal task on notification

This technique requires either task cooperation or ptracing of the task that is causing the notifications. This means it can not be used with debuggers, some seccomp filters, upstart job control and yama restrictions may also be a problem.

A SIG_STOP? is sent to the task, causing it to suspend. A error value of ????, is returned from the syscall. In addition a notification is sent to a listener.

The userspace listener must receive the notification, trace the task, reply to the notification, restart the task at the original syscall.


The task maybe already ptraced by the listener
??? should we require the listener register a ptrace handler, so the notification can verify the correct ptracer before sending the notification????


### No Listeners

If there are no matching listeners to an interactive notification, the notification is cancelled and the error code in the audit structure is returned.

??? debug/info message for missing listener

Notifications that are cancelled are not cached to either the type nor audit caches.


### Dealing with time outs and interrupts


### replying to a notification



#### registering a ptrace handler


### replying to a notification





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



# Keeping notifications from becoming an attack vector via consuming kernel resources

type cache, audit cache, and notification queues are all bounded

interactive notifications always have a time out



