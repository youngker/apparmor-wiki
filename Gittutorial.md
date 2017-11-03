links to git documentation/tutorials...

The AppArmor kernel module can be found in **Linux 2.6.36** in the security/apparmor directory. Please read the [notes](apparmor/upstream_release_notes) about using the upstream version of AppArmor as it may not be the same as development or previous versions.

The development code for AppArmor can be found at

**<git://git.kernel.org/pub/scm/linux/kernel/git/jj/linux-apparmor>**

Their are multiple branches in the git tree with some of them for
development and others as stable branches. Development branches
see frequent rebasing and amending of commits and as such should be
generally be avoided except for by developers. The stable branches can
be reliably tracked as they are never rebased (hashes remain stable)
and only new bug fix commits are added.

-   master - development branch and frequently is rebased or has commits amended

Stable branches (kernel version - apparmor version)

-   v3.0-aa2.6
-   v3.1-aa2.6
-   v3.2-aa2.7
-   v3.3-aa2.7
-   v3.3-aa2.8
-   v3.4-aa2.8

to obtain the AppArmor source tree from a command line you can do

` git clone `[`git://git.kernel.org/pub/scm/linux/kernel/git/jj/linux-apparmor`](git://git.kernel.org/pub/scm/linux/kernel/git/jj/linux-apparmor)

if you have another kernel source tree present it is well worth using the --reference option.

to obtain a branch in the AppArmor source tree from the local AppArmor git repository do

` git checkout --track -b `<branch_name>` origin/`<branch_name>
` eg. for the AA2.5-2.6.33 branch you would do`
`    git checkout --track origin/v3.4-aa2.8 -b v3.4-aa2.8 `

Some development and patches may exist only in the Ubuntu kernels, the patches are managed via a kernel.ubuntu.com tree:

` git clone `[`git://kernel.ubuntu.com/jj/ubuntu-saucy.git`](git://kernel.ubuntu.com/jj/ubuntu-saucy.git)

brief step by step of getting AA with git
