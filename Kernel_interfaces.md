Low level interfaces to the AppArmor kernel module
==================================================

### /proc/\<pid\>/attr/

-   /proc/\<pid\>/attr/current
    -   **read**: current confinement of a task

        This file contains the name of the currently confining profile,
        which may be optionally followed by the profile mode in
        parenthesis. The name is not null terminated nor followed by
        a new line. If the task is not confined the file will contain
        the word *unconfined*.
    -   **write**: interface for change\_hat, change\_profile, set\_profile

        Writing to this file is the control interface for change\_hat,
        change\_profile. The ability to successfully write a given
        command depends on the profiles confinement and rules. The
        command must be written entirely in a single write and it
        does not need to be null terminated.

        -  change hat

           The change hat command allows transitioning of a task
           between a main profile and special hat profiles. The
           \<hat name\> is the name of a hat as it would appear in a
           profile. The token is an unsigned decimal or hexadecimal
           number in ascii. This command can only be issued by a task
           to it self. The basic format of the command is

           ```
'changehat '<token>^<hat name>
           ```

           -   entering a hat or changing between sibling hats

               To enter a hat or change between hats a valid hat name
               must be given followed by a token, which is required
               to leave the hat. If the profile is currently in a
               hat and trying to change to a sibling hat then the
               token passed must be the same as was used to enter
               the current hat. If the hat is successfully entered
               the token is remembered and must be matched to change
               out of the hat via the change hat interface; e.g.:

               ```
changehat 1234^hat1
               ```

           -   returning from a hat

               To return from a hat, the change hat command is issued
               without a profile name, and with a token value matching
               the token that was used to do the initial change hat. If
               the change hat call is successful the task returns to
               the parent profile and the token is forgotten, allowing
               for new change hat calls using different tokens; e.g.:

               ```
changehat 1234
               ```

           -   errors

               Change hat can fail for a number of reasons, and it
               returns different error codes depending on the failure.

               -   EINVAL: invalid leading command word (missing space between command and args?), no command args, invalid token, invalid hat name, no hat name and zero token
               -   EACCES: change\_hat command issued by another task, or task is being ptraced and new hat is not allowed by trace
               -   EPERM: change\_hat by unconfined task
               -   ENOMEM: out of memory
               -   ENOENT: hat not found
               -   killed: the task will be killed if it is currently in a hat and it passes a token that does not match the currently stored token.

        -   change hatv ????
        -   change profile

            ```
'changeprofile '[':'<namespace>':']<profile name>
            ```
relative names (eg. child and hat profiles, direct transition????

        -   change profilev ????

-   /proc/<pid>/attr/prev - AppArmor v2.4

    -   read: This file provides the profile name of the parent profile when the task is confined by a hat profile. This file is empty when a task is unconfined or does not have a parent profile to return to.
-   /proc/<pid>/attr/exec - change\_profile on exec
-   /proc/<pid>/attr/fscreate - unused
-   /proc/<pid>/attr/keycreate - unused
-   /proc/<pid>/attr/sockcreate - unused

### module parameters

-   /sys/modules/apparmor/parameters/audit
-   /sys/modules/apparmor/parameters/audit\_header
-   /sys/modules/apparmor/parameters/complain
-   /sys/modules/apparmor/parameters/debug
-   /sys/modules/apparmor/parameters/enabled
-   /sys/modules/apparmor/parameters/lock\_policy
-   /sys/modules/apparmor/parameters/logsyscall
-   /sys/modules/apparmor/parameters/mode
-   /sys/modules/apparmor/parameters/path\_max

### securityfs - /sys/kernel/security/apparmor

AppArmor presents a filesystem under securityfs that provides policy
management and introspection as well as introspection into the modules
current settings.

##### Policy Management 

These files provide for loading, replacing, and removing profiles and
profile namespaces. The loading and removal interfaces take data in
the [binary profile format](Binary_profile_format), while the remove
interface takes a profile name.

-   /sys/kernel/security/apparmor/.load
    -   write: binary profile to load
-   /sys/kernel/security/apparmor/.replace
    -   write: binary profile to load
-   /sys/kernel/security/apparmor/.remove
    -   write: name of profile to remove

##### Policy Introspection

-   /sys/kernel/security/apparmor/profiles
    -   read: text list of all profiles loaded

##### Module Introspection

-   /sys/kernel/security/apparmor/matching
    -   read: text of what file matching is done
-   /sys/kernel/security/apparmor/features - AppArmor 2.3
    -   read: text list of different feature versions
