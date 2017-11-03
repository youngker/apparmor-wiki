What are Launchpad and Bazaar
=============================

Launchpad is a web based platform for software development and code hosting.

Bazaar (also known as bzr) is a distributed version control system
that is designed to be easy to use. It is the version control system
of choice for launchpad and provides good integration with Launchpad.

More recently, Launchpad has added support for the git version control system.

Downloading AppArmor
====================

Downloading tarballs
--------------------

The current release tarballs can be found in
[launchpad](https://launchpad.net/apparmor), by selecting
the series and then from the series page selecting the desired
download.

Checking out the source tree with bazaar
----------------------------------------

To get an svn style checkout of the current development branch do

` bzr co lp:apparmor`

to create a local branch

` bzr branch lp:apparmor`

Checking out the apparmor source tree with git
----------------------------------------------

There is work underway to move the AppArmor source trees to
git. Otherwise, you can try to use git-bzr-ng as described in the
Old Stuff section.

Checking out the apparmor-profiles repository with git
------------------------------------------------------

The [AppArmor Profiles](https://launchpad.net/apparmor-profiles)
repository is now hosted with git on Launchpad. To check out (aka
clone):

` git clone `[`https://git.launchpad.net/apparmor-profiles`](https://git.launchpad.net/apparmor-profiles)

AppArmor developers who have commit/push rights will probably want to use the git+ssh method instead:

` git clone git+ssh://LAUNCHPAD_USERNAME@git.launchpad.net/apparmor-profiles`

(See [Configuring git with
Launchpad](https://help.launchpad.net/Code/Git#Configuring_Git) to
configure git to mimic the lp:apparmor-profiles bzr style references.)

Making a merge proposal to apparmor-profiles with launchpad and git
-------------------------------------------------------------------

First, clone the master repo:

` git clone `[`https://git.launchpad.net/apparmor-profiles`](https://git.launchpad.net/apparmor-profiles)

Make a branch, and commit your changes to it:

` git branch -b BRANCH_NAME`

(Hack away with tool of choice)

` git commit`

Then, push your changes to launchpad. To do so:

-   add a remote repository under that is associated with the apparmor-profiles project:

` git remote add REMOTE_NAME git+ssh://LAUNCHPAD_USERNAME@git.launchpad.net/~LAUNCHPAD_USERNAME/apparmor-profiles/+git/YOUR_REPO_NAME`

(`YOUR_REPO_NAME` can be something simple like `apparmor-profiles`. `REMOTE_NAME` should be something to let you know it points to an different location from the primary repo on Launchpad.)

-   push your branch

` git push REMOTE_NAME BRANCH_NAME`

It should show up shortly at

` `[`https://code.launchpad.net/~LAUNCHPAD_USERNAME/apparmor-profiles/+git/YOUR_REPO_NAME`](https://code.launchpad.net/~LAUNCHPAD_USERNAME/apparmor-profiles/+git/YOUR_REPO_NAME)

and the specific branch should be available from

` `[`https://code.launchpad.net/~LAUNCHPAD_USERNAME/apparmor-profiles/+git/YOUR_REPO_NAME/+ref/BRANCH_NAME`](https://code.launchpad.net/~LAUNCHPAD_USERNAME/apparmor-profiles/+git/YOUR_REPO_NAME/+ref/BRANCH_NAME)

From the latter page, there should be an option under the section *Branch Merges* to **Propose for Merging**, which should point to

` `[`https://code.launchpad.net/~LAUNCHPAD_USERNAME/apparmor-profiles/+git/YOUR_REPO_NAME/+ref/BRANCH_NAME/+register-merge`](https://code.launchpad.net/~LAUNCHPAD_USERNAME/apparmor-profiles/+git/YOUR_REPO_NAME/+ref/BRANCH_NAME/+register-merge)

This page will let you propose a merge to the main project. The *Target Reference Location* is a fancy name for the main project's branch you wish to merge to; for the main apparmor-profiles development branch, you would enter *master* into this field.

Merging a proposal into the apparmor-profiles repo
--------------------------------------------------

**NOTE: for apparmor-profiles maintainers**

Once a merge proposal is in shape to be merged, do the following with git.

-   add the merging branch as a remote to your git tree:

` git remote add REMOTE_NAME git+ssh://YOUR_LAUNCHPAD_USERNAME@git.launchpad.net/~THEIR_LAUNCHPAD_USERNAME/apparmor-profiles/+git/THEIR_REPO_NAME`

-   checkout their branch and review the commits:

` git checkout REMOTE_NAME/BRANCH_NAME`

-   switch back to *master* and merge their changes:

` git checkout master`
` git merge --no-ff REMOTE_NAME/BRANCH_NAME`

(The `--no-ff` will force a seperate merge commit, helping to clarify who merged/approved the branch. Include a short description, any bug reference urls “Bug: <https://somesite/bug/123456/>”, and Acked-Bys as need be in the merge commit message.)

-   Once you're happy with the merge and your commit message (`git commit --amend` will let you edit your commit message), push the commit to launchpad:

` git push origin master`

That's it! Launchpad will notice the merge and mark the state of the merge proposal as *Merged*. You can delete the remote from the merge requestor from your git tree, or you can keep it around if you expect the proposer to future merge requests.

Links
=====

Launchpad help
--------------

-   [Launchpad](https://help.launchpad.net/)

Bazaar
------

-   [bazaar](http://bazaar.canonical.com/en/)
-   [Arstechnica:An introduction to collaborative development with Launchpad](http://arstechnica.com/business/2010/02/an-introduction-to-collaborative-development-with-launchpad.ars)

git
---

-   [Using git with Launchpad](https://help.launchpad.net/Code/Git)
-   [Pro Git book online](https://git-scm.com/book/en/v2)

Old Stuff
=========

The apparmor development repository is not currently mirrored with git. There is an effort underway to make this happen again

Checking out the source tree using git + git-bzr-ng
---------------------------------------------------

It is possible to checkout the userspace tools using git + git-bzr-ng

<https://github.com/termie/git-bzr-ng/>

install git-bzr-ng copy into git commands

`on Ubuntu cp git-bzr.py /usr/lib/git-core/git-bzr`
`chmod +x /usr/lib/git-core/git-bzr`

There is a known common issue with bzr-fast-import that can cause git-bzr-ng to fail with newer versions of bzr

-   <https://github.com/termie/git-bzr-ng/issues/38>
-   tracking bug in launchpad <https://bugs.launchpad.net/bzr/+bug/541626>

To fix the issue we need to grab the patch from the end of the bug and patch bzr-fast-import. The patch can be found at the link below, click on the “download diff” link.

-   <http://bazaar.launchpad.net/~larstiq/bzr/bug541626/revision/6524>

patch the bzrlib/btree\_index.py on your system

-   on Ubuntu it can be found at /usr/share/pyshared/bzrlib/btree\_index.py

Clone the repository

`git bzr clone lp:apparmor`

` git clone `[`git://git.kernel.org/pub/scm/linux/security/apparmor/apparmor-bzr-mirror.git`](git://git.kernel.org/pub/scm/linux/security/apparmor/apparmor-bzr-mirror.git)

brief step by step of using bzr with AA
