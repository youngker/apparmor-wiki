Policy authoring introduction
=============================

In AppArmor terms policy is the sum of all profiles defined for the
system. Creating good policy is hard. It involves a lot more than
just creating a quick profile around an application. It requires
knowledge of the system, the interaction of applications and the
types of restrictions that the policy is designed to enforce. Even
when just creating a single profile for an application, there needs
to be some understanding of the resources the application is using
to determine what is safe to give an application access to.

This is a general guide on policy authoring, providing tricks and
pit falls for writing a good policy.

Protection goals
================

One of the decisions that needs to be made when authoring policy is
what are the security goals. What needs to be protected, does data
need to not leak out, or is it sufficient to prevent execution of
malicious content. ??? something about users being the week point.

Policy Types
============

There are several general types of policies that require different
levels of attention to detail.

-   Targeted - Targeted is the standard way in which AppArmor is
    deployed. It trades off the security of a totally system look
    down for easy of use and policy authoring, by recognizing that
    there are set of high value applications that can be more easily
    confined than the entire system while still providing value. In
    a targeted policy only a selection of the task on a system are
    confined in any meaningful way. Usually the targeted applications
    are network facing apps.

> ??? something about it being hard to confine the desktop ???

-   Total system - Total system confinement involves confining every
    application on the system.

> ??? what is missing from current apparmor to do this well ??? link
> to doc explaining how to load policy in the initrd, so that you can
> confine init [confining all applications](Confining_all_tasks)

-   RBAC - Role based access control can be done as a total system
    confinement or a per Role confinement but all tasks within a role
    must be completely confined.

AppArmor rules can make authoring profiles easy to understand but
hide policy interactions (type vs file location). Good abstractions
can be used to achieve much of this

Hrrmmm have some general rules and some policy specific guides

Exec rules
==========

flexible making it easy to shoot self in foot

When to use ix, px, cx

Discussions on - creating profiles,

> when to use children, hats, ...

- creating abstractions

Creating Abstractions discussion - where to put this???
=======================================================

Abstracting profiles can make them easier to read, understand, and update.

Abstracting done through 2 main mechanisms - includes - variables

Includes provide - rule sharing - single editing point - method of
expanding profile without editing file via directory includes

-   drop profile fragment in a directory

Variables - provide means of dynamically modifying what a rule means

-   How to identify what can be shared/should be shared?

Includes for Abstractions - used when rule and permissions should
be the same for all consumers - can split permission across several
includes, and allow include to accumulate perms, e.g.:

> foo-ro  contains read only perms, while foo-rw includes foo and expands
> it to include write perms

Problem with Include abstractions is the assumption that profiles
want same permissions for a given set of files - Get around this
with variables

-   define common variables in global/common include
-   use the variable and specify permission in profile or include

-   can do a form of typing by defining a variable, and sharing it
    (every profile then gets to define access perms for the type), e.g.:

    ```
    @{PRIVATE_FILES} = @{HOME}/.ssh  ...
    ```
    
    then in profile

    ```
    @{PRIVATE_FILES}  r,
    @{PRIVATE_FILES}  rw,  ..
    ```

   -   limit: currently variables can contain are only expanded at the
       rule level so the @{TYPE1} variable couldn't contain partial
       rules for both network and file access.
