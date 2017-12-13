AppArmor 2.7 is incremental improvement over AppArmor 2.6, that has
focused on bug fixing and internal cleanups of the userspace code
over new features. AppArmor 2.7 is purely a userspace release, it
leverages the same kernel code as AppArmor 2.5.

-   For information on other versions please see [AppArmor versions](AppArmor_versions)

New Features and Improvements
-----------------------------

### AppArmor release wide changes

-   Merged in Several patches carried by Ubuntu and opensuse
-   Support for systemd
-   Improved Debian packaging and build

### aa\_status

-   completely rewritten (in python) and updated

### aa\_notify

-   fix notifier so that it works on opensuse

### AppArmor Parser

-   add capability syslog support
-   support for rlimit cpu
-   conf file to control optimization settings
-   improved debugging dumps
-   build improvements and cleanups to code
-   check parser's own time stamp with respect to binary cache
-   improved profile attachment specification handling
-   better support of older kernel with compiles involving network rules
-   multiple minore bug fixes

### AppArmor Utils (aa-genprof/aa-logprof)

-   update log entry types
-   prefer /var/log/syslog over /var/log/messages
-   turn off printk ratelimiting while generating profiles
-   add capability syslog
-   Fix px/Px cx/Cx permission modifiers so they aren't always lower case
-   fix subprofile whitespacing

### AppArmor Library (libapparmor)

-   new fn aa\_find\_moutpoint
-   new fn aa\_is\_enabled
-   new fn aa\_getcon
-   new fn aa\_gettaskcon
-   new fn aa\_getpeercon

### apparmor.vim

-   add pux and PUX permissions
-   add capability syslog

### AppArmor Profiles

-   multiarch paths
-   update with capability syslog
-   multiple minor profile improvements
-   /var/run -&gt; /run updates

### initscripts

-   systemd support add to rc.apparmor.functions
-   actually skip loading profiles that are reported as skipped
-   properly handle child profile separators
-   unload hats first

