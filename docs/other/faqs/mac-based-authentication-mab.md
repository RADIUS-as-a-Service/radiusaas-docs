# MAC-based authentication

## Overview

In some cases, devices simply do neither allow to use certificates nor the configuration of username/password for 802.1X. The only chance to "authenticate" such devices, is to allow-list the set of eligible client devices by means of their MAC address. This concept is called _"MAC-based authentication"_ or _"MAC Authentication Bypass (MAB)"_.

## Definitions

### MAC Authentication Bypass (MAB)

MAC Authentication Bypass (MAB) means, that the network equipment (NE - e.g. switch or WiFi controller) allows certain clients to connect to the network without authenticating via 802.1X. These clients are "allowed to bypass the authentication", so to say.

To identify, which clients are allowed to connect to the network without authentication, the NE evaluates the MAC address of the requesting client. Therefore the NE compares the MAC address of the client with a list of MAC addresses defined by the administrator. This list of MAC addresses may be stored in:

* network equipment (e.g. switch or WiFi-controller)\
  or
* RADIUS server (MAC-based RADIUS)

### MAC-Based RADIUS

When using MAC-Based RADIUS, the list of allowed MAC addresses is stored in the RADIUS server. Every time a client tries to connect to the network without 802.1X authentication, the network equipment sends a RADIUS access request to the RADIUS server.&#x20;

As credentials it uses the client's MAC address as username and as password.

## Security Considerations

The MAC-based authentication has just a very basic level of security. It can be overcome very easily.

Here are our recommendations:

* Please use MAC-based authentication only if absolutely necessary
* Segment MAC-based authentication devices from others (e.g. by assigning a dedicated VLAN with special security treatment)

## How to implement with RADIUSaaS

### Configure your Network Equipment

Please refer to the documentation of your network equipment to configure support for MAC-based RADIUS authentication.

For Meraki devices you may want to have a look [here](https://documentation.meraki.com/MR/Encryption\_and\_Authentication/Enabling\_MAC-based\_access\_control\_on\_an\_SSID).

### Add Users/MAC Addresses in RADIUSaaS

Add the MAC credentials (client MAC address = username = password) as [regular users](../../portal/users.md) in RADIUSaaS. The format of the credentials depends on the network equipment. In many cases (e.g. Meraki) it is lower case and without delimiting characters.

To bulk-import users you may want to use the [CSV import feature in RADIUSaaS](https://docs.radiusaas.com/portal/users#csv-import).

### Segment Devices

You can use the [rules ](../../portal/settings/rules/)in RADIUSaaS to segment MAC-based authentication devices from others by defining dedicated VLANs and/or SSIDs.

### Limit to dedicated WiFi APs

In a [WiFi rule](../../portal/settings/rules/wifi.md), you can optionally limit the MAC-based authentication to certain access points by setting the "Which Access Point MAC addresses are allowed".
