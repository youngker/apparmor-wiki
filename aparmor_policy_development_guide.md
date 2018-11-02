# Related Documents
- [Compiler Development Guide](apparmor_compiler_development_guide)
- [Library Development Guide](apparmor_library_development_guide)
- [Profile Tool Guide](apparmor_profile_tools_guide)
- [Kernel Development Guide](apparmor_kernel_development_guide)
- [Policy Development Guide](aparmor_policy_development_guide)

# Disclaimer
- This assumes you know about the basics of apparmor policy and enforcement ???link
- This document probably lags the code
- This document is a general overview, and some hints and tips. It can't cover everything the code does.

# TODO

# Profile Attachment specifications

Profile attachment specifications define what executable a profile will automatically be attached to.

defines a labeling

## Scripts

Profile attachments can be defined for scripts that are called directly as if they were an executable. Allowing them to have a different confinement attached to them at exec than the interpreter.

  Eg.<br>
  /bin/script foo                            #uses script attachment for confinement

Scripts that are started by running the interpreter can NOT have a different confinement attached at exec (it is still possible for the interpreter to setup a different confinement but that requires the interpreter to use the apparmor api).

  Eg.<br>
  bash /bin/script foo                        #uses interpreters attachment for confinement

This means that care must be taken with scripts, to ensure the correct confinement is applied.

### Denying direct invocation of the interpreter

If profile attachments are defined for scripts, one way to ensure the correct confinement for a script will be used is to define a profile for the interpreter that blocks it from loading and running scripts. This ensures that the only way a script will be run is by calling the script directly.

### Script attachments and writable locations

Script attachments are subject to the same rules and general binary attachments. See ???

### Scripts and locations without attachment

If a script or executable is created in a location without attachment, its possible the script or executable could be used to by-pass policy restrictions if care is not taken.

Profiles should not use executable name based transitions (px, or cx) for paths that overlap writeable locations.


Unconfined processes can by-pass confinement ???






## Attachments and writeable locations

Attachments should be kept as tight as possible and they should not overlap writeable locations for files in a profiled applications. If the applications does have write to a location that overlaps attachment it is possible that an application could create a script or binary executable in a location matched by an attachment that can be used to attain a different confinement than was intended by policy.

