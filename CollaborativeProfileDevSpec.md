Goals
=====

We need more profiles for applications both upstream and
in distributions, and therefore want to encourage people to
collaboratively develop and share those profiles. People want to
share their profiles for certain specialized configurations as well.

Use Cases/User Stories
======================

1.  Joe, a sysadmin starts logprof for the first time
    -   “do you want to participate in collaboration on new profiles for your distribution?”
        -   “yes” could be to download the Vcs branch
    -   “do you want to share your profiles with the community?”
2.  Sarah, a sysadmin, wants to create a profile for the *foo* application. She starts genprof; **what happens?** Different cases:
    1.  Nonexistent profile in both distro and collaborative/showcase area
        -   Should nonexistant profile get automatically promoted to collaborative section, instead of just living in show case section?
            -   Does the distro/release moderator decide what gets promoted to collaborative?
            -   Do the contributors decide for themselves whether something is collaborative or showcase worthy?
        -   Profile not in distro but in collaborative area
        -   User wants profile to go to showcase (nonexistent)
    2.  Profile in distro but not in collaborative area
    3.  Profile in both distro and collaborative area
        -   User wants profile to go to showcase (existing)
3.  Frank, a sysadmin, updates a local policy? **what happens?**
    -   if the policy was shipped with the distro and Frank has agreed to share?
    -   if the policy was pulled originally from the collaborative section?
    -   if the policy was pulled originally from the showcase section?
4.  Melissa, a distro/release moderator, updates a published policy for the frobnitz application. Tony, a sysadmin, is using Melissa's original frobnitz profile. Does Tony receive any notification?
    -   ... if his frobnitz policy is unmodified?
    -   ... if his local frobnitz policy is modified?
        -   ... and those modifications are published in the showcase section?

Assumptions
===========

There are three places to get policies:

-   Official supported distro policies, packaged with apps
-   Unsupported alpha/beta policies, packaged separately
-   Collaborative development policies, on web site (backend vcs?)

Design
======

Web Site Requirements
---------------------

> As an aside, I still dislike these names, as I don't they're accurately
> descriptive; we want collaboration to happen under both sections,
> and to me, policies that you would showcase are policies that would
> be your reference policies. --sbeattie

1.  Collaborative Section (profiles good for inclusion by default)
    -   Only a single profile per software can be uploaded (per distro/release)
    -   Collaboration tools:
        TODO section  
        e.g.: “- haven't tested with custom script directory”

        BUGS section  
        e.g.: “- when I run this profile, the XYZ plugin doesn't work”

        COMMENTS section  
        e.g.: “- I think the dbus part looks wonky...shouldn't it be more restrictive?”
2.  Showcase Section (one-off profiles that aren't for general usage)
    -   would still be good to have notes, configuration that lead to profile
    -   way to compare against collaborative, or reference policy

Userspace tools
---------------

-   Check for profiles and tell user (without automatically downloading)
-   Ask user to contribute once profile is complete
    -   Ask some questions to determine if it's “Collaborative” or “Showcase”
    -   Can only upload to “Collaborative” if no existing profile in same distro/release

Moderation
----------

-   Each distro has moderators that will decide when a profile is ready for inclusion in alpha/beta package

Implementation
==============

Notes and Open Questions
========================

Notes
-----

Open Questions
--------------

### Can we use Launchpad for collaboration?

#### pros

-   bug tracker
-   mailing list
-   bzr integration
-   potentially faster to implement

#### cons

-   not specifically designed for profiling

### Using a VCS as the backend store

-   probably need to hide the VCS from the user in the tool
-   Which VCS? bzr? git? hg?

### What is the profile naming convention, particularly in the showcase section?

### What about Policy Sets?

Basically all profiles belonging to a distro,release,XXX would enable
multiple sets per distro/release if desired

### How to handle new distro releases gaining previous release's policy progress?

-   File as distro/release version?
-   Copied over when distro release comes out?
-   Copied over once moderator is comfortable with policy?

### Is large scale management of policies entirely out of scope?

-   Use case: Bob wants to use the repo to keep a set of policies developed for his AWS instances consistent
-   Use case: Alice wants to integrate with a configuration management tool like puppet somehow.

