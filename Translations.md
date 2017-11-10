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
