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

The parser is an old code base that is slowly being modernized, as such it has all the warts of both old and new at the same time. The goal currently is to make adding new code easier and slowly finish moving the old code over to new patterns.
