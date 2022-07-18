---
description: >-
  This is the documentation for our RADIUSaaS Rule Engine, through which you can
  add an additional layer of security by defining rules that further restrict
  network access or by assigning VLAN IDs.
---

# Rules

## General&#x20;

All rules you have configured will be applied **after** successful credential authentication, which means the rules only become effective after valid authentication credentials have been provided. This implies, in order to pass the first authentication wall, valid **Trusted Roots** (certificate-based authentication) or **Users** (Username+Password-based authentication) have to be added to your instance.&#x20;

### Default Rule

To avoid disruption of any existing instance or in case you do not want to use the Rule Engine at all, any authentication is allowed if no rule is defined by default. This is realized through our default rule "**Any authentication allowed**".

{% hint style="warning" %}
The default rule "**Any authentication allowed**" still requires the presence of valid authentication credentials for a successful network authentication.
{% endhint %}

### Order of Rule Execution

If you have multiple rules configured, they will be applied in the order you see in your web portal - from top to bottom.&#x20;

The only exception is the "**Any authentication allowed**" rule, that will be handled as last step in case it is configured. This is especially helpful during a ramp-in scenario, where you might not be certain that your rules cover all use-cases or locations. All authentication request rejected by the prior rules will then still be accepted by the default rule. In the dashboard you are then able to observe the devices/users failing for all other rules and correct/extend the rules accordingly.&#x20;

In case you end up having a large number of rules, we recommend - for the sake of maintaining high performance - to order the rules in a way that the most likely rules are hit first.

![](<../../../.gitbook/assets/image (73).png>)

## Rule Options - Overview

### Authentication

* Allow only specific authentication sources
  * e.g. WiFi or LAN (VPN support is in progress)
* Allow only specific authentication types
  * e.g. Certificate or Username+Password

#### Certificate-based Authentication

* Allow only specific Trusted Roots
* Allow only specific Intune IDs or ignore the certificate attribute entirely

#### Username+Password-based Authentication

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
