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

???

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
