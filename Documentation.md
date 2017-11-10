AppArmor Documentation
======================

AppArmor Essentials
-------------------

-   [Getting started](GettingStarted)
-   [Quick guide to AppArmor profile language](QuickProfileLanguage)
-   [Monitoring AppArmor](AppArmorMonitoring)
-   [AppArmor configuration and policy directory layout](Policy_Layout)
-   [Determining if AppArmor is causing a problem](AppArmor_Failures)
-   [Frequently Asked Questions](FAQ)

Manuals
-------

Profile enforcement:

-   [apparmor\_parser](http://manpages.ubuntu.com/manpages/precise/en/man8/apparmor_parser.8.html)
-   [aa-audit](http://manpages.ubuntu.com/manpages/precise/en/man8/aa-audit)
-   [aa-enforce](http://manpages.ubuntu.com/manpages/precise/en/man8/aa-enforce.8.html)
-   [aa-complain](http://manpages.ubuntu.com/manpages/precise/en/man8/aa-complain.8.html)

Monitoring tools:

-   [aa-status](http://manpages.ubuntu.com/manpages/precise/en/man8/aa-status.8.html)
-   [aa-notify](http://manpages.ubuntu.com/manpages/precise/en/man8/aa-notify)
-   [aa-unconfined](http://manpages.ubuntu.com/manpages/precise/en/man8/aa-unconfined.8.html)

Profile development:

-   [aa-autodep](http://manpages.ubuntu.com/manpages/precise/en/man8/aa-autodep.8.html)
-   [aa-logprof](http://manpages.ubuntu.com/manpages/precise/en/man8/aa-logprof.8.html)
-   [aa-genprof](http://manpages.ubuntu.com/manpages/precise/en/man8/aa-genprof.8.html)
-   [mod\_apparmor](http://manpages.ubuntu.com/manpages/precise/en/man8/mod_apparmor.8.html)
-   [aa\_change\_hat](http://manpages.ubuntu.com/manpages/precise/en/man2/aa_change_hat.2.html)
-   [aa\_change\_profile](http://manpages.ubuntu.com/manpages/precise/en/man2/aa_change_profile.2.html)
-   [PAM plugin](pam_apparmor)

How-to and Tutorials
--------------------

-   [Creating and modifying AppArmor policy with the tools](Profiling_with_tools)
-   [Creating and modifying AppArmor policy by hand](Profiling_by_hand)
-   [Using mod\_apparmor with Apache to confine web applications](mod_apparmor_example) - DRAFT
-   [Using AppAmor with libvirt to confine virtual machines](Libvirt)
-   [Integrating AppArmor with PAM for login-based policies](Pam_apparmor_example)
-   [Using AppArmor for Role Based Access Control (RBAC)](AppArmorRBAC) - DRAFT
-   [Using AppArmor for Multilevel security (MLS)](AppArmorMLS) - DRAFT
-   [Using AppArmor to confine and control Windows applications run through wine](AppArmorWine) - DRAFT
-   [Working with full system policy](FullSystemPolicy) - DRAFT
-   [|How to use AppArmor in systemd](AppArmorInSystemd) - DRAFT

In Depth
--------

Policy:

-   [Policy Layout Reference](Policy_Layout)
-   [Policy creation guide](CreatingPolicy) - DRAFT
-   [AppArmor Profile Name Specification](AppArmorProfileSpec) - DRAFT
-   [Using policy namespaces in AppArmor](AppArmorNamespaces) - DRAFT
-   [Using policy stacking in AppArmor](AppArmorStacking) - DRAFT
-   [Using AppArmor to confine and secure X Windows applications](AppArmorXace) - DRAFT
-   [Using AppArmor to secure Ubuntu snappy Desktop applications](AppArmorSnappyDesktop) - DRAFT
-   [Using AppArmor to confine and secure DConf/GSettings for Desktop applications](AppArmorGSettings) - DRAFT
-   [mod\_apparmor Apache module](mod_apparmor) - DRAFT
-   [Core Policy Reference Manual](AppArmor_Core_Policy_Reference) - DRAFT

Technical documention:

-   [AppArmor confinement model](AppArmor_Model) - DRAFT
-   [Kernel Documentation](TechnicalDoc_Kernel) - DRAFT
-   Policy Encoding
    -   [TechnicalDoc\_HFA](TechnicalDoc_HFA) - DRAFT
        -   [Format of the DFA/HFA](TechnicalDoc_HFA_Layout) - DRAFT
        -   [PolicyDB the encoding of policy rules into the HFA](TechnicalDoc_PolicyDB) - DRAFT
        -   [Encoding permissions](TechnicalDoc_HFA_permissions) - DRAFT
        -   [Putting it altogether relationship of Policy and DFA/HFA](TechnicalDoc_Policy_Layout) - DRAFT
        -   [Steps in generating the DFA/HFA](TechnicalDoc_HFA) - DRAFT
    -   [Encoding of Rule Paths](TechnicalDoc_RulePathEncoding) - DRAFT
        -   [Encoding of File Rule Paths](TechnicalDoc_FileRuleEncoding) - DRAFT
        -   [Encoding of Network Rule Paths](TechnicalDoc_NetworkRuleEncoding) - DRAFT
        -   [Encoding of Mount Rule Paths](TechnicalDoc_MountRuleEncoding) - DRAFT
            -   [Encoding of the mount flags](TechnicalDoc_Mount_Flags) - DRAFT
        -   [Encoding of DBus Rule Paths](TechnicalDoc_DBusRuleEncoding) - DRAFT
        -   [Encoding of X Window Rule Paths](TechnicalDoc_XWindowsRuleEncoding) - DRAFT

-   [Technical Documentation](TechnicalDoc) - DRAFT
-   Other
    -   [Accessing /proc/ files results in ptrace requests](TechnicalDoc_Proc_and_ptrace)

Development:

-   [AppArmor development APIs](AppArmorAPIs) - DRAFT
-   [AppArmor Interfaces](AppArmorInterfaces) - DRAFT
-   [AppArmor Update Guidelines](AppArmorUpdateGuidelines) - DRAFT

Presentations
-------------

-   old Novell presentations?
-   LSS 2013
-   LSS 2014
-   LSS 2015

Misc
----

-   [AppArmor History](AppArmor_History)
-   [Versions of AppArmor & Release Notes](AppArmor_versions)
-   [Development Roadmap](DevelopmentRoadmap)
-   [Bug tracking](BugTracking)
-   [Work items](WorkItems)
-   [Release Process](ReleaseProcess)
-   [Compiler improvements tracking](CompilerImprovements)
-   [Presentation Slides](AppArmor_Presentations)
-   [Translations Process](Translations)

Community Documentation
-----------------------

-   [Ubuntu AppArmor guide](https://help.ubuntu.com/community/AppArmor) is an introduction to using AppArmor on Ubuntu platforms.

