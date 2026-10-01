# EAP-TLS Authentication Failures over CGNAT and Cellular Links

In network security, the Maximum Transmission Unit (MTU) is a critical networking parameter that defines the largest packet size that can be transmitted over a network link without being fragmented. While most users don't need to think about MTU, it becomes an important troubleshooting factor in specific scenarios, particularly with RADIUS and EAP-TLS authentication. A typical symptom is that authentication works over the primary internet connection but fails over a cellular or satellite backup link. This article explains how MTU affects RADIUS, especially when large payloads such as EAP-TLS certificates are involved, why this can lead to authentication failures, and how to avoid them with RADIUSaaS.

***

#### Introduction to MTU and Fragmentation

The MTU is the largest packet size that a network can handle without breaking it into smaller pieces. The standard MTU for most Ethernet networks is 1500 bytes. When a packet is too large for a network link, it must be either dropped or broken into smaller pieces, a process called IP fragmentation. The destination host is responsible for reassembling all the fragments back into the original packet.

Links such as 4G/5G and satellite connections often have a smaller effective MTU than a typical fixed-line connection, because the carrier adds its own tunnelling overhead. Packets that pass through unchanged on a fixed line may therefore be fragmented on these links.

***

#### EAP vs. IP Fragmentation: Why This Distinction Matters

The distinction between EAP and IP fragmentation is crucial for understanding authentication issues.

IP fragmentation happens at the network layer. A router or host breaks a single UDP datagram into several smaller IP packets. This happens below the RADIUS application, which is unaware that its packet was fragmented.

EAP fragmentation happens at the application layer. The EAP protocol itself does not support fragmentation, but individual EAP methods such as EAP-TLS implement their own. When a large EAP-TLS message, such as a certificate, is too large, it is split into multiple EAP fragments, and each fragment is carried in its own RADIUS packet.

The key difference is that IP fragmentation relies on the network delivering every fragment so the packet can be reassembled, which often fails. EAP fragmentation is handled by the two EAP endpoints, the client and the RADIUS server, and avoids those network-level issues.

***

#### Framed-MTU vs. EAP Fragmentation Size

The Framed-MTU attribute is often confused with EAP fragmentation, partly because it is used in two different ways.

In an Access-Accept, Framed-MTU tells the RADIUS client which MTU to configure for the user's session after authentication. It has no effect on the authentication handshake itself.

In an Access-Request, it does matter for EAP. RFC 3579, Section 2.4, explains that a RADIUS server cannot use MTU discovery to learn the link MTU, so the authenticator may include Framed-MTU in an Access-Request containing EAP to give the server this information. A server that receives it "MUST NOT send any subsequent packet in this EAP conversation" whose concatenated EAP-Message attributes exceed the advertised size, taking the link type into account. For IEEE 802.11, for example, the server may send an EAP packet up to Framed-MTU minus four octets, allowing for the 802.1X header fields.

Framed-MTU in an Access-Request is therefore the standard way for an authenticator to limit the size of EAP packets the server sends. It describes the link between the authenticator and the client, though, not the network path between the authenticator and the RADIUS server. It can reduce the size of RADIUS packets on that path as a side effect, but it isn't a guarantee against IP fragmentation on links with a reduced MTU.

Reference: [RFC 3579, Section 2.4 – Fragmentation](https://www.rfc-editor.org/rfc/rfc3579#section-2.4)

***

#### Why EAP Fragmentation Alone Is Not Enough

In principle, EAP fragmentation keeps every RADIUS packet small enough to cross the network without being IP-fragmented. That is why reducing the EAP fragment size is a common recommendation for self-hosted RADIUS servers.

In practice, the fragment size in each direction is set by the two EAP endpoints: the supplicant on the client side and the RADIUS server on the other. The authenticator acts as a pass-through and does not re-fragment EAP-TLS messages. EAP settings on switches and access points, such as those offered by Cisco, Aruba or Juniper, mainly govern the link between the authenticator and the client. They don't directly control the size of the packets the RADIUS server sends back. Some platforms, such as Meraki, don't expose this setting at all.

The largest message in an EAP-TLS exchange usually travels from the server to the authenticator: the Access-Challenge carrying the server certificate chain. That makes the server-side fragment size the deciding factor. With RADIUSaaS this value is managed by the service and can't be adjusted per customer, so tuning EAP fragmentation on your devices alone won't reliably prevent IP fragmentation on links with a reduced MTU.

Lowering the MTU of the entire WAN interface isn't a good answer either. It affects all traffic on that link, and on its own it doesn't stop UDP RADIUS packets from being fragmented. The more robust approach is to change how the RADIUS traffic is transported, as described below.

***

#### Why Fragmentation Fails with CGNAT

IP fragmentation is a common cause of authentication failures, especially on networks using Carrier-Grade NAT (CGNAT), such as most 4G/5G connections and Starlink.

Many firewalls and CGNAT devices are not designed to handle fragmented packets. Only the first fragment carries the UDP port numbers that NAT uses to track a connection, so the remaining fragments are often dropped, either deliberately for security reasons or because the device can't match them to a session.

If even one fragment is lost, the original UDP datagram cannot be reassembled. UDP has no retransmission mechanism for the missing piece, so the whole packet is lost and the authentication eventually times out.

This affects both directions, but in EAP-TLS it most often hits the server's response. The Access-Challenge carrying the server certificate chain is usually the largest packet in the exchange. If its fragments are dropped, the authenticator never receives the challenge and the authentication fails.

***

#### Solution: Use RadSec, or Keep UDP RADIUS off CGNAT Paths

With RADIUSaaS, the EAP fragment size on the server side is managed by us and can't be changed per customer. The most reliable fix is therefore to keep the large authentication packets from being IP-fragmented on paths that can't handle fragments.

The preferred option is to connect your authenticator to RADIUSaaS using RadSec instead of classic RADIUS over UDP. RadSec carries RADIUS over TLS on TCP port 2083, so large packets such as the server certificate chain are split into TCP segments sized for the path rather than into IP fragments. Firewalls and CGNAT devices handle these like any other TCP traffic, and the problem disappears. Many current platforms support RadSec natively. If your device connects over a link with a reduced MTU, such as a cellular or satellite backup, also make sure the TCP MSS matches the real path MTU. You can do this by setting a lower MTU on that WAN interface, so large TLS segments are not silently dropped.

If your authenticator does not support RadSec, avoid sending UDP RADIUS directly across CGNAT links such as 4G/5G or Starlink. One option is to route the RADIUS traffic through an existing site-to-site VPN to a location with a fixed internet connection, and send it on to the RADIUSaaS proxy from there. Inside the VPN tunnel, fragments are encapsulated and are no longer dropped by the carrier's NAT. Another option is to run a small RadSec proxy on site. It accepts UDP RADIUS from your devices on the local network and forwards it to RADIUSaaS over RadSec.

***

#### Conclusion

Large EAP-TLS messages, particularly the RADIUS server's certificate chain sent during the handshake, can exceed the MTU of some network paths. When that happens over UDP, the packets are IP-fragmented. Firewalls and CGNAT devices often drop those fragments, which causes authentication timeouts that typically appear only on certain links, such as a cellular backup connection. Because the RADIUSaaS server-side fragment size can't be tuned per customer, the most robust fix is to use RadSec wherever possible, which removes IP fragmentation entirely. Where RadSec isn't available, keep UDP RADIUS off CGNAT paths by tunnelling it or by using a local RadSec proxy.
