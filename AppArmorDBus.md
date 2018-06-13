wip

# Requirements

# Introduction

The DBus daemon has been extended to be an AppArmor trusted helper. It is trusted with handling the mediation of dbus messages between applications. DBus policy is integrated into regular AppArmor policy, that is compiled and loaded into the kernel.

The DBus daemon does not load policy
DBus activation/launcher ???


# Building

???

# Enabling

???

# Policy

DBUS RULE = ( DBUS MESSAGE RULE | DBUS SERVICE RULE | DBUS
           EAVESDROP RULE | DBUS COMBINED RULE )

           DBUS MESSAGE RULE = [ QUALIFIERS ] 'dbus' [ DBUS ACCESS EXPRESSION
           ] [ DBUS BUS ] [ DBUS PATH ] [ DBUS INTERFACE ] [ DBUS MEMBER ] [
           DBUS PEER ]

           DBUS SERVICE RULE = [ QUALIFIERS ] 'dbus' [ DBUS ACCESS EXPRESSION
           ] [ DBUS BUS ] [ DBUS NAME ]

           DBUS EAVESDROP RULE = [ QUALIFIERS ] 'dbus' [ DBUS ACCESS
           EXPRESSION ] [ DBUS BUS ]

           DBUS COMBINED RULE = [ QUALIFIERS ] 'dbus' [ DBUS ACCESS EXPRESSION
           ] [ DBUS BUS ]

           DBUS ACCESS EXPRESSION = ( DBUS ACCESS | '(' DBUS ACCESS LIST ')' )

           DBUS BUS = 'bus' '=' '(' 'system' | 'session' | '"' AARE '"' | AARE
           ')'

           DBUS PATH = 'path' '=' '(' '"' AARE '"' | AARE ')'

           DBUS INTERFACE = 'interface' '=' '(' '"' AARE '"' | AARE ')'

           DBUS MEMBER = 'member' '=' '(' '"' AARE '"' | AARE ')'

           DBUS PEER = 'peer' '=' '(' [ DBUS NAME ] [ DBUS LABEL ] ')'

           DBUS NAME = 'name' '=' '(' '"' AARE '"' | AARE ')'

           DBUS LABEL = 'label' '=' '(' '"' AARE '"' | AARE ')'

           DBUS ACCESS LIST = Comma separated list of DBUS ACCESS

           DBUS ACCESS = ( 'send' | 'receive' | 'bind' | 'eavesdrop' | 'r' |
           'read' | 'w' | 'write' | 'rw' )
             Some accesses are incompatible with some rules; see below.


     AppArmor supports DBus mediation. The mediation is performed in
       conjunction with the DBus daemon. The DBus daemon verifies that
       communications over the bus are permitted by AppArmor policy.

       AppArmor DBus rules are accumulated so that the granted DBus
       permissions are the union of all the listed DBus rule permissions.

       AppArmor DBus rules are broad and general and become more restrictive
       as further information is specified. Policy may be specified down to
       the interface member level (method or signal name), however the
       contents of messages are not examined.

       Some AppArmor DBus permissions are not compatible with all AppArmor
       DBus rules.  The 'bind' permission cannot be used in message rules. The
       'send' and 'receive' permissions cannot be used in service rules. The
       'eavesdrop' permission cannot be used in rules containing any
       conditionals outside of the 'bus' conditional.

       'r' and 'read' are synonyms for 'receive'. 'w' and 'write' are synonyms
       for 'send'. 'rw' is a synonym for both 'send' and 'receive'.

       AppArmor DBus permissions are implied when a rule does not explicitly
       state an access list. By default, all DBus permissions are implied.
       Only message permissions are implied for message rules and only service
       permissions are implied for service rules.

       Example AppArmor DBus rules:

           # Allow all DBus access
           dbus,

           # Explicitly allow all DBus access,
           dbus (send, receive, bind),

           # Deny send/receive/bind access to the session bus
           deny dbus bus=session,

           # Allow bind access for a particular name on any bus
           dbus bind name=com.example.ExampleName,

           # Allow receive access for a particular path and interface
           dbus receive path=/com/example/path interface=com.example.Interface,

           # Deny send/receive access to the system bus for a particular interface
           deny dbus bus=system interface=com.example.ExampleInterface,

           # Allow send access for a particular path, interface, member, and pair of
           # peer names:
           dbus send
                bus=session
                path=/com/example/path
                interface=com.example.Interface
                member=ExampleMethod
                peer=(name=(com.example.ExampleName1|com.example.ExampleName2)),

           # Allow receive access for all unconfined peers
           dbus receive peer=(label=unconfined)),

           # Allow eavesdropping on the system bus
           dbus eavesdrop bus=system,

           # Allow and audit all eavesdropping
           audit dbus eavesdrop,


# Querying DBus Peer Security Context

The apparmor security context of a peer's connection can be found
using the org.freedesktop.DBus.GetConnectionCredentials() bus method.

Example. in C++

```
  struct BusDaemon {
    static const std::string &name() {
        static std::string s = “org.freedesktop.DBus”;
        return s;
    }
    struct GetConnectionCredentials {
        typedef BusDaemon Interface;
        static const std::string &name() {
            static std::string s = “GetConnectionCredentials”;
            return s;
        }
        static const std::chrono::milliseconds default_timeout() {
            return std::chrono::seconds{1};
        }
    };
  };
  std::string get_client_apparmor_context(const [Message::Ptr](Message::Ptr) &message) {
        if (!aa_is_enabled()) {
            return “unconfined”;
        }
        auto service = core::dbus::[Service::use_service](Service::use_service)(
            impl->access_bus(), “org.freedesktop.DBus”);
        auto obj = service->object_for_path(
            ObjectPath(“/org/freedesktop/DBus”));
        core::dbus::Result<std::map<std::string,Variant>> result;
        try {
            result = obj->invoke_method_synchronously<BusDaemon::GetConnectionCredentials,std::map<std::string,Variant>>(message->sender());
        } catch (const std::exception &e) {
            fprintf(stderr, “Error getting connection credentials: %s\n”, e.what());
            return std::string();
        }
        if (result.is_error()) {
            fprintf(stderr, “Error getting connection credentials: %s\n”, result.error().print().c_str());
            return std::string();
        }
        const auto& creds = result.value();
        auto it = creds.find(“LinuxSecurityLabel”);
        if (it == creds.end()) {
            fprintf(stderr, “Connection credentials don't include security label”);
            return std::string();
        }
        std::vector<int8_t> label;
        try {
            label = it->second.as<std::vector<int8_t>>();
        } catch (const std::exception &e) {
            fprintf(stderr, “Could not convert security label to byte array”);
            return std::string();
        }
        return std::string(aa_splitcon(reinterpret_cast<char*>(&label[0]), nullptr));
    }
```

# Implementation details

## Kernel Flag

## Query interface

## Auditing

## Caching

## rule encoding
[rule encoding](Technicaldoc_dbusruleencoding)
