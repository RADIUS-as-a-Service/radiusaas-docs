# Details

## What is RADIUS?

Whenever large companies need network authentication, [RADIUS (RFC 2865)](https://tools.ietf.org/html/rfc2865) is the protocol of choice. RADIUS is an AAA protocol which stands for **authentication, authorization and accounting**. It is therefore best suited for controlling access to networks like WiFi, Wired (802.1X, EAP) or VPN. The protocol was developed by Livingston Enterprises, Inc. in 1991 and is now part of the IETF standards.

## What is RadSec?

RADIUS is an efficient protocol for authentication purposes that uses the UDP transport protocol. Nonetheless, some traffic will not be encrypted during transport. This can be avoided by using [RadSec (RFC 6614)](https://tools.ietf.org/html/rfc6614) which is transported over TCP and completely encapsulated within a TLS  tunnel.&#x20;

## Authentication Certificates

The easiest way to push device or user certificates to your clients is [SCEPman](https://www.scepman.com/), as it is super-easy to deploy and integrates seamlessly with Intune and other MDM systems.\
\
You can also use the [Microsoft Cloud PKI](configuration/get-started/scenario-based-guides/microsoft-cloud-pki.md), your own on-premise PKI or other compatible CAs.

RADIUSaaS supports multiple CAs in parallel.

#### Online Certificate Verification

To check if a certificate is considered valid by your Certificate Authority (CA) **at authentication time**, RADIUSaaS leverages the Online Certificate Status Protocol (OCSP) or Certificate Revocation Lists (CRLs).

## Our Service

#### Admin Portal

Each customer has access to their own personal instance through their own RADIUSaaS Admin Portal, which can be used for tasks such as [creating users](admin-portal/users.md#add), changing [allowed certificates](admin-portal/settings/trusted-roots.md), [adding proxies](admin-portal/settings/settings-proxy.md), creating [rules](admin-portal/settings/rules/) or performing troubleshooting using RADIUSaaS Insights.&#x20;

#### RADIUS to RadSec Proxy

The service's internal RADIUS server only allows [RadSec](details.md#what-is-radsec) connections. If your WiFi infrastructure does not support RadSec, RADIUSaaS features a [proxy](admin-portal/settings/settings-proxy.md) functionality, which will establish a secure tunnel allowing you to use the service with traditional UDP.

#### Guests, BYOD and IOT Devices&#x20;

Some of your devices may not be able to receive certificates. Reasons could be that they are not managed by any policy provider/MDM system, or they are simply technically not able to work with certificates. \
In those cases, BYOD or guests scenarios, you can [add users](admin-portal/users.md#add) to your instance and restrict the access to a specific time frame, if needed. This allows you to authenticate printers, TVs or other devices with a single instance of the service while using the same SSID.

#### Regions

RADIUSaaS can be used globally.

**RADIUSaaS' core service** can be deployed into datacenters in the following regions and countries:

* Australia
* European Union
* United Kingdom
* United States of America

**RADIUS proxies** can be deployed into datacenters on [all continents](admin-portal/settings/settings-proxy.md#regions).&#x20;

## Getting Started

Please follow the steps on the following page to get your clients ready authenticating with RADIUS-as-a-Service!

{% content-ref url="configuration/get-started/" %}
[get-started](configuration/get-started/)
{% endcontent-ref %}
