# Warning this document is a WIP

# Introduction

Environment variables can be used to alter program behavior and have been leveraged to exploit programs. As such some environment variables are dangerous and must be handled carefully. AppArmor provides several ways to make sure the applications environment is safe

# glibc safe exec environment scrubbing

safe/unsafe version of exec rules, environment is scrubbed by glibc, list changes depending on version of glibc, may not be scrubbed if different libc is used.

# Profile environment variable mediation and scrubbing

Because libc environment scrubbing is insufficient in many cases (eg. interpreters), AppArmor provides a means to specify what the environment should look like when an application starts running.

AppArmor can filter out environment variables, filter out parters of environment variables values, deny exec based on the presence of an environment variable or its values, and can even set or modify environment variables.

It is important to note that AppArmor environment variable filtering is only applied at exec time and does not mediate what a task does to its environment at run time.

profile python /usr/bin/python {

  environment {
       deny XFOO,
       deny XBAR=x*,
       filter XFRED,
       filter XGEORGE=x*,
       filter PATH in? x*
       set PATH=FOO
       append PATH=BAR
       prepend PATH=BAR
       replace PATH in X* -> foo,

  }

}

# Exec rule environment modification

In addition to being able to specify environment controls on a per profile basis, AppArmor allows individual exec rules to specify the same type of mediation and modification. This allows policy to take into account exec relationships when setting up environment variable mediation.

It is important to note that exec rule environment mediation is applied before profile based environment mediation.

??? way to override


