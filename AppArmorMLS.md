WARNING this page is extremely preliminary and has been imported
wholesale from the old wiki without changes and contains bad/wrong
information, you have been warned

Versions of AppArmor 2.4 and above are capable of enforcing an
[MLS](http://en.wikipedia.org/wiki/Multilevel_security) style of
security. AppArmor achieves the level enforcement at a user level
granularity. All applications run by the user should be considered
at the same level. To obtain a different level of access, a new user
login is used.

Overview of MLS style security with AppArmor
============================================

??? need to fit in that DAC still applies, unless explicitly disabled
using capabilities ???

The techniques described here build on those used to do
[RBAC](http://developer.novell.com/wiki/index.php/Apparmor_RBAC)
with AppArmor.

The basics idea behind doing an MLS style security in AppArmor is
to use a role based profile per security level. The profile for a
level contains access rules for all user interaction at and between
levels. To enforce a read down, write up requirement files of users
in a lower level are specified as read only, and files of users in
higher levels are write only. Files within the current level can be
controlled in a variety of ways.

A given user (uid) can be assigned to only a single level (role),
and to change levels the user must log in as a new user that has a
different uid.

? can make user names username-level etc to help remember, can allow
passwords to be the same ? can the pam module be modified to prompt
for level at auth and automatically change the user name? This would
ease the burden of changing levels. ? - maybe. it should be able to
map username as long the apparmor auth component is the first module.

-   have the auth component get the user info, change the username and return auth\_okay for the next module
-   to then auth against.
-   The only potential problem is if pam maps to the uid before calling into its auth modules

? conversly, the pam module could also take the level from the loggin name so it doesn't need to be prompted for.

Remapping user names in pam\_apparmor
=====================================

pam\_apparmor option to remap entered user name, level to username\_level

used to provide single loging username but separation using multiple user ids.

can overcome dac by giving capability dac override (user, user\_level1, user\_level2, ...)

Basics of constructing MLS style profiles
=========================================

So for a 3 level model, with users A, B belonging to level 1, users
C, D to level 2, and users E, F to level 3 the profiles would look
something like: ??? PROBLEM you don't want to allow a lower level to
write to any arbitrary file at a higher level ???

```
 ??? fix this
 profile level_1 {
   ...
    #level 1 is the bottom so there is no read down
   owner=(C D E F) /** w,     #allow write up to levels 2 and 3
   owner=(A B) /** rwkl,      #allow users in this level to communicate
    ...
 }
  profile level_2 {
   ...
    owner=(A B) /** r,        #allow read down to level 1
   owner=(E F) /** w,        #allow write up to level 3
   owner /** rwkl,           #don't allow users on this level to share
    ...
 }
  profile level_3 {
   ...
    owner=(A B C D) /** r,    #allow read down to level 1 and 2
   owner=(E F) /** rwkl,     #allow users on this level to share
    ...
 }
``` 

Example 1
---------

While example 1 outlines the general idea, it is not very flexible or
manageable, as adding a user requires updating multiple profiles. To
help remedy this variables and includes are used to abstract out the
user specific information, allowing updates to be made in a single
location.

A variable is defined for the users of each level and then the variable
can be substituted in place of the owners in the list. Resulting in
profiles that look something like:

```
 @level_1_users = A B
 @level_2_users = C D
 @level_3_users = E F
  profile level_1 {
   ...
    #level 1 is the bottom so there is no read down
   owner=(@{level_2_users} @{level_3_users}) /** w,         #allow write up to levels 2 and 3
   owner=(@{level_1_users})                  /** rwkl,      #allow users in this level to communicate
    ...
 }
  profile level_2 {
   ...
    owner=(@{level_1_users}) /** r,                          #allow read down to level 1
   owner=(@{level_3_users}) /** w,                          #allow write up to level 3
   owner /** rwkl,                                          #don't allow users on this level to share
    ...
 }
  profile level_3 {
   ...
    owner=(@{level_1_users} @{level_2_users}) /** r,         #allow read down to level 1 and 2
   owner=(@{level_3_users})                  /** rwkl,      #allow users on this level to share
    ...
 }
```

Example 2
---------

While example 2 is better than example 1, it is still not abstracted
enough. Profiles are likely to be in their own files, and there may
be multiple profiles for applications in a single level. To do this
the user variables are moved into an include file and shared parts
of a level description can be moved into their own includes.

```
 FILE: mls/users
   @level_1_users = A B
   @level_2_users = C D
   @level_3_users = E F
  FILE: profile_level_1
   profile level_1 {
     #include <mls/users>
     ...
      #level 1 is the bottom so there is no read down
     owner=(@{level_2_users} @{level_3_users}) /** w,         #allow write up to levels 2 and 3
     owner=(@{level_1_users})                  /** rwkl,      #allow users in this level to communicate
      ...
   }
  FILE: profile_level_2
   profile level_2 {
     #include <mls/users>
     ...
      owner=(@{level_1_users}) /** r,                          #allow read down to level 1
     owner=(@{level_3_users}) /** w,                          #allow write up to level 3
     owner /** rwkl,                                          #don't allow users on this level to share
      ...
   }
  FILE: profile_level_3
   profile level_3 {
     #include <mls/users>
     ...
      owner=(@{level_1_users} @{level_2_users}) /** r,         #allow read down to level 1 and 2
     owner=(@{level_3_users})                  /** rwkl,      #allow users on this level to share
      ...
   }
```

Example 3
---------

??pam\_apparmor config

?? MLS with AppArmor
====================

Manually creating and managing an MLS style policy as built up in
?above section? is not trivial. There are multiple places where a
single piece of data needs to be updated, ???. AppArmor provides some
tools and configuration options that help build MLS style policy.

?pam\_apparmor configuration automatically creating user variables

?pam\_apparmor allowing for single name mapping to multiple login
levels

?tools supporting standard include locations

?tools for managing policy with respect to MLS

Tools to manage AppArmor MLS
============================

tools to help manage and automate

how does pam\_apparmor interact with MLS how do we keep configuration
in sync how do we add/remove users - this necessitates reloading policy

-   who has the rights to add/remove users
-   do we want to break this right up as is done for roles

how do we leverage su/sudo to do role and level changes??? could put
a wrapper around su, but not sudo as we don't want to go to root uid

hrmm problem with role management, and not reloading policy. Can make
it so user can add other users to a given role but then need to reload
affected policy. May need to go with a manage tool that can update
the policy and reload. Must be done like sudo yuk. Maybe done as a
sudo wrapper????
