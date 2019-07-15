# Upstream AppArmor Releases

| AppArmor user space Version | Feature | Minimum Supported Kernel | Required Kernel Version for supported featutres | Notes |
|-----------------------------|---------|------------------------|------------------------------------------------|-------|
| 2.3 | <ul> <li>change_profile allows unconfined processes to enter confinement</li> <li>add aa-repo.pl tool</li> <li>Allow for profile creation without attachment specification</li> <li>Children (local) profiles <ul> <li>Add "cx" execute permission</li> <li>Named profile transitions</li> </ul></li> <li>Hats<ul> <li>change_hat rules</li><li>change_profile rules</li></li> <li>Profile namespaces</li> <li>Link pair rules</li> <li>File rules conditional on file ownership</li> <li>Per rule audit control</li> <li>Deny rules</li> <li>Alias rules</li> <li>Rlimit rules</li> <li>Set capabilities per profile</li> </ul>
  | pre 2.6.24 + out of tree patches (v4 abi) | 2.6.24+ (v5 bi)<sup>1</sup>   | ```1``` <ul><li>2.3 features not supported due to upstreaming changes<ul><li>basic socket mediation - requires out of tree patch</li><li>xattr mediation</li><li>the ability to set confinement on a pre-existing task</li><li>semantics of file mediation changed some</li></ul></li></ul> |
| 2.3.1  |  | | 2.6.27, 2.6.28   | |
| [2.4](Release_Notes_2.4)  |<ul><li>pux transitions</li><li>Improved profile namespace support</li><li>Caching of compiled profiles</li><li>kill mode</li><li>audit control</li><li>change profile on exec</li></ul>   | | 2.6.31, 2.6.32  |Removed support for:<ul><li>chown, chmod, and xattr mediation</li><li>Path-based mediation of unix domain sockets</li><li>Set profile interface</li> |
| [2.5](Release_Notes_2.5)  | <ul><li>Improved policy and DFA verification</li><li>chmod/chown mediation</li><li>change_hatv</li><li>Use CAP_MAC_ADMIN to determine if policy modification is allowed</li><li>hierarchical namespaces</li><li>New profile flags</li><li>Renaming replace</li><li>DFA minimization</li><li>Improved table compression</li><li>New optimization and debugging flags for dfa creation</li><li>aa-decode utility</li><li>apparmor_notifier rewrite</li></ul>  | | 2.6.31   |Removed support for:<ul><li>set capabilities</li><li>Unnecessary /proc//xaddr/* restrictions</li><li>Parallel profile load</li></ul>|
| [2.5.1](Release_Notes_2.5.1)  | <ul><li>Reduce memory usage</li><li>Add ability for apparmor_parser to dump flattened profiles</li><li>Add support for LSM_AUDIT format messages</li><li>Improved auditd handling</li><li>Add support for truncate, rename_src, and rename_dest operation sin SubDomain.pm</li></ul>  | | 2.6.31 (patched), 2.6.32 (patched), 2.6.33 - 2.6.37   | |
| [2.5.2](Release_Notes_2.5.2)  |<ul><li>change_hatv(), change_hat_vargs() available via swig interfaces</li><li>Bug fixes</li></ul>  | |2.6.31 (patched), 2.6.32 (patched), 2.6.33 - 2.6.38   | Removed utils/severity.pl due to incorrect license/copyright statement|
| [2.6.0](Release_Notes_2.6.0)  | <ul> <li> Add support for profile names that are independent of attachment specification </li> <li> Add ability to dump compiled policy to a file </li> <li> Add aa-disable utility </li> <li> Add support for newer auditd formatted messages </li> <li> make change_hatv(), change_hat_varargs() available via swig interfaces </li> </ul>| |2.6.31 (patched), 2.6.32 (patched), 2.6.33 - 2.6.38   | |
| [2.6.1](Release_Notes_2.6.1)  |<ul><li>Bug fixes</li></ul> |  |2.6.31 (patched), 2.6.32 (patched), 2.6.33 - 2.6.38 |
| [2.7](Release_Notes_2.7)  |<ul><li>Support for systemd</li><li>Support for CAP_SYSLOG</li><li>Support rlimit cpu</li></ul>| |2.6.35 - 2.6.39, 3.0, 3.1, 3.2   | last point release to support old immunix/suse kernels using out of tree patches and pcre matching engine (v3 abi) |
| [2.7.2](Release_Notes_2.7.2)  |<ul><li>Bug fixes</li><li>Profile fixes and updates</li></ul> |  |2.6.35 - 2.6.39, 3.0, 3.1, 3.2   | |
| [2.8](Release_Notes_2.8)  | <ul><li>Basic mount rules</li><li>New introspection interface</li><li>New aa-exec utility</li><li>New aa-easyprof utility</li><li>Language improvements</li></ul> | | 3.3, 3.4   | Support for basic mount rules requires the mount kernel patch. |
| [2.8.1](Release_Notes_2.8.1)  |<ul><li>Bug fixes</li></ul>  | |3.3 - 3.6   | |
| [2.8.2](Release_Notes_2.8.2)  |<ul><li>Bug fixes</li></ul>  | |3.3 - 3.6   | |
| [2.8.3](Release_Notes_2.8.3)  |<ul><li>Add --create-cache-dir command line option to apparmor_parser</li><li>Bug fixes</li><li>Profile fixes</li></ul>  | |3.3 - 3.6   | |
| [2.8.4](Release_Notes_2.8.4)  |<ul><li>Policy updates</li><li>Bug fixes</li></ul> |  |3.3+   | |
| [2.8.5](Release_Notes_2.8.5)  |<ul><li>Policy updates</li><li>Bug fixes</li></ul>  | |3.3+   | |
| [2.9.0](Release_Notes_2.9.0)  |<ul><li>Support for mediation of<ul><li>dbus</li><li>signals</li><li>ptrace</li><li>unix abstract sockets</li></ul></li><li>New "allow" keyword</li></ul>  | |3.3+   | |
| [2.9.1](Release_Notes_2.9.1)  | <ul><li>Policy updates</li><li>Bug fixes</li></ul> | |3.3+   | |
| [2.9.2](Release_Notes_2.9.2)  |<ul><li>Policy updates</li><li>Bug fixes</li></ul> | |3.3+   | |
| [2.9.3](Release_Notes_2.9.3)  |<ul><li>Policy updates</li><li>Bug fixes</li></ul>  | |3.3+   | |
| [2.9.4](Release_Notes_2.9.4)  |<ul><li>Policy updates</li><li>Bug fixes</li></ul>  | |3.3+   | |
| [2.9.5](Release_Notes_2.9.5)  | <ul><li>Policy updates</li><li>Bug fixes</li><li>Partial fix for CVE-2017-6507</li> | |3.3+   | |
| [2.10](Release_Notes_2.10)  | <ul> <li>New libapparmor APIs</li> <li>Improved policy compile times</li> <li>Ability to use variables in profile names</li> <li>AppArmor Tools add support for <ul> <li>pux, cux, CUx exec modes</li> <li>Profile attachment specifications</li> <li>Profile de-duplication</li> <li>change_profile rules</li> <li>--no-reload option</li> </ul> </li> </ul> | |3.3+   | |
| [2.10.1](Release_Notes_2.10.1)  |<ul> <li>Allow "unspec" (AF_UNSPEC) family in network rules</li> <li>Policy Updates</li> <li>Bug Fixes</li> </ul> |  |3.3+   | |
| [2.10.2](Release_Notes_2.10.2)  |<ul> <li>Policy Updates</li> <li>Bug Fixes</li> </ul>  | |3.3+   | |
| [2.10.3](Release_Notes_2.10.3)  | <ul> <li>Policy Updates</li> <li>Bug Fixes</li> <li>Fix CVE-2017-6507</ul> | |3.3+   | |
| [2.10.4](Release_Notes_2.10.4)  | <ul> <li>Policy Updates</li> <li>Bug Fixes</li> <li>Add support for zsh in logprof.conf</ul> | |3.3+   | |
| [2.11](Release_Notes_2.11)  | <ul> <li>apparmor_parser supports parallel compiles and loads</li> <li>Utils fully support for dbus, ptrace, and signal rules/events</li> <li>Support stacking in exec and change_profile rules</li> <li>change_profile rules accept exec mode modifier</li> <li>Utils switched to python3 (python2 deprecated)</li> </ul> | |3.3+   | |
| [2.11.1](Release_Notes_2.11.1)  |<ul><li>Add network 'smc' keyword in NetworkRule</li><li>Policy updates</li><li>Bug fixes</li></ul>  | |3.3+   | |
| [2.11.2](Release_Notes_2.11.2)  |<ul><li>Policy updates</li><li>Bug fixes</li></ul>  | |3.3+   | |
| [2.12](Release_Notes_2.12)  |<ul> <li>Reworked YaST interface</li> <li>Add support for 'owner' events to aa-logprof and aa-genprof</li> <li>Bug Fixes</li> <li>Policy Updates</li> </ul>  | |3.3+   | |
| [2.12.1](Release_Notes_2.12.1)  |<ul> <li>Add support for conditional includes in policy</li> <li>Policy Updates</li> <li>Bug Fixes</li> </ul>  | |3.3+   | |
| [2.12.2](Release_Notes_2.12.2)  |<ul> <li>Policy Updates</li> <li>Bug Fixes</li> </ul> |  |3.3+   | |
| [2.13](Release_Notes_2.13)  | <ul> <li>Support conditional includes in policy</li> <li>Allow policy caches to be retained between kernel versions</li> <li>Overlay cache locations</li> <li>Add ablity to customize aa-notify notification message</li> </ul> | |3.3+   | |
| [2.13.1](Release_Notes_2.13.1)  |<ul><li>Policy updates</li><li>Bug fixes</li></ul> |  |3.3+   | |
| [2.13.2](Release_Notes_2.13.2)  |<ul><li>Policy updates</li><li>Bug fixes</li></ul>  | |3.3+   | |
| [3.0](Release_Notes_3.0) |  | |4.13+   | |
