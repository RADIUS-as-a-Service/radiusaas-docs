---
description: >-
  This page provides an overview of the RADIUS and RadSec public IP addresses
  and ports
---

# Ports & IP Addresses

![](<../../../.gitbook/assets/image (79) (1).png>)

## Radsec / TCP

### **Server IP Address**

{% hint style="warning" %}
This IP address only speaks [RadSec](../../../details.md#what-is-radsec)!
{% endhint %}

The public IP Address at which your server is available. ****&#x20;

### **Radsec Port**

The standard RadSec port.

## Radius / UDP

### Server IP Address and Location

{% hint style="warning" %}
This IP address only speaks [RADIUS](../../../details.md#what-is-radius)!
{% endhint %}

Geo-location of the RADIUS proxy/proxies as well as the respective public IP address(es).

### UDP Auth Port

The standard **** RADIUS authentication port

### UDP Acc Port

The standard **** RADIUS accounting port

### Shared Secret

The shard secret that encrypts the RADIUS communication between your network gear and RADIUSaaS.

## Optional Settings

### OCSP Hard-Fail

{% hint style="warning" %}
In case you **enable** this setting, authentication requests will be **rejected** if your CA's OCSP responder is not available/reachable.
{% endhint %}

This setting determines how RADIUSaaS behaves, when the OCSP responder of your trusted CA cannot be reached.&#x20;

By default, we **recommend to disable OCSP hard-fail** to increase the availability of the service by allowing authentication requests to be accepted even if the OCSP responder cannot be reached. With this **soft-fail** mechanism, and in case OCSP is not reachable, RADIUSaaS will only check if the incoming certificate was signed by one of the [trusted CAs](../settings-trusted-roots/trusted-roots.md) (and process the optional [Rules](../rules/)).
