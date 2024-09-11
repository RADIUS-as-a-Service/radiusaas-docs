---
description: RADIUSaaS | Hassle Free WiFi Auth
---

# Welcome

{% hint style="warning" %}
**Important Service Information**

as of 10 Sep 2024

**Issue**

Multiple customers with sites in the US are currently reporting RADIUS authentication issues.

**Root Cause Analysis**

* This is not an issue caused by RADIUSaaS.
* Based on customer feedback, AT\&T (ATT) seems to be suffering from issues related to the transport / processing of UDP packets.

**Affected Scope**

* **US-based** sites using **AT\&T or Cogent Communications as ISPs**, that use
* **RADIUS / UDP** for AAA (RadSec / TCP connections are not affected).

**Remediation**

* Switch to an alternative ISP / internet route.
* Inform AT\&T about the issue.
{% endhint %}

RADIUSaaS offers easy and secure authentication for accessing network resources. It delivers the comfort, reliability, and scalability of a native cloud SaaS. Supported protocols are RADIUS as well as RadSec. Authentication is based on certificates. RADIUSaaS is generally capable of validating every certificate that can be used for client authentication. However, to be able to lock someone with a revoked certificate out of your network, choose a CA which provides a publicly accessible OCSP or CRL endpoint.

![](../.gitbook/assets/radius-aas-flow.png)

These docs cover technical aspects of RADIUSaaS. All other information can be found on the [RADIUSaaS website](https://radius-as-a-service.com/).
