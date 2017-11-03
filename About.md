AppArmor is a [Mandatory Access Control (MAC)](https://en.wikipedia.org/wiki/Mandatory_access_control) security system. Its aim is to provide an easy-to-use security system targeted at securing individual applications by defining and restricting what resources and application has access to and can share. Unlike other container technologies (chroot, lxc containers) AppArmor focuses on in place sharing and control to enforce security instead of making a physical copy of the system to obtain complete isolation. In addition to controlling the standard system resources (files, networking, capabilities, ...) AppArmor provides extension to system services and daemon's (dbus, gsettings, X, ...) allowing its policy enforcement integration beyond what isolation allows.

Specifically AppArmor is a variant of [Domain Type Enforcement (DTE)](http://citeseer.ist.psu.edu/viewdoc/summary?doi=10.1.1.37.1501) which is itself a variant of [Type Enforcement (TE)](https://en.wikipedia.org/wiki/Type_enforcement). While AppArmor is capable of providing a total system policy its policy is designed and focused around an application (domain) centric model allowing for easy targeting of specific applications or users. Because AppArmor does not require a total system policy, it is easy to selectively deploy in just a few hours.

The base unit of AppArmor confinement is a profile, which is a text file describing the privileges, and accesses that an application or service has, in the form of a [white list](https://en.wikipedia.org/wiki/Whitelist). The profile text file is compiled by the policy compiler (the apparmor\_parser) and loaded into the kernel where the AppArmor security module, a [Linux Security Module](https://en.wikipedia.org/wiki/Linux_Security_Modules), enforces all kernel based privileges. The AppArmor kernel module exposes an interface and API so that Trusted Helpers (system services that have been extended to support AppArmor policy enforcement) can enforce the portion of the policy that they are responsible for.

AppArmor allows for profiles of different modes to be mixed allowing for some profiles to be enforcing policy while others are being developed in a complain mode. It uses include files to ease development, and it has a far lower barrier to entry than other popular MAC systems.

Properties of AppArmor include:

* profiles are simple text files.
* comments are supported in the profile.
* absolute paths as well as file globbing can be used when specifying file access

*  various access controls for files are present.

*  access controls for networking are present

*  specificity in rule matching, ie the most specific rule matches

*  include files are supported to ease development and simplify profiles

*  variables can be defined and manipulated outside the profile

*  AppArmor profiles are easy to read and audit

*  AppArmor policy can be read and audited on a per application profile basis

AppArmor is an established technology first seen in Immunix and later integrated into Ubuntu, Novell/SUSE, and Mandriva. Core AppArmor functionality is in the mainline Linux kernel from 2.6.36 onwards; work is ongoing by AppArmor, Ubuntu and other developers to merge additional AppArmor functionality into the mainline kernel.

The exact set of features and permissions available vary with the version of AppArmor installed, the version of the Linux kernel, and which system services a Linux distribution has enabled (eg. For AppArmor dbus mediation to be enforced, this option must be enabled in the distros dbus daemon).
