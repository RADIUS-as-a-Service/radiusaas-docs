# EAP-TLS Authentication Failures over CGNAT and Cellular Links

In network security, the **Maximum Transmission Unit (MTU)** is a critical networking parameter that defines the largest packet size that can be transmitted over a network link without being fragmented. While most users don't need to think about MTU, it becomes an important troubleshooting factor in specific scenarios, particularly with RADIUS and EAP-TLS authentication. This article explores how MTU affects the RADIUS protocol, especially when dealing with large payloads like the certificates used in EAP-TLS, and why this can lead to authentication failures.

***

## Introduction to MTU and Fragmentation

The **MTU** is the largest packet size that a network can handle without breaking it into smaller pieces. The standard MTU for most Ethernet networks is 1500 bytes. When a packet is too large for a network link, it must be either dropped or broken into smaller, acceptable pieces, a process called **IP fragmentation**. The destination host is responsible for reassembling all the fragments back into the original packet.

***

## EAP vs. IP Fragmentation: Why This Distinction Matters

The distinction between EAP and IP fragmentation is crucial for understanding authentication issues.

* **IP Fragmentation (Network Layer)**: This is where a router breaks a single UDP datagram into multiple, smaller IP packets. This happens below the RADIUS application, which is unaware that its packet was fragmented.
* **EAP Fragmentation (Application Layer)**: The EAP protocol itself does not support fragmentation, but it provides a framework where individual EAP methods, such as EAP-TLS, can implement their own fragmentation mechanisms. When a large EAP-TLS message (e.g., a certificate) is too large for the MTU, it can be broken into multiple EAP fragments. Each fragment is then encapsulated within its own RADIUS packet and sent individually.

The key difference is that **IP fragmentation** relies on the network to reassemble packets, a process that often fails. **EAP fragmentation** relies on the client and server to handle reassembly, which is a more robust process that avoids network-level issues.

***

## The Authenticator's Role in Fragmentation

The RADIUS client, or Authenticator, is a key player in this process. While it acts as a proxy, it can be configured to fragment EAP messages to avoid IP fragmentation. This is the preferred method for handling large payloads. For example, an Authenticator can be configured to break down a large EAP message received from a supplicant before encapsulating it in a RADIUS packet and sending it to the server. This ensures the packet is properly sized for the network path.

***

## Framed-MTU vs. EAP Fragmentation Size

The Framed-MTU attribute is often confused with EAP fragmentation, partly because it is used in two different ways.

In an Access-Accept, Framed-MTU tells the RADIUS client which MTU to configure for the user's session after authentication. It has no effect on the authentication handshake itself.

In an Access-Request, it does matter for EAP. RFC 3579, Section 2.4, explains that a RADIUS server cannot use MTU discovery to learn the link MTU, so the authenticator may include Framed-MTU in an Access-Request containing EAP to give the server this information. A server that receives it "MUST NOT send any subsequent packet in this EAP conversation" whose concatenated EAP-Message attributes exceed the advertised size, taking the link type into account. For IEEE 802.11, for example, the server may send an EAP packet up to Framed-MTU minus four octets, allowing for the 802.1X header fields.&#x20;

Framed-MTU in an Access-Request is therefore the standard way for an authenticator to limit the size of EAP packets the server sends. It describes the link between the authenticator and the client, though, not the network path between the authenticator and the RADIUS server. It can reduce the size of RADIUS packets on that path as a side effect, but it isn't a guarantee against IP fragmentation on links with a reduced MTU.

***

## Why EAP Fragmentation is the Better Solution

EAP fragmentation must be configured on both the Authenticator and the RADIUS server to ensure a reliable and successful EAP-TLS authentication. Since the EAP-TLS process is a two-way conversation, both sides must be capable of fragmenting large payloads. It is also a best practice to configure both sides with the same EAP fragment size to ensure consistency and prevent authentication failures.

Given this necessity, when a large certificate causes authentication to fail, it's often due to IP fragmentation. Firewalls or CGNAT devices may drop fragmented packets, causing the authentication to time out. Instead of a blanket reduction of the MTU for all network traffic, the best practice is to configure **EAP fragmentation on the authenticator and RADIUS server.**

This approach offers several key advantages:

* **Targeted Fix**: Configuring a specific EAP fragment size on the authenticator or RADIUS server directly solves the problem at its source. It ensures that the large authentication packets are broken into smaller, acceptable pieces before being sent over the network, preventing IP fragmentation without affecting other traffic.
* **No Significant Performance Impact on the Overall Network**: Reducing the global MTU for an interface affects all network traffic, which can lead to increased overhead and a decrease in network efficiency. By using EAP fragmentation, the rest of the network's traffic continues to use the standard MTU, preserving optimal performance. While EAP fragmentation does create more smaller packets for the same payload and may introduce slight latency due to more roundtrips, this has a negligible performance impact on the overall network and is a necessary trade-off for a successful authentication.
* **Protocol-Specific Design**: EAP methods that support fragmentation are designed to handle large payloads. Relying on this built-in feature is a more reliable and standards-based approach than relying on a network-layer workaround.

Common appliances with RADIUS client capabilities (like switches and access points from Cisco, Aruba, and Juniper) have a feature to configure EAP fragmentation. Similarly, RADIUS servers like FreeRADIUS have a `fragment_size` setting to control the maximum EAP fragment size.&#x20;

It is important to note that not all vendors provide this functionality. For example, Meraki does not offer a user-configurable EAP fragmentation setting in its dashboard, which can be a significant limitation.

***

## Why Fragmentation Fails with CGNAT

IP fragmentation is a common cause of authentication failures, especially on networks using Carrier-Grade NAT (CGNAT), such as Starlink.

* **Fragment Dropping**: Many firewalls and CGNAT devices are not designed to handle fragmented packets efficiently. For security or performance reasons, they may drop the fragments or fail to reassemble them correctly.
* **Packet Loss**: If even one fragment is lost during transit, the entire original UDP datagram cannot be reassembled by the server. Since UDP is connectionless and has no retransmission mechanism, the RADIUS server never receives a complete `Access-Request`, and the authentication fails.

The lack of a retransmission mechanism for fragmented UDP traffic is the core reason IP fragmentation is an unreliable solution for authentication. This is why configuring EAP fragmentation is the correct solution. It ensures that the EAP messages are already in small pieces, preventing them from being fragmented at the IP layer. This bypasses the fragmentation issues caused by firewalls or CGNAT devices that drop fragmented traffic.

***

## Solution: Use RadSec, or Keep UDP RADIUS off CGNAT Paths

With RADIUSaaS, the EAP fragment size on the server side is managed by us and can't be changed per customer. The most reliable fix is therefore to keep the large authentication packets from being IP-fragmented on paths that can't handle fragments.

The preferred option is to connect your authenticator to RADIUSaaS using RadSec instead of classic RADIUS over UDP. RadSec carries RADIUS over TLS on TCP port 2083, so large packets such as the server certificate chain are split into TCP segments sized for the path rather than into IP fragments. Firewalls and CGNAT devices handle these like any other TCP traffic, and the problem disappears. Many current platforms support RadSec natively. If your device connects over a link with a reduced MTU, such as a cellular or satellite backup, also make sure the TCP MSS matches the real path MTU. You can do this by setting a lower MTU on that WAN interface, so large TLS segments are not silently dropped.

If your authenticator does not support RadSec, avoid sending UDP RADIUS directly across CGNAT links such as 4G/5G or Starlink. One option is to route the RADIUS traffic through an existing site-to-site VPN to a location with a fixed internet connection and send it on to the RADIUSaaS proxy from there. Inside the VPN tunnel, fragments are encapsulated and are no longer dropped by the carrier's NAT. Another option is to run a small RadSec proxy on site. It accepts UDP RADIUS from your devices on the local network and forwards it to RADIUSaaS over RadSec.

## Conclusion

Large EAP-TLS messages, particularly the RADIUS server's certificate chain sent during the handshake, can exceed the MTU of some network paths. When that happens over UDP, the packets are IP-fragmented. Firewalls and CGNAT devices often drop those fragments, which causes authentication timeouts that typically appear only on certain links, such as a cellular backup connection. Because the RADIUSaaS server-side fragment size can't be tuned per customer, the most robust fix is to use RadSec wherever possible, which removes IP fragmentation entirely. Where RadSec isn't available, keep UDP RADIUS off CGNAT paths by tunnelling it or by using a local RadSec proxy.

***

[https://www.rfc-editor.org/rfc/rfc3579#section-2.4](https://www.rfc-editor.org/rfc/rfc3579#section-2.4)

