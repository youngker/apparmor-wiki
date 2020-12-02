AppArmor 3.0.1 was released 2020-12-02.

# Introduction

AppArmor 3.0.1 is a maintenance release of the user space components
of the AppArmor security project. The kernel portion of the project
is maintained and pushed separately.

This version of the userspace should work with all kernel versions from
2.6.15 and later (some earlier version of the kernel if they have the
apparmor patches applied).

The kernel portion of the project is maintained and pushed separately.


# Highlighted fixes or improvements
- Adds support for ```capability checkpoint_restore``` for the 5.9 and later kernels

# Obtaining the Release

- https://gitlab.com/apparmor/apparmor/-/releases/v3.0.1

# Changes in this Release

These release notes cover all changes between 3.0 (5d51483bfecf556183558644dc8958135397a7e2) and 3.0.1 (b0f08aa9d678197b8e3477c2fbff790f50a1de5e) [apparmor-3.0 branch](https://gitlab.com/apparmor/apparmor/tree/apparmor-3.0).


## Library
- fix failure in procattr accesses due to domain change ([MR:681](https://gitlab.com/apparmor/apparmor/-/merge_requests/681), [AABUG:131](https://gitlab.com/apparmor/apparmor/-/issues/131))
- add missing include for `socklen_t` ([MR:642](https://gitlab.com/apparmor/apparmor/-/merge_requests/642))
- add _aa_asprintf to private symbols ([MR:643](https://gitlab.com/apparmor/apparmor/-/merge_requests/643))
- add `aa_features_new_from_file` to public symbols ([MR:643](https://gitlab.com/apparmor/apparmor/-/merge_requests/643))

## Policy Compiler (a.k.a apparmor\_parser)
- add support for capability checkpoint_restore in the policy language.
- Add support for CAP_CHECKPOINT_RESTORE ([MR:654](https://gitlab.com/apparmor/apparmor/-/merge_requests/654))
- Fix warning message when complain mode is forced ([MR:649](https://gitlab.com/apparmor/apparmor/-/merge_requests/649), [LP:1899218](https://bugs.launchpad.net/bugs/1899218))
- fix parser.conf commenting on pinning an abi ([MR:648](https://gitlab.com/apparmor/apparmor/-/merge_requests/648))
- dont force host cpp to detect reallocarray ([MR:647](https://gitlab.com/apparmor/apparmor/-/merge_requests/647))

## aa_status
- Fix build issue with musl ([MR:647](https://gitlab.com/apparmor/apparmor/-/merge_requests/647))

## aa-notify
- don't crash if the logfile is not present due to rotation ([MR:688](https://gitlab.com/apparmor/apparmor/-/merge_requests/688), [AABUG:130](https://gitlab.com/apparmor/apparmor/-/issues/130))
- Stop aa-notify from exit after 100s of polling ([MR:660](https://gitlab.com/apparmor/apparmor/-/merge_requests/660), [AABUG:126](https://gitlab.com/apparmor/apparmor/-/issues/126))
- Skip test if it can not access /var/log/wtmp ([MR:641](https://gitlab.com/apparmor/apparmor/-/merge_requests/641), [AABUG:120](https://gitlab.com/apparmor/apparmor/-/issues/120))

## Utils
- add capability checkpoint_restore to the severity db ([MR:656](https://gitlab.com/apparmor/apparmor/-/merge_requests/656))
- check if abstractions exist ([MR:683](https://gitlab.com/apparmor/apparmor/-/merge_requests/683), [BOO:1178527](https://bugzilla.opensuse.org/show_bug.cgi?id=1178527))
- aa-autodep: load abstractions on start ([MR:682](https://gitlab.com/apparmor/apparmor/-/merge_requests/682), [BOO:1178527](https://bugzilla.opensuse.org/show_bug.cgi?id=1178527))
- Fix hotkey conflict in utils de.po and id.po ([MR:675](https://gitlab.com/apparmor/apparmor/-/merge_requests/675))
- fix failure of make -C profiles check-logprof ([MR:663](https://gitlab.com/apparmor/apparmor/-/merge_requests/663), [AABUG:36](https://gitlab.com/apparmor/apparmor/-/issues/36))
- split linting with PYFLAKES into a separate target. ([AABUG:121](https://gitlab.com/apparmor/apparmor/-/issues/121))

## Policy

#### abstractions
- fonts
  - Add Fontmatrix support ([MR:657](https://gitlab.com/apparmor/apparmor/-/merge_requests/657))
- mesa
  - tightens cache location and add fallback ([AABUG:91](https://gitlab.com/apparmor/apparmor/-/issues/91))
- ubuntu-browsers
  - Add support for brave browser ([MR:667](https://gitlab.com/apparmor/apparmor/-/merge_requests/667))
  - Adjust ubuntu-integration to use abstractions/exo-open ([MR:666](https://gitlab.com/apparmor/apparmor/-/merge_requests/666))
- X
  - Allow (only) reading X compose cache ([MR:685](https://gitlab.com/apparmor/apparmor/-/merge_requests/685))
  - make x11 socket writable again ([MR:664](https://gitlab.com/apparmor/apparmor/-/merge_requests/664))
  - adjust for new ICEauthority path in run ([MR:668](https://gitlab.com/apparmor/apparmor/-/merge_requests/668))

#### profiles
- dhclient-script
  - Fix invalid Pux (should be PUx) permissions ([MR:676](https://gitlab.com/apparmor/apparmor/-/merge_requests/676))
  - Fix dhclient and dhclient-script profiles to work on debian buster ([MR:645](https://gitlab.com/apparmor/apparmor/-/merge_requests/645))
- dovecot
  - allow reading dh.pem ([MR:671](https://gitlab.com/apparmor/apparmor/-/merge_requests/671), [debug10](https://bugs.debian.org/10))
  - allow kill signal ([MR:671](https://gitlab.com/apparmor/apparmor/-/merge_requests/671))
- nscd
  - Fix service fails on Arch Linux ([MR:651](https://gitlab.com/apparmor/apparmor/-/merge_requests/651), [AABUG:124](https://gitlab.com/apparmor/apparmor/-/issues/124))

## Documentation
- Fix typos in man pages ([MR:669](https://gitlab.com/apparmor/apparmor/-/merge_requests/669))
- apparmor_xattrs.7: fix whatis entry ([MR:669](https://gitlab.com/apparmor/apparmor/-/merge_requests/669))
- fix manpage order ([MR:646](https://gitlab.com/apparmor/apparmor/-/merge_requests/646))


## Tests
- regression
  - Fix aa_policy_cache to use correct config file ([MR:653](https://gitlab.com/apparmor/apparmor/-/merge_requests/653))
  - Fix regression tests when using in tree parser ([MR:653](https://gitlab.com/apparmor/apparmor/-/merge_requests/653))

# Note

There is a semantic change in the 4.8 kernel (commit
9f834ec18defc369d73ccf9e87a2790bfa05bf46) that affects apparmor policy
enforcement. Specifically it affects when the m permission bit is
checked for elf binary executables. Policy and tests within apparmor
2.12 and later have been updated to support running on pre 4.8 and 4.8+ kernels.
