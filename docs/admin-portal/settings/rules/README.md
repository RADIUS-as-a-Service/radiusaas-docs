---
description: >-
  This is the documentation of the RADIUSaaS Rule Engine, which allows you to
  add another layer of security by defining rules that further restrict network
  access requests or by assigning VLAN IDs.
---

# Rules

## General&#x20;

All rules you have configured will be applied **after** successful credential authentication, which means the rules only become effective after valid authentication credentials have been provided. This implies, in order to pass the first authentication wall, valid **Trusted Roots** (certificate-based authentication) or **Users** (Username+Password-based authentication) have to be added to your instance.&#x20;

### Default rule

To avoid disruption of any existing instance or in case you do not want to use the Rule Engine at all, any authentication is allowed if no rule is defined by default. This is realized through our default rule **Any authentication allowed**.

{% hint style="warning" %}
The default rule **Any authentication allowed** still requires the presence of valid authentication credentials for a successful network authentication.
{% endhint %}

### Order of rule execution

If you have multiple rules configured, they will be applied in the order you see in your web portal - from top to bottom.&#x20;

The only exception is the **Any authentication allowed** rule, that will be handled as last step in case it is configured. This is especially helpful during a ramp-in scenario, where you might not be certain that your rules cover all use-cases or locations. All authentication request rejected by the prior rules will then still be accepted by the default rule. In the dashboard you are then able to observe the devices/users failing for all other rules and correct/extend the rules accordingly.&#x20;

In case you end up having a large number of rules, we recommend - for the sake of maintaining high performance - to order the rules in a way that the most likely rules are hit first.

<figure><img src="../../../../.gitbook/assets/image (435).png" alt=""><figcaption><p>Showing Rules</p></figcaption></figure>

## Rule options

### Authentication

* Allow only specific authentication sources
  * e.g. WiFi or LAN (VPN support is in progress)
* Allow only specific authentication types
  * e.g. Certificate or Username+Password

#### Certificate-based Authentication

* Allow only specific Trusted CAs (root or issuing CAs)
* Allow only specific Intune IDs or ignore the certificate attribute entirely

#### Username/password-based Authentication

* Only allow usernames that match a Regex-pattern

### Configuration

#### Infrastructure Constraints

* Define which **SSIDs** are allowed
* Define which **Access Points or Network Switches** are allowed (MAC-based)&#x20;

#### VLAN Assignment

Assign VLAN IDs ...

* Statically&#x20;
* By evaluating a custom **certificate extension**
* By parsing attributes in your certificate **subject name**

#### Additional RADIUS Return Attributes

By default, you can select from the following list of return attributes that you can return as part of a matching rule:

* Filter-Id
* Fortinet-Group-Name
* Framed-MTU
* Tunnel-Password
* Cisco-AVPair
* Class

{% hint style="info" %}
RADIUS return attributes allow network administrators to define specific settings for individual users or groups.

For example,

* For User Profile Configuration, an attribute can specify the maximum session duration, allowed services (such as VPN or Wi-Fi), and IP address assignment method.
* For Dynamic IP Address Assignment, an attribute might specify that the user should receive a static IP address or use DHCP for dynamic assignment.
* For Access Control and Authorization, an attribute determines the user’s access level (e.g., guest, employee, administrator) and any restrictions (e.g., time limits).
* For Session Management, an attribute can specify session timeout (how long the user can stay connected), idle timeout (disconnect after inactivity), and maximum simultaneous logins.
* For Quality of Service (QoS), an attribute might prioritize voice traffic over data traffic for a specific user.

Vendors can create their own custom attributes (vendor-specific attributes or VSAs). These allow for additional functionality beyond the standard IETF attributes. VSAs are encapsulated within the standard attribute 26.
{% endhint %}

Return additional RADIUS attributes ...

* Statically&#x20;
* By evaluating a custom **certificate extension**
* By parsing attributes in your certificate **subject name**
