# Warning this document is a WIP

## to figure out

- default allow if no rules specified (ie no additional restrictions)
  - if env rules present?
    - default deny, white list. like rest of policy?
    - default allow, black list

- anchoring? vs. contains
- how to identify subpattern in rule that should be filtered

# Introduction

Environment variables can be used to alter program behavior and have been leveraged to exploit programs. As such some environment variables are dangerous and must be handled carefully. AppArmor provides several ways to make sure the applications environment is safe

# glibc safe exec environment scrubbing

safe/unsafe version of exec rules, environment is scrubbed by glibc, list changes depending on version of glibc, may not be scrubbed if different libc is used.

# Profile environment variable mediation and scrubbing

Because libc environment scrubbing is insufficient in many cases (eg. interpreters have their own special environment variables that libc doesn't know about), AppArmor provides a means to specify what the environment should look like when an application starts running.

AppArmor can filter out environment variables, filter out parts of environment variables values, deny exec based on the presence of an environment variable or its values, and can even set or modify environment variables.

It is important to note that AppArmor environment variable filtering is only applied at exec time and does not mediate what a task does to its environment at run time.

## environment rules

Environment controls begin with the keyword ```environment``` followed by an individual rule or a block of rules. If a block of rules is used only environment rules may appear in the block. Multiple rules or blocks can be declared and as other AppArmor rules the affects are declarative (order doesn't matter) and cumulative. As a matter of style it is recommend to keep environment variables together in a block at the top of the profile, to make it easier for people to understand but multiple rules spread throughout the profile are allowed to fascilitate their use in abstractions.

The basic structure of an environment rule is as follows.

 ```
   ENVIRONMENT_RULE := ( AUDIT_QUALIFIER QUALIFIER 'environment' VARIABLES ('='VALUES)? [ ':=' VALUE ] ',' | 'environment '{' ( AUDIT_QUALIFIER QUALIFIER VARIABLES ['=' VALUES] [ ':=' VALUE ] ',' )* '}' )

   AUDIT_QUALIFIER := (audit|quiet|access)
   QUALIFIER := (allow|deny|require|filter|delete|set)
   VARIABLES := pattern of variable names
   VALUES := pattern of variable values that the rule matches
 ```

it is important to note that if the optional VALUES are not specified the rule will match the variable regardless of the value.

The qualifiers that can be used are

- allow - if the rule is matched the environment variable will be allowed
- deny - if the rule is matched execution will be denied
- require - if the rule is not matched execution will be denied
- filter - if the rule is matched the variable's value will be filtered . ???Filter part of rule
- delete - if the rule is matched the variable will deleted from the environment.
- set - if the rule is matched the variable will be set to the provided value

??? IF no environment rules default allow, else default deny ???

???allow do we make env rules white listing + black list

??? anchored vs. unanchored expressions
= using ^ and $ vs contains


???? use remove and filter or filter value? ??? to indicate whether to remove the var or delete the value.

???? what of matching portion, we need follow on/tail to match do match but grouping to find value to remove
eg. a section of the path
  PATH contains \^(@{HOME}/+bin/*.*(:|$)|:.*(@{HOME}/+bin/*.*(:|$))

we could add ^$ anchors to expressions


maybe special keyword for PATH elements because it is so common


### Examples

Deny execution if the PATH variable is present in the environment

```
profile foo {

   deny environment PATH,
}
```

---

Deny execution if the users PATH is present in the the path environment variable.

```
profile foo {
   deny environment PATH contains @{HOME}{:*,},

}
```

---

Remove the PATH variable if present

```
profile foo {

   filter environment PATH,
}
```

---

Remove the PATH variable if it contains @{HOME} in the path value

```
profile foo {
   filter environment PATH contains @{HOME}{:*,},
}
```

---

Filter the portion of the PATH variable that matches

```
profile foo {
   filter environment PATH contains @{HOME}{:*,}, ?????
}
```


```
profile foo {
   environment {
      # make this a black list
      allow  *,
      deny LD_PRELOAD,
      delete PYTHON_PATH,
      filter PATH=?????,
      require HOME,
      set APPARMOR_SESSION := yes,
   }
}
```

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


