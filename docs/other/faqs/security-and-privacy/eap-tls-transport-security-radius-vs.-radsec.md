---
description: >-
  This article explains the key differences in transport security between RADIUS
  and RadSec.
---

# EAP-TLS Transport Security: RADIUS vs. RadSec

When deploying secure authentication using EAP-TLS, the underlying transport protocol plays a key role in determining how and where data is protected during transmission.

Both RADIUS (UDP) and RadSec (RADIUS over TLS) can carry EAP-TLS messages, but they differ in how they secure those messages and handle various protocol components. This document compares how EAP-TLS behaves over RADIUS versus RadSec, focusing on encryption, shared secret usage, attribute exposure, and deployment implications. While the EAP-TLS method itself remains the same, the characteristics of the transport protocol affect data visibility, integrity, and operational behaviour.

In summary, while EAP-TLS provides strong end-to-end protection for user credentials in both, RadSec (TLS) is superior for protecting metadata (like IP addresses and the Outer Identity) by encrypting the entire RADIUS packet, which remains in plaintext with standard RADIUS (UDP).

***

### Transport Security: Core Differences

EAP-TLS provides end-to-end encryption of the authentication exchange between the client (supplicant) and the authentication server. However, this does not automatically encrypt the RADIUS messages themselves during transit. That function is handled by the transport layer protocol:

| Protocol   | Transport Layer | Encryption                | Default Port | Notes                                    |
| ---------- | --------------- | ------------------------- | ------------ | ---------------------------------------- |
| **RADIUS** | UDP (typically) | No transport encryption   | UDP 1812     | Stateless; may be filtered by firewalls. |
| **RadSec** | TLS over TCP    | Full transport encryption | TCP 2083     | Stateful; establishes secure tunnels.    |

While these ports are the recognised standards, some deployments may customise them for local policy or infrastructure reasons.

***

### The Role of the RADIUS Shared Secret in Both Protocols

Even when using RadSec (which encrypts the entire RADIUS packet with TLS), the shared secret is still required to maintain compatibility with traditional RADIUS protocol semantics:

* **Message-Authenticator AVP**: Provides packet integrity using HMAC-MD5 with the shared secret.
* **User-Password AVP (legacy)**: Encrypts passwords using the shared secret, as per RADIUS specifications.
* **Hybrid Compatibility**: Essential for environments using RadSec proxies to forward requests to legacy RADIUS servers.

These behaviours ensure compatibility with existing RADIUS implementations, even when the transport is upgraded to use TLS.

Crucially, the shared secret is never transmitted across the network in either RADIUS or RadSec. It is used locally on both the client and server to compute and verify message integrity checks (e.g., the Message-Authenticator attribute) and verify authenticity.

***

### Logging and Auditing Impact

**RADIUS (UDP)**

With RADIUS, attributes and headers are visible in plaintext, making it easier to capture and review packets during troubleshooting, auditing, or incident response. However, this also exposes potentially sensitive metadata.

**RadSec (TLS)**

With RadSec, the entire RADIUS packet (including headers and AVPs) is encrypted using TLS.&#x20;

* This prevents passive network eavesdropping of sensitive metadata during audits or incidents, significantly enhancing confidentiality and integrity at the transport level.
* However, packet captures will not reveal any useful information without access to the TLS session keys.
* Troubleshooting and live traffic inspection may require additional tools or logging at the application level.

Organisations should review how logging and diagnostics are performed when transitioning to RadSec, as this change can affect security monitoring workflows.

***

### Support in Network Equipment

Support for RadSec is not universal across all network devices and infrastructure.&#x20;

* Many network switches, wireless access points, and firewalls support only traditional RADIUS.
* Some vendors may only support RadSec on specific platforms or require additional licensing or configuration.
* In mixed environments, a RadSec-to-RADIUS proxy may be needed to bridge modern and legacy components.

This has deployment and interoperability implications, especially in large or phased rollouts. Infrastructure assessment and testing are recommended before full adoption.

***

### Comparison Table: EAP-TLS over RADIUS vs. RadSec

This table compares key data elements when using EAP-TLS over RADIUS (UDP) and RadSec (TLS), grouped by visibility risk during transmission.

<table><thead><tr><th width="144.20001220703125">Data Element</th><th width="157.39996337890625">RADIUS (UDP)</th><th width="231.00006103515625">RadSec (RADIUS over TLS)</th><th>Example / Notes</th></tr></thead><tbody><tr><td><strong>Outer RADIUS packet</strong></td><td>Clear text (over UDP or TCP)</td><td>Encrypted (over TLS)</td><td>Includes both headers and AVPs. E.g., packet visible in Wireshark or tcpdump.</td></tr><tr><td><strong>User identity (outer identity)</strong></td><td>Sent in clear text</td><td>Encrypted in TLS</td><td><code>anonymous@organisation.com</code> used for realm routing.</td></tr><tr><td><strong>RADIUS AVPs (attributes)</strong></td><td>Some in clear text (e.g., NAS-IP, User-Name)</td><td>Fully encrypted in TLS</td><td><code>NAS-IP-Address=10.0.0.1, Calling-Station-Id=AA:BB:CC:DD:EE:FF</code></td></tr><tr><td><strong>RADIUS headers</strong></td><td>Clear text</td><td>Encrypted in TLS</td><td>Code (<code>Access-Request</code>), Identifier, Length, Authenticator.</td></tr><tr><td><strong>RADIUS shared secret</strong></td><td>Not transmitted (used internally)</td><td>Not transmitted (used internally)</td><td>Used for AVP encryption and <code>Message-Authenticator</code> validation.</td></tr><tr><td><strong>EAP payload (EAP-TLS messages)</strong></td><td>Encrypted between client and server</td><td>Encrypted in TLS and EAP-TLS</td><td>Includes TLS handshake, certificate validation, etc.</td></tr><tr><td><strong>User credentials (certificates/keys)</strong></td><td>Encrypted inside EAP-TLS</td><td>Encrypted inside EAP-TLS + TLS</td><td>Client certificate with subject <code>CN=jsmith, OU=IT</code></td></tr><tr><td><strong>Password (e.g., PEAP or MSCHAPv2)</strong></td><td>Encrypted in tunnelled method (e.g., PEAP)</td><td>Encrypted in tunnel + TLS</td><td>While not applicable to EAP-TLS directly, this applies to other password-based methods.</td></tr></tbody></table>

***

### Use of Outer vs Inner Identity in EAP-TLS

EAP-TLS authentication uses two identities:

* **Outer Identity**: Sent in the RADIUS `User-Name` AVP, usually to facilitate routing between realms. Often an anonymous placeholder.\
  &#xNAN;_&#x45;xample: `anonymous@domain.com`_
* **Inner Identity**: Contained within the client certificate and used for actual authentication inside the EAP-TLS tunnel.\
  &#xNAN;_&#x45;xample: Certificate Subject `CN=jsmith, O=Org, C=AU`_

In **RADIUS**, the outer identity is exposed on the wire. In **RadSec**, it is encrypted along with the rest of the packet.

#### Outer Identity

The table below provides an overview on the typical values various OS platforms populate the Outer Identity with.

| Credential Type    | Outer Idenity                                                                                                                                                                                               | Applicable OS                                                                                                                                                                           |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Username/Password  | <p><br><br>Typically:<br>- <code>UPN</code><br>- <code>Email Address</code></p>                                                                                                                             | <ul><li>Windows<br>(can be anonymised)</li><li>iOS, iPadOS, macOS<br>(can be anonymised)</li><li>Android<br>(can be anonymised)</li></ul>                                               |
| Device Certificate | <p>Common name (CN) property of the client certificate's subject name. <br><br>Typically:<br>- <code>DeviceName</code><br>- <code>Intune Device ID</code> (GUID)<br>- <code>AAD Device ID</code> (GUID)</p> | <p></p><ul><li>Windows<br>(<strong>cannot be anonymised</strong>)*</li></ul><ul><li>iOS, iPadOS, macOS<br>(can be anonymised)</li></ul><ul><li>Android<br>(can be anonymised)</li></ul> |
| User Certificate   | <p>Common name (CN) property of the client certificate's subject name. <br><br>Typically:<br>- <code>UPN</code><br>- <code>Email Address</code></p>                                                         | <p></p><ul><li>Windows<br>(<strong>cannot be anonymised</strong>)*</li></ul><ul><li>iOS, iPadOS, macOS<br>(can be anonymised)</li></ul><ul><li>Android<br>(can be anonymised)</li></ul> |

\*The EAP client used in Windows does not support identity privacy for EAP-TLS.

***

### Standards and RFC References

For readers seeking deeper understanding or implementation detail, the following standards provide foundational context:

* **RADIUS**: RFC 2865
* **RadSec** (RADIUS over TLS): RFC 6614
* **EAP-TLS**: RFC 5216
* **EAP** (Extensible Authentication Protocol): RFC 3748

These documents define packet formats, behaviours, and transport security considerations for implementers and reviewers.

***

### Conclusion and Key Takeaways

Transporting EAP-TLS over either RADIUS or RadSec offers the same strong end-to-end protection for user credentials but differs significantly in how metadata and protocol elements are exposed during transit.&#x20;

* **EAP-TLS provides strong end-to-end protection for user credentials** over both RADIUS and RadSec.
* **The primary difference is transport security**: While RADIUS uses UDP and does not encrypt the RADIUS packet itself, RadSec wraps the entire packet in TLS, hiding all attributes and headers from the wire.
* **RadSec encrypts all metadata** (headers, attributes, and Outer Identity), preventing network-level exposure.
* **The Shared Secret is never transmitted.** It is only used locally to compute message integrity checks and verify authenticity.
* **RadSec provides enhanced confidentiality** but complicates diagnostics due to encryption, and its support is not universal across all network equipment.
* Privacy concerns related to the Outer Identity being transmitted in plaintext with RADIUS can be mitigated by configuring identity privacy or leveraging device certificates that do not contain any PII data in the common name attribute.

