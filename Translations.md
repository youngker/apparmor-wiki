This page documents the process for handling translations in the
AppArmor user space tools development.

Overview
--------

Translations are handled using [GNU
gettext](http://www.gnu.org/software/gettext/manual/). The specific
tools that need translation services will have a po/ subdirectory
containing the template (.pot) file and the language specific .po
files. These are compiled into the .mo files for installation. The
translation .po files are hand-editable, although specialized utilities
like [Poedit](http://www.poedit.net/) are available.

Launchpad Translations
----------------------

AppArmor is also a registered project in [Launchpad's Translation
service](https://translations.launchpad.net/), enabling the people
who have volunteered to perform translations through Launchpad to
contribute translations to the AppArmor project.

[AppArmor translations overview
page](https://translations.launchpad.net/apparmor/master) (for the
apparmor trunk branch)

The launchpad translations project is
configured to automatically commit to a separate bzr branch,
[lp:~apparmor-dev/apparmor/launchpad-translations-branch](https://code.launchpad.net/~apparmor-dev/apparmor/launchpad-translations-branch).
Changes coming from this branch will periodically need to be merged
over to the apparmor trunk branch. Changes made to the translations
on the apparmor trunk branch will automatically be imported to the
launchpad translations project as each commit happens.

[Launchpad Translations help
page](https://help.launchpad.net/Translations)

Performing Merges from Launchpad
--------------------------------

To merge from the Launchpad translations branch stored in bzr into the master git tree, the following steps need to be taken:

*  Ensure that a bzr remote plugin is installed for use from within git. On an Ubuntu system, this would be the bzr-git package; unfortunately, this has been broken in the current (as 2020.02.17) 20.04 release's transition to breezy, so an 18.04 host/vm is needed.
*  Checkout the translations branch in git: 
```bash
$ git clone bzr::lp:~apparmor-dev/apparmor/launchpad-translations-branch/ apparmor-lp-translations
```
*  Find the first commit that is newer than what has been previously merged and export the patches:
```bash
$ git format-patch --output-directory ~/patches/apparmor-translations 1b977565c519bfa382dc9d8a142696f86f2de5e2..
```
*  Switch to your apparmor git tree and create/checkout a translations branch
```bash
$ git checkout -b translations master
```
*  Import the patches into your branch:
```bash
$ git am -s ~/patches/apparmor-translations/*
```
*  Check that the commit history looks sane, push to a personal repository on gitlab to ensure that it passes CI
*  Once you're happy, merge the translations to master:
```bash
$ git checkout master
$ git merge --signoff --no-ff  translations
```