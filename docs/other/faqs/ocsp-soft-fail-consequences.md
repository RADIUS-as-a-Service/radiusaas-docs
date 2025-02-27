---
description: >-
  This page provides an overview on the pros and cons in the terms of OCSP
  Soft-fail mechanism.
---

# OCSP Soft-fail Consequences

Before we dive into the pros and cons let's start by quickly recapping what the setting means:

Please note: All of those examples describe the behaviour of an authentication where the supplicant has a valid certificate (not expired, issued by a trusted CA).

#### Soft-fail = Enabled

If a problem occurs when querying the OCSP responder such as a timeout or incorrect data, the application treats the certificate revocation status as '**good**'.

#### Soft-fail = Disabled

If a problem occurs when querying the OCSP responder such as a timeout or incorrect data, the application treats the certificate revocation status as '**revoked**'.&#x20;

If you use **OCSP-Autodetect** and the client certificate does not include an OCSP responder URL, the application treats the certificate revocation status as '**revoked**'.&#x20;

## Pros and Cons

### Soft-fail Enabled

#### Pros

1. **Higher Availability & Fewer Disruptions**
   * Users are less likely to experience sudden authentication failures due to transient network issues or temporary OCSP responder outages.
   * Improves user experience and reduces support calls related to “can’t connect” issues caused solely by OCSP connectivity or service problems.
2. **Better Tolerance of OCSP Service Outages**
   * If the OCSP hosting provider experiences downtime or certificate-related issues, authentications will still succeed.
3. **Minimal Impact on Business Continuity**
   * Especially important in environments where downtime has a high cost (e.g., critical infrastructure, emergency services, or 24/7 businesses).

#### Cons

1. **Security Risk: Revoked Certificates May Be Accepted**
   * A failure in the RADIUS server's revocation status verification, due to OCSP unavailability or other issues, will result in the acceptance of a revoked certificate.
   * This undermines the trust model of certificate-based authentication and can be exploited if an attacker deliberately forces OCSP failures.
2. **Lack of Visibility Into System-wide Issues**
   * If the system silently bypasses revocation checks, administrators might not immediately notice that an OCSP responder is down or misconfigured, prolonging the vulnerability window.

### Soft-fail Disabled

#### Pros

1. **Higher Security Assurance**
   * Ensures that any certificate that cannot be positively validated against the OCSP responder is rejected.
   * Protects against usage of revoked, or otherwise compromised certificates.
2. **Clear Operational Signals**
   * If authentication suddenly starts failing, it forces quick attention to OCSP availability or configuration problems.
   * Administrators become immediately aware of any connectivity or trust chain issues because no user can authenticate unless the OCSP check is successful.

#### Cons

1. **Single Point of Failure**
   * If the OCSP responder is down, DDOS attacked, unreachable, or misconfigured, **all** valid certificate authentications fail.
   * Can cause significant business disruption and a flood of support calls.
2. **Reliance on OCSP Service Uptime**
   * Implies your OCSP service must be highly available and robustly monitored.
   * Requires thorough failover planning (e.g., multiple OCSP responders, load balancing, or redundancy) to prevent massive outages.
   * Requires planning for updating the responder
3. **Possible Frustration for Users**
   * Users can’t connect even when they have perfectly valid certificates, simply because the OCSP check can’t complete.
