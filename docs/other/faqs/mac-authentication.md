---
description: >-
  This article aims to clarify the difference between MAC Authentication Bypass
  (MAB) and MAC-based Authentication (MBA) as they relate to RADIUSaaS.
---

# MAC Authentication

## Overview

Customers typically want to authenticate their devices using certificates or username and password. There are, however, many devices that are unable to use modern authentication protocols either, because they are too old or it was not designed to participate in enterprise environments such as video game consoles. The technology that allows such devices to participate in an enterprise network can be either MAC Authentication Bypass (MAB) or MAC-based Authentication (MBA). Quite often these terminologies are used interchangeably, however, it's important to distinguish between them as they employ different techniques to accomplish the same goal. That is, to allow network access to clients that cannot speak 802.1X.

## Definitions

**MAC Authentication Bypass (MAB)** is a feature that allows devices to connect to a network by verifying their MAC address instead of requiring user or other credentials. It is commonly used for devices that cannot support standard authentication methods like 802.1X, such as older printers, PS5, Xbox or IoT devices. MAB works by allowing the switch / AP to check the device's MAC address against a predefined list and grant access if there is a match. This MAC address database can be stored on either the switch / AP or on a RADIUS server. In this case, there is no actual authentication taking place.&#x20;

**MAC-based Authentication (MBA)** is a feature in which, the MAC address of the client device is used as username & password in the authentication phase. A client that wants to have network access and  lacks 802.1X enterprise features needs to use its MAC address as credentials. The network equipment takes these credentials (Username = Password = MAC address) and forwards them on to the authentication server (RADIUS) as if it was the client. At this point a regular authentication takes place and if successful, the authenticator grants network access for the client.&#x20;

### How does MAB work in general?

When the MAC database is not maintained by the Switch / AP itself but by an external RADIUS server, the following sequence applies:

1. A client with MAC address 00:00:00:00:00:01 requests network access.
2. The Switch / AP will take this MAC address and forwards it to the RADIUS server in an Access-Request message, in which the MAC address is parsed into the **Called-Station-Id** RADIUS attribute-value-pair (AVP).
3. The RADIUS server checks its database for the MAC address received and will grant access if there is a match returning an Access-Accept message or Access-Reject if there is no such match.&#x20;
4. If Access-Accept is received from the RADIUS server, the Switch / AP will allow the client to join the network and receive DHCP configuration.&#x20;
5. The client can now connect to the Internet or denied access and placed on a restricted VLAN depending on the security policy.&#x20;

<img src="../../.gitbook/assets/file.excalidraw (3) (1).svg" alt="" class="gitbook-drawing">

### In practice

When an old printer that does not support 802.1X tries to request network access, the switch or AP will pretend to be that printer and takes over the authentication on behalf of the printer by authenticating to RADIUSaaS using one of the supported [protocols](https://docs.radiusaas.com/admin-portal/users#protocols). As part of this process it will check if the MAC address is listed on the RADIUSaaS database in the form of a manually added [User](../../admin-portal/users.md). (username = password = MAC address), If so, then an Access-Accept message is returned and the authentication completes giving the printer network access.

## Does MBA work with RADIUSaaS?

With the above definitions in mind, the current implementation of RADIUSaaS can support **MAC-based Authentication** **(MBA)** as long as your network equipment can communicate via one of [these ](https://docs.radiusaas.com/admin-portal/users#protocols)protocols.&#x20;

### Security considerations

In general, MAB provides little to no security and should not be used because MAC addresses can be spoofed easily. Because of this, MAB is not supported by RADIUSaaS. Other protocols such as PAP or CHAP are also not supported as they are considered weak.

### Configuration

MBA requires configuration in two places:&#x20;

1. On the **authenticator**. These are your switches and/or AP. Because configuring your devices is vendor specific, please consult your documentation in this regard.
2.  On **RADIUSaaS**. This is your authentication server. To support MBA, you will need to [add](../../admin-portal/users.md#add) users to your RADIUSaaS instance. These users will need to be formatted as username = password = MAC address. If you have multiple users, you could speed this process up by [importing them via CSV](../../admin-portal/users.md#csv-import) file. The delimiter for the MAC address can usually be defined on your authenticator, however, we recommend to use the colon notation.&#x20;

    <figure><img src="../../../.gitbook/assets/image (465).png" alt=""><figcaption><p>Showing username and password as MAC address</p></figcaption></figure>

