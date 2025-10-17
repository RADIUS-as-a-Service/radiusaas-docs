# Transport Security in RADIUS vs. RadSec

When deploying secure authentication using Extensible Authentication Protocol (EAP), the choice of transport protocol - RADIUS over UDP vs. RADIUS over TLS (RadSec) - plays a critical role in determining how securely metadata and protocol elements are transmitted across the network.

Both RADIUS and RadSec can carry EAP messages including those used in common methods like EAP-TLS, PEAP, and EAP-TTLS. While the inner workings of the EAP method remain the same, the transport protocol impacts visibility, integrity, confidentiality of metadata, and operational behavior.

This document compares how RADIUS and RadSec affect EAP transport - focusing on encryption, shared secret usage, attribute exposure, logging implications, and deployment considerations.

***

### Transport Security: Core Differences

EAP methods such as EAP-TLS, PEAP, and EAP-TTLS provide authentication mechanisms between clients (supplicants) and authentication servers. However, these methods do not inherently encrypt the transport of metadata between intermediate nodes. This role falls to the transport layer protocol:

* RADIUS (UDP) transmits EAP messages and RADIUS attributes in plaintext.
* RadSec (TLS) encrypts the entire RADIUS packet, securing all attributes and protocol headers during transit.

The table below summarises these differences which affects confidentiality of attributes such as outer identity, NAS IP, calling station ID, and more. While EAP methods may encrypt user credentials (e.g., certificates or passwords), metadata remains exposed over standard RADIUS unless RadSec is used.

| Protocol                 | RADIUS                                   | RadSec                                |
| ------------------------ | ---------------------------------------- | ------------------------------------- |
| Transport Layer Protocol | UDP (typically)                          | TLS over TCP                          |
| Encryption               | No transport encryption                  | Full transport encryption             |
| Default Port\*           | UDP 1812                                 | TCP 2083                              |
| Notes                    | Stateless; may be filtered by firewalls. | Stateful; establishes secure tunnels. |

\*While these ports are the recognised standards, some deployments may customise them for local policy or infrastructure reasons.

***

### The Role of the RADIUS Shared Secret in Both Protocols

The RADIUS shared secret is used for packet integrity and legacy encryption behaviours. This applies regardless of whether the transport is standard RADIUS or RadSec.

**Key uses:**

* **Message-Authenticator AVP**: Ensures packet integrity using HMAC-MD5.
* **User-Password AVP (legacy methods)**: Encrypts credentials using the shared secret.
* **RadSec compatibility**: Even in RadSec, the shared secret is maintained for compatibility with RADIUS semantics and intermediate systems.

{% hint style="info" %}
**Note:** The shared secret is never transmitted over the network - it is used locally for HMAC operations and AVP validation.
{% endhint %}

***

***

### Data Element Visibility: RADIUS vs. RadSec

This table compares key data elements when using EAP-TLS over RADIUS (UDP) and RadSec (TLS), grouped by visibility risk during transmission.

<table data-full-width="false"><thead><tr><th width="151.4000244140625">Data Element</th><th width="175.0001220703125">RADIUS (UDP)</th><th width="175.80010986328125">RadSec (RADIUS over TLS)</th><th>Example / Notes</th></tr></thead><tbody><tr><td><strong>Outer RADIUS packet</strong></td><td>Clear text (over UDP or TCP).</td><td>Encrypted (over TLS).</td><td>Includes both headers and AVPs. E.g., packet visible in Wireshark or tcpdump.</td></tr><tr><td><strong>User identity (outer identity)</strong></td><td>Sent in clear text.</td><td>Encrypted (over TLS).</td><td><code>anonymous@organisation.com</code> used for realm routing.</td></tr><tr><td><strong>RADIUS AVPs (Attributes)</strong></td><td>Some in clear text (e.g., NAS-IP, User-Name).</td><td>Fully encrypted in TLS.</td><td><p><code>NAS-IP-Address,</code></p><p> <code>Calling-Station-Id</code></p></td></tr><tr><td><strong>RADIUS headers</strong></td><td>Clear text</td><td>Encrypted in TLS</td><td><p>Code (<code>Access-Request</code>), <code>Identifier,</code> </p><p><code>Length,</code> </p><p><code>Authenticator</code></p></td></tr><tr><td><strong>RADIUS shared secret</strong></td><td>Not transmitted (used internally)</td><td>Not transmitted (used internally)</td><td>Used for AVP encryption and <code>Message-Authenticator</code> validation.</td></tr><tr><td><strong>EAP payload</strong> </td><td>Encrypted between client and server</td><td>Encrypted in TLS</td><td>Includes TLS handshake, certificate validation, etc.</td></tr><tr><td><strong>User credentials (certificates/keys)</strong></td><td>Encrypted in EAP tunnel</td><td>Encrypted in EAP-tunnel + TLS</td><td>Applies to certificate-based (EAP-TLS) and password-based (PEAP) methods. Client certificate with subject <code>CN=jsmith, OU=IT</code></td></tr><tr><td><strong>Password (e.g., PEAP or MSCHAPv2)</strong></td><td>Encrypted in tunnelled method (e.g., PEAP)</td><td>Encrypted in EAP tunnel + TLS</td><td>Applies to password-based methods.</td></tr></tbody></table>

***

### Identity Privacy: Outer vs. Inner Identity

EAP methods that use tunnelling or certificates (e.g., PEAP, EAP-TTLS, EAP-TLS) support outer and inner identities:

* **Outer Identity**: Sent in the clear in the User-Name AVP to facilitate realm routing. Often anonymised (e.g.,  `anonymous@domain.com`).
* **Inner Identity**: Carried securely inside the encrypted EAP tunnel (e.g., username/password, certificate CN).

#### Behaviour across transports:

* In **RADIUS**, the outer identity is sent in plaintext and is visible on the wire.
* In **RadSec**, the entire packet including the outer identity is encrypted.

Privacy concerns related to the Outer Identity being transmitted in plaintext with RADIUS can be mitigated by configuring identity privacy or leveraging device certificates that do not contain PII data in the common name attribute

{% hint style="info" %}
**Note**: Some platforms, like Windows’ native EAP client, may not support identity privacy by default in methods like EAP-TLS, potentially exposing the real identity in the outer layer.
{% endhint %}

#### Outer Identity

The table below provides an overview on the typical values various OS platforms populate the Outer Identity with.

| Credential Type    | Outer Idenity                                                                                                                                                                                               | Applicable OS                                                                                                                                                                            |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Username/Password  | <p><br><br>Typically:<br>- <code>UPN</code><br>- <code>Email Address</code></p>                                                                                                                             | <ul><li>Windows<br>(can be anonymised)</li><li>iOS, iPadOS, macOS<br>(can be anonymised)</li><li>Android<br>(can be anonymised)</li></ul>                                                |
| Device Certificate | <p>Common name (CN) property of the client certificate's subject name. <br><br>Typically:<br>- <code>DeviceName</code><br>- <code>Intune Device ID</code> (GUID)<br>- <code>AAD Device ID</code> (GUID)</p> | <p></p><ul><li>Windows<br>(<strong>cannot be anonymised</strong>) *</li></ul><ul><li>iOS, iPadOS, macOS<br>(can be anonymised)</li></ul><ul><li>Android<br>(can be anonymised)</li></ul> |
| User Certificate   | <p>Common name (CN) property of the client certificate's subject name. <br><br>Typically:<br>- <code>UPN</code><br>- <code>Email Address</code></p>                                                         | <p></p><ul><li>Windows<br>(<strong>cannot be anonymised</strong>)*</li></ul><ul><li>iOS, iPadOS, macOS<br>(can be anonymised)</li></ul><ul><li>Android<br>(can be anonymised)</li></ul>  |

\*The EAP client used in Windows does not support identity privacy for EAP-TLS.

***

### Logging and Deployment Considerations

#### RadSec (TLS)

RadSec encrypts the entire RADIUS packet, which prevents passive eavesdropping. This shifts logging and diagnostics from network inspection to the application or RADIUS server level, as these are the endpoints that terminate the TLS session. Organizations transitioning to RadSec should evaluate monitoring workflows to accommodate encrypted traffic.

#### RADIUS (UDP)

The plaintext transmission of attributes (User-Name, NAS-IP-Address, etc.) simplifies troubleshooting and live diagnostics, enabling packet capture with minimal tooling. However, this increases the exposure risk of sensitive metadata during transit.

#### Network Support

Support for RadSec is not universal across all network devices and infrastructure. Many network switches, wireless access points, and firewalls support only traditional RADIUS. In mixed environments, a RadSec-to-RADIUS proxy may be needed to bridge modern and legacy components. Infrastructure assessments are recommended before deploying RadSec in production.

***

### Conclusion and Key Takeaways

Transporting EAP methods over either RADIUS or RadSec offers the same strong end-to-end protection for user credentials but differs significantly in how metadata and protocol elements are exposed during transit.

* **EAP End-to-End Protection**: EAP provides strong protection for user credentials over both RADIUS and RadSec.
* **Primary Difference**: RADIUS uses UDP and does not encrypt the RADIUS packet itself, while RadSec wraps the entire packet in TLS, hiding all attributes and headers from the wire.
* **Metadata Confidentiality**: RadSec encrypts all metadata (headers, attributes, and Outer Identity), preventing network-level exposure.
* **Shared Secret**: The Shared Secret is never transmitted. It is only used locally to compute message integrity checks and verify authenticity.
* **Trade-Offs**: RadSec provides enhanced confidentiality but complicates diagnostics due to encryption, and its support is not universal across all network equipment.

***

### Standards and RFC References

For further detail on protocols and specifications:

* EAP (Extensible Authentication Protocol): [RFC 3748](https://datatracker.ietf.org/doc/html/rfc3748)
* EAP-TLS: [RFC 5216](https://app.gitbook.com/u/prTa0MCUWsXJtIRi1RLj5w5HJ062)
* RADIUS (Remote Authentication Dial-In User Service): [RFC 2865](https://app.gitbook.com/u/prTa0MCUWsXJtIRi1RLj5w5HJ062)
* RadSec (RADIUS over TLS): [RFC 6614](https://app.gitbook.com/o/-LhPlvZ6dc8XcqY7tdZw/s/yDA5bmNydJ2FcKrKVuL4/)
