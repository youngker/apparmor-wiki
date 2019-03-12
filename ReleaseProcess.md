AppArmor Release Process Notes
==============================

This page is an attempt to capture the AppArmor release process.

**TODO**: update this for gitlab/git conversion.

Making a Kernel Pull request
----------------------------

1. Preparatory Steps
   1. Ensure patches have been staged in linux-next for at least 2 weeks before the request pull is sent
   1. Built and test the set of patches, preferably with different configs and architectures
   2. Dump the candidate patches and run checkpatch on them, make adjustments as needed
   3. Ensure patches have been staged in linux-next for at least 2 weeks before the request pull is sent
2. Sending a pull request

   $ git checkout work

   $ git tag -s apparmor-pr-year-month-day work

       eg. $ git tag -s apparmor-pr-2018-08-23

     Summaries changes in the pull request, and exit editor

       + Features
         - add support for mapping secids and using secctxes
         - add the ability to get a task's secid
         - add support for audit rule filtering

       + Cleanups
         - multiple typo fixes
         - Convert to use match_string() helper
         - update git and wiki locations in AppArmor docs
         - improve get_buffers macro by using get_cpu_ptr
         - Use an IDR to allocate apparmor secids

       + Bug fixes
         - fix '*seclen' is never less than zero
         - fix mediation of prlimit
         - fix memory leak when deduping profile load
         - fix ptrace read check
         - fix memory leak of rule on error exit path

   Push the tag to the upstream repository. There is no need to push the branch

   $ git push  aa-upstream +apparmor-pr-2018-08-23

   Prepare the request-pull

   $ git request-pull pull <base commit> aa-upstream-pull apparmor-pr-2018-08-23 >msg.txt

   prepare email to Linus, lkml and lsm including the request the request pull

   send preferable early in the 2 week merge window



Making a Userspace Release
--------------------------

### Release Preparation

During the run-up to a release, it's helpful to do some preparation
work.

1.  Review trunk check-ins and decide which should be included
2.  Write release notes, summarizing the changes as in [Release\_Notes\_2.8](Release_Notes_2.8):
    -   New features
    -   Language changes
    -   Improvements
    -   Bug fixes
    -   Profile changes

#### Creating a new series in launchpad

If the upcoming release is a new major or minor release (not a point
release, ie. bugfix release), in advance a series should be created
for it, so that milestones can be created for it, assisting with
bug targeting.

1.  https://launchpad.net/apparmor/+addseries
    -   Set the name to be the **Major.Minor** series version
        (e.g. 2.6). Note that you can adjust this after the fact
        (though it does affect some release paths), so if it's
        uncertain whether a release will be a Major or Minor bump,
        it's okay to create a series even if the release version is
        in flux (e.g. 2.7 vs 3.0).
    -   Fill in the Summary as best you can
    -   Since the series is created long before the release is made and branched off, typically the **Branch** will be the bzr trunk: `~apparmor-dev/apparmor/master`

#### Creating a new milestone in launchpad

Milestones in launchpad are used both for attaching releases to,
as well as for bug targeting. To create a new milestone:
1.  https://launchpad.net/apparmor/X.X/+addmilestone where X.X is the
    series the milestone is being created for (e.g. 2.6 for a 2.6.1
    release milestone)
    -   Set the **Name** to be the Major.Minor.Micro release version (e.g. 2.6.2, 3.0.0)
    -   Leave **Code Name** blank unless you're feeling creative
    -   For **Date Targeted**, enter an expected release date, if known

### Official Releases

1.  Bump the release and library version
    -   In a checkout out AppArmor user tools directory edit the **`common/Version`** file to have the appropriate version (eg. 2.6.0).
        -   Note that for the 2.5.x release series only, adjusting the version means editing the VERSION variable in the `common/Make.rules` file.
    -   If any changes to the libapparmor library have been made, edit the version information in libraries/libapparmor/src/Makefile.am according to the rules specified in the file.
        1.  Note that version changes will need to be coherent across all releases. This may force an update to AA\_LIB\_CURRENT
    -   Commit the change and push it back to launchpad (eg. bzr commit).
        1.  You don't need to tag the commit here, we'll tag the release in bzr later on
2.  Create the release tarball with `make tarball` in the top level of the checked out tree.
    -   bzr/launchpad may have a problem with this over bzr+ssh (i.e. you're working off of the `lp:apparmor` or `lp:apparmor/X.Y` branches). If so, you can work around this by setting the REPO\_URL to use the https URL, e.g. `make tarball REPO_URL=https://code.launchpad.net/~apparmor-dev/apparmor/2.8`
3.  Sign the tarball with the apparmor signing key: `gpg --detach-sig --armor -u apparmor@lists.ubuntu.com TARBALL`
4.  Verify that the signature was done correctly: `gpg --verify TARBALL.asc`
    -   The AppArmor signing key has the fingerprint **3ECD CBA5 FB34 D254 961C C53F 6689 E64E 3D36 64BB**
5.  Perform any last minute builds and tests on the tarball to ensure there are no brown paper bag issues.
6.  Tag the release with `make tag` in the toplevel of an up to date checked out tree, to ensure consistently named tags. This does the equivalent of `bzr tag apparmor_VERSION` (e.g. `bzr tag apparmor_2.6.0`).
    -   Note that tags can be deleted and re-added if testing the generated release shows a critical bug that needs to be fixed before release.
7.  GIT: push tag to the repository  `git push <upstream repo> +<tag>`
8.  \[Optional\] upload packages based on the release to the appropriate apparmor-dev ppa: https://launchpad.net/~apparmor-dev/+archive/apparmor-X.Y (where X.Y is the series version)
9.  Create a new release from the milestone: https://launchpad.net/apparmor/+milestone/X.Y.Z/+addrelease
    -   Once a release has been created in launchpad, files can be uploaded to it
10.  Upload the tarball and detached gpg signature to the launchpad page: https://launchpad.net/apparmor/X.Y/X.Y.Z/+adddownloadfile (where X.Y is the series and X.Y.Z is the specific version)
    -   The **Description** field should just be **AppArmor X.Y.Z**
11. [Branch the release](ReleaseProcess#Branching_Userspace_Trees) (For major releases only)
    -   For point releases (2.6.x) a new branch is unnecessary, as only stable patches are being applied. The tag is sufficient to identify the release points.
12. Update wiki
    -   Update the release notes. There are two separate entries for release notes:
        1.  A stub entry on the [release notes](AppArmor_versions) page.
            -   The release note stub should have its release date updated to the current date
            -   Move the stub from the under development section (bottom of the page) to the top of the Released version section. If this is a point release it should go under any new major release but above previous point releases. ie. the ordering is chronological by major release and then minor release.
            -   If the next release version is known start new entry for it in the under development section at the bottom of the page and create a new page by specifying the link and after saving click on the link and edit the page for the actual release note entries.
        2.  the full release notes for the release on their own page [Release\_Notes\_X.X.X](Release_Notes_X.X.X) (e.g. [Release\_Notes\_2.6.0](Release_Notes_2.6.0)):
            -   Previous release notes can be used as a template.
    -   Update current release information on the [main page](Main_Page#Userspace)
        -   if major release or point release for the most recent major release, update **Current stable release**
        -   if point release for an older release series, update **Prior supported release**
13. Mirror to kernel.org (require kernel.org account)
    -   Mirror release tarball
        -   If major release create a release directory in **/pub/linux/security/apparmor/**.
            -   minor releases use the major release directory, but have the point release as part of their name
        -   copy release tarball into the release directory, it should contain the release name. eg. apparmor-2.6.1.tar.gz
        -   copy in the md5 checksum into release directory into a file named after the release. eg. apparmor-2.6.1.md5
    -   Make sure git mirror of apparmor bzr tree is up to date
        -   update kernel.org git mirror of bzr ????
        -   **/pub/scm/linux/security/apparmor/**
    -   Make sure any additional kernel patches are synced to the apparmor kernel tree
        -   **/pub/scm/linux/kernel/git/jj/apparmor-dev.git/**
14. Send announcement of the release to the apparmor mailing list **apparmor@lists.ubuntu.com**
15. Make a copy of the announcement on the launchpad page at https://launchpad.net/apparmor/+announce
16. Go through bugs targeted to the milestone https://bugs.launchpad.net/apparmor/+milestone/X.Y.Z and either close them or move them to a different milestone if they were not fixed by this release

> ??? Register a new series in lp - when should this be done?  Before
> files can be uploaded to it, but can be after branch is created

> ??? setting expected and release dates in lp ??? what of next release
> series.  ie release 2.6, should a 2.6.1 series be created at that time
> as place holder for dev.

-   how are point releases different from major release

    -   only tag not branch
    -   wiki replace old point release as current version
        old point release doesn't become prev version?
    -   old version should be the most current version of
        the last major release

### Snapshots

To create a snapshot

1.  checkout (or make sure your local copy is up to date) the appropriate bzr branch
    -   `bzr co lp:~apparmor`
2.  make the release tarball
    -   in the base directory of the bzr tree type `make tarball`. This will create an appropriately named archive file. eg. apparmor-2.6.0.tar.gz
3.  upload the release tarball to launchpad
    -   In launchpad goto the release page pertaining to the snapshot being uploaded (from the main page click on the link to the release)
    -   click on the **Add download file** link
        -   In the **Description** field ????
        -   click the **Choose File** button and select the tarball
        -   **GPG Signature** ????
        -   Set **File content type** to **Code Release Tarball**
        -   click on the **upload** button

What of updating the release notes in lp - make sure the release
notes added have a link to the wiki release notes

Branching Userspace Trees
-------------------------

Some time after a new release series has been made (e.g. 2.6.0), an
official bzr branch should be made to capture fixes for future point
releases, leaving trunk available for larger scale development. This
does not need to occur immediately after the release, but should
be coordinated with the rest of the development team so that it's
understood that the only changes that should be committed to trunk
are the kinds of fixes that would be suitable for the point releases.

To do this, the following steps should be taken:

1.  Have an up to date checkout of the trunk
2.  tag the branchpoint on the trunk; e.g. `bzr tag apparmor_2.XX_branchpoint`
3.  make a local branch of your checkout; e.g. `bzr branch my_local_trunk apparmor-2.XX`
4.  change into the new branch directory; e.g. `cd apparmor-2.XX`
5.  push the branch to the apparmor project on launchpad; e.g. `bzr push lp:~apparmor-dev/apparmor/2.XX`
6.  Associate the branch with the release series: https://launchpad.net/apparmor/X.X/+setbranch (replace X.X with the release version, e.g. 2.6)
7.  I usually blow away the pushed branch locally and re-checkout the new branch to verify that the aliases are working right; e.g. `bzr co lp:apparmor/2.XX`
8.  Modify the bzr REPOURL location in the top level Makefile to match the new branch; make tarball needs this to be correct for subsequent releases off of the branch,
9.  edit common/Version to adjust in preparation for the XX+1 release with something like 2.XX.90 (git doesn't like '~' in tags, so versioning like 2.XY~pre1 are a bad idea)
10. send email to list **\[administrative\] AppArmor 2.XX branch created**, with branch url and note about needing nominations
