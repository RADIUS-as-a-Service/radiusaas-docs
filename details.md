# Details

## What is RADIUS?

Whenever large companies need network authentication, [RADIUS (RFC 2865)](https://tools.ietf.org/html/rfc2865) is the protocol of choice. RADIUS is an AAA protocol which stands for **authentication, authorization and accounting**. It is therefore best suited for controlling access to networks like Wi-Fi, Wired (802.1X, EAP) or VPN. The protocol was developed by Livingston Enterprises, Inc. in 1991 and is now part of the IETF standards.

## What is RadSec?

RADIUS is an efficient protocol for authentication purposes that uses the UDP transport protocol. Nonetheless, some traffic will not be encrypted during transport. This can be avoided by using [RadSec (RFC 6614)](https://tools.ietf.org/html/rfc6614) which is transported over TCP and completely encapsulated within a TLS  tunnel.&#x20;

## Authentication Certificates

The easiest way to push device or user certificates to your clients is [SCEPman](https://www.scepman.com), as it is super-easy to deploy and integrates seamlessly with Intune.\
\
You can also use your own on-premise PKI.&#x20;

RADIUSaaS supports multiple CAs in parallel.

#### OCSP

To proof revocation of certificates, our RADIUSaaS leverages OSCP.&#x20;

A Certificate Revocation List (CRL) is currently not supported.&#x20;

## Our Service

#### Admin Portal

Each customer has access to their own personal instance through their own RADIUSaaS Admin Portal, which can be used for tasks such as [creating users](portal/users.md#add), changing [allowed certificates](portal/settings/settings-trusted-roots/), [adding proxies](portal/settings/settings-proxy.md), creating [rules](portal/settings/rules/) or performing troubleshooting using RADIUSaaS Insights.&#x20;

#### RADIUS to RadSec Proxy

Our RADIUS server only allows [RadSec](details.md#what-is-radsec) connections. If your WiFi infrastructure does not support RadSec, our RADIUSaaS features a [proxy](portal/settings/settings-proxy.md) functionality, which will establish a secure tunnel allowing you to use our service with traditional UDP.

#### Guests and IOT Devices&#x20;

Some of your devices may not able to receive certificates. Reasons could be that they are not managed by any policy provider or they simply are technically not able to work with certificates. \
In those cases or guests scenarios, you can [add users](portal/users.md#add) to your instance and restrict the access to a specific time frame if needed. This allows you to authenticate printers, TV's or other devices with the same instance.

## Getting Started

Please follow the steps on the following page to get your clients ready authenticating with RADIUS-as-a-Service!

{% content-ref url="how-to-use/get-started/" %}
[get-started](how-to-use/get-started/)
{% endcontent-ref %}





&#x20;
