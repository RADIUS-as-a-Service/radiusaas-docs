---
description: >-
  This page answers questions on the possibility to perform MAC-based
  authentication with RADIUSaaS.
---

# MAC Authentication Bypass (MAB)

{% hint style="warning" %}
It is our general recommendation to - whenever possible - leverage certificates for authentication. If this is not possible, e.g. due to technical or practical reasons, employing username/password-based authentication should be the second choice.
{% endhint %}

In some cases, devices simply do neither allow to use certificates for 802.1X nor the configuration of username/password credentials. In those instances, the only chance to "authenticate" such devices, is to allow-list the set of eligible client devices by means of their MAC address. This can typically be done directly on the access point or switch.

If the network infrastructure is large and distributed, it can be beneficial to use a central database that contains the MAC addresses of all devices on the permission list. This is where RADIUSaaS can help: By default, many devices submit their MAC address as their identity (i.e., username) and also populate the associated password property with their MAC address. If this is the case, and the access point/switch forwards this information, the RADIUSaaS platform can create [username/password](../../portal/users.md) accounts where the username and password match the MAC address of the device to be allowed on the list. Pseudo-authentication through RADIUSaaS is possible when the client speaks one of the [supported EAP protocols](../../portal/users.md#protocols). In addition, the [Rule Engine](../../portal/settings/rules/) can be used to place devices that use MAB in a specific VLAN (e.g., regex the username for a MAC address pattern).

{% hint style="info" %}
In some cases, forwarding the device's MAC address as identity and password must explicitly be enabled on the access point or switch. Here is an [example](https://documentation.meraki.com/MR/Encryption\_and\_Authentication/Enabling\_MAC-based\_access\_control\_on\_an\_SSID) from Meraki's documentation.
{% endhint %}
