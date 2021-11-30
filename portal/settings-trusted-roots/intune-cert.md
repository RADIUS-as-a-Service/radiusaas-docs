---
description: >-
  It's also possible to use the certificate which every Windows 10 machine
  receives from Intune when joining your Azure Active Directory
---

# Intune Certificate

{% hint style="danger" %}
### **This setting is optional. In case you are not familiar with Intune Certificates, please do not configure them!**
{% endhint %}

## Why we are not recommending to use Intune certificates for authentication purposes?

* Intune certificates are not supported by Microsoft for other purposes than management with Intune.
* Validity time for the Intune certificates is 1 year.
* There is no mechanism to revoke certificates (like OCSP).

Instead of using Intune certificates, we recommend to use certificates from a PKI like [SCEPman](https://scepman.com).

## Configure Intune IDs

{% hint style="danger" %}
Use this setting with care. One of the following IDs **must** exist in the certificate extension 1.2.840.113556.5.14. All other certificates will **rejected**.
{% endhint %}

To get your Intune Tenant ID, follow these steps:&#x20;

* Press **Windows + R** and enter **certlm.msc**
* Go to your personal certificates. There will be a certificate from one of the following issuers
  * SC Online Issuing
  * MDM Device Authority&#x20;
*   Open this certificate, go to **Details** and search for the extension&#x20;

    1.2.840.113556.5.14
* The printed HEX value is your Intune Tenant ID.&#x20;

#### Example:

The Tenant ID of the following certificate is: **bb4397cb6891c64db17f766487518a6a**

![](<../../.gitbook/assets/image (33).png>)
