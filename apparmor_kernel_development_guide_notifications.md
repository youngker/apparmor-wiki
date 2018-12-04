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

- multicast policy changes
- multicast complain
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


# Scope of Notification
Namespace

Hierarchical

Limiting scope

# Notification



# Notification Message Life cycle

Notifications are based on apparmor audit messages. However instead of being converted to a text format they are passed in machine native binary structure.

## LSM hooks

AppArmor can not sleep in the majority of the LSM hooks due to locking. Even if the LSM hook allows the task to sleep, apparmor's internal locking (even just rcu_read_lock) prevents sleeping within code. To avoid having to deal with the locking issues, notifications are delayed until hook exit.

Each hook that allows notifications gets a aa_notify struct

  DEFINE_NOTIFY(name, ...)

And at hook exit

  do_notify(name, ...)

If a notification is determined to be needed the audit message is queued up on the aa_notify struct. And it is handled in do_notify at the end of the hook. This allows us to avoid apparmor internal locking and also to group multiple notifications into a single message.

## ??




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

Prompt notifications are interactive. There are two mechanism for dealing with them depending on where the notification occurs.


### No Listeners

If there are no matching listeners to an interactive notification, the notification is cancelled and the error code in the audit structure is returned.

??? debug/info message for missing listener

Notifications that are cancelled are not cached to either the type nor audit caches.


### Dealing with time outs and interrupts


### replying to a notification

### waiting on event

### ptrace

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





