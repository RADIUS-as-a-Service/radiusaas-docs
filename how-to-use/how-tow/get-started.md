---
description: >-
  This page is a Step-by-Step guide to get your clients authenticated after
  you've received the details for your instance
---

# Getting Started

{% hint style="success" %}
Before you can really start using our service, two things are are **very** important to find out:&#x20;

1. Does your WiFi vendor support [RadSec](../../details.md#what-is-radsec)?
2. Which type of certificates are you currently deploying? User or device certificates?
{% endhint %}

### 1. RADIUS Server&#x20;

#### If your vendor supports [RadSec](../../details.md#what-is-radsec):

1. Add the certificate of your WiFi infrastructure to your RADIUS instance as described [here](../../portal/settings-server.md#add).
2. Download the active Server Certificate as described [here](../../portal/settings-server.md#download). You will need it later on.
3. Tell your RADIUS server which certificates will be allowed to connect as described [here](../../portal/settings-trusted-roots/trusted-roots.md#add) .

#### If your vendor does not support [RadSec](../../details.md#what-is-radsec):

1. [Add a proxy ](../../portal/settings-proxy.md#add)to your instance.
2. Download the active Server Certificate as described [here](../../portal/settings-server.md#download). You will need it later on.
3. Tell your RADIUS server which certificates will be allowed to connect as described [here](../../portal/settings-trusted-roots/trusted-roots.md#add) .

### 2. WiFi Settings

#### If your vendor supports [RadSec](../../details.md#what-is-radsec):

{% hint style="info" %}
Use the IP Address which is shown on your [Server Settings](../../portal/settings-server.md) page and port 2083
{% endhint %}

We have a sample vendor Step-by-Step guide for some vendors [here](../access-point-setup/radsec-available/). But in general the following steps apply:

1. Import your active RADIUS server certificate to your WiFi infrastructure.
2. Add the certificate, which your APs are using to your **RadSec allowed Connection** list as described [here](../../portal/settings-server.md#add).
3. Create a new RADIUS profile.
4. Set the IP address and the port of your server in your RADIUS profile.
5. Assign the created profile to your SSID.

#### If your vendor does not support [RadSec](../../details.md#what-is-radsec):

{% hint style="info" %}
Use the IP Address of your **Proxy**, the shared secret on your [Server Settings ](../../portal/settings-server.md)page and the authentication port 1812
{% endhint %}

We have a sample vendor Step-by-Step guide for some vendors [here](../access-point-setup/proxy-needed/). But it's in general the following steps:

1. Create a new RADIUS profile.
2. Set the IP address of your **Proxy**, the shared secret from your [Server Settings](../../portal/settings-server.md) page and the authentication port 1812.
3. Assign the profile to the wanted SSID.

### 3. Azure

No matter which certificate type you are deploying, do not forget to import your Server Certificate to your clients as trusted root as described [here](../../azure/trusted-root.md#to-add-a-trusted-root-profile-for-your-clients).

#### If you are deploying User Certificates:

* Windows Devices:&#x20;
  * Follow the profile generation as described [here](../../azure/wifi-profile/windows.md#user-certificates)
* Apple Devices:
  * Follow the profile generation as described [here](../../azure/wifi-profile/apple-devices.md)
* Android:&#x20;
  * Follow the profile generation as described [here](../../azure/wifi-profile/android.md)

#### If you're deploying Device Certificates:&#x20;

> This step can only work if you have followed the previous RADIUS Server setup step

1. Go to your **Authentication Certificates** section in your Admin Portal and create your WiFi XML as described [here](../../portal/settings-trusted-roots/xml.md#wifi).
2. Create the WiFi profile for your device types:

* Windows Devices:&#x20;
  * Follow the profile generation as described [here](../../azure/wifi-profile/windows.md#device-certificates)
* Apple Devices:
  * Follow the profile generation as described [here](../../azure/wifi-profile/apple-devices.md)
* Android:&#x20;
  * Follow the profile generation as described [here](../../azure/wifi-profile/android.md)

###







