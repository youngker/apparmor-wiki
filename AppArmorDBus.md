wip

# Requirements

# Introduction

AppArmor supports DBus mediation. The mediation is performed in conjunction with the DBus daemon. The DBus daemon verifies that communications over the bus are permitted by AppArmor policy.

The DBus daemon does not load policy
DBus activation/launcher ???


# Building

dbus can be built using autotools or cmake

## autotools
When configuring and building with autotools pass --enable-apparmor as an option to ./configure

Also enabling the option --enable-libaudit is also suggested. 

## cmake

todo

# Enabling

If dbus is built with apparmor support then apparmor support can be configured in the dbus configuration file with configuration element.

apparmor mode=(disabled,enabled,required)

The default mode is "enabled". In "enabled" mode, AppArmor mediation will be performed if AppArmor support is available in the kernel. If it is not available, dbus-daemon will start but AppArmor mediation will not occur. In "disabled" mode, AppArmor mediation is disabled. In "required" mode, AppArmor mediation will be enabled if AppArmor support is available, otherwise dbus-daemon will refuse to start.

The AppArmor mediation mode of the bus cannot be changed after the bus starts. Modifying the mode in the configuration file and sending a SIGHUP signal to the daemon has no effect on the mediation mode.

no dbus policy rules needed as apparmor dbus rules are part of apparmor policy files.

# System Service Activation

todo

Use regular profile attachment

or

Define a profile for dbus daemon that over rides attachments with exec rules

# Policy
AppArmor DBus policy is integrated into regular AppArmor policy. The DBus rules follow standard policy conventions that is they are accumulated so that the granted DBus permissions are the union of all the listed DBus rule permissions.

AppArmor DBus rules are broad and general and become more restrictive as further information is specified. Policy may be specified down to the interface member level (method or signal name), however the contents of messages are not examined.

Some AppArmor DBus permissions are not compatible with all AppArmor DBus rules.  The 'bind' permission cannot be used in message rules. The 'send' and 'receive' permissions cannot be used in service rules. The 'eavesdrop' permission cannot be used in rules containing any conditionals outside of the 'bus' conditional.

 'r' and 'read' are synonyms for 'receive'.'<br><br>
 'w' and 'write' are synonyms for 'send'.<br><br>
 'rw' is a synonym for both 'send' and 'receive'.<br><br>

AppArmor DBus permissions are implied when a rule does not explicitly state an access list. By default, all DBus permissions are implied. Only message permissions are implied for message rules and only service permissions are implied for service rules.

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

## DBus rule syntax
 DBUS RULE = ( DBUS MESSAGE RULE | DBUS SERVICE RULE | DBUS EAVESDROP RULE | DBUS COMBINED RULE )

 DBUS MESSAGE RULE = [ QUALIFIERS ] 'dbus' [ DBUS ACCESS EXPRESSION ] [ DBUS BUS ] [ DBUS PATH ] [ DBUS INTERFACE ] [ DBUS MEMBER ] [DBUS PEER ]

 DBUS SERVICE RULE = [ QUALIFIERS ] 'dbus' [ DBUS ACCESS EXPRESSION ] [ DBUS BUS ] [ DBUS NAME ]

 DBUS EAVESDROP RULE = [ QUALIFIERS ] 'dbus' [ DBUS ACCESS EXPRESSION ] [ DBUS BUS ]

 DBUS COMBINED RULE = [ QUALIFIERS ] 'dbus' [ DBUS ACCESS EXPRESSION ] [ DBUS BUS ]

 DBUS ACCESS EXPRESSION = ( DBUS ACCESS | '(' DBUS ACCESS LIST ')' )

 DBUS BUS = 'bus' '=' '(' 'system' | 'session' | '"' AARE '"' | AARE ')'

 DBUS PATH = 'path' '=' '(' '"' AARE '"' | AARE ')'

 DBUS INTERFACE = 'interface' '=' '(' '"' AARE '"' | AARE ')'

 DBUS MEMBER = 'member' '=' '(' '"' AARE '"' | AARE ')'

 DBUS PEER = 'peer' '=' '(' [ DBUS NAME ] [ DBUS LABEL ] ')'

 DBUS NAME = 'name' '=' '(' '"' AARE '"' | AARE ')'

 DBUS LABEL = 'label' '=' '(' '"' AARE '"' | AARE ')'

 DBUS ACCESS LIST = Comma separated list of DBUS ACCESS

 DBUS ACCESS = ( 'send' | 'receive' | 'bind' | 'eavesdrop' | 'r' | 'read' | 'w' | 'write' | 'rw' )

    * Some accesses are incompatible with some rules; see below.

## DBus binary policy encoding
DBus policy is encoded similar to kernel based AppArmor policy where it is encoded as part of the [policydb](Technicaldoc_policydb). This means that it is loaded into the kernel and can be queried using the apparmor query and instrospection infrastructure.

For a full description of its layout see [dbus rule encoding in the Technical documentation](Technicaldoc_dbusruleencoding).


# Querying DBus Peer Security Context

The apparmor security context of a peer's connection can be found
using the org.freedesktop.DBus.GetConnectionCredentials() bus method. Older version of Ubuntu use the Ubuntu specific org.freedesktop.DBus.GetConnectionAppArmorSecurityContext bus method instead.

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
