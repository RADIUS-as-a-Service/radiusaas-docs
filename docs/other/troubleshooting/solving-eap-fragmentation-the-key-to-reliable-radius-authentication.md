# Solving EAP Fragmentation: The Key to Reliable RADIUS Authentication

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

The Framed-MTU is often confused with EAP fragmentation. The Framed-MTU is an attribute usually sent in an `Access-Accept` message from the RADIUS server to the RADIUS client. Its purpose is to set the IP MTU for the user's data session after they have been successfully authenticated. It cannot solve fragmentation issues that occur during the authentication handshake itself, which is when EAP fragmentation is needed.

In a less common scenario, the Framed-MTU attribute can also be sent from the RADIUS client to the server in an `Access-Request` message to signal the MTU that the client is capable of supporting or would prefer to use. It's a hint or a suggestion, not a command. The RADIUS server is free to ignore this value or use it as a factor when deciding on the MTU for the session, however, this is not a standard mechanism for negotiating EAP fragment size.

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

## Conclusion

The interaction between large EAP-TLS certificate payloads and a network's MTU can be a hidden cause of authentication failures. While the RADIUS protocol relies on the network to handle fragmentation, firewalls and CGNAT devices often drop fragmented packets. By understanding the distinction between EAP and IP fragmentation, and by implementing the best practice of configuring EAP fragmentation on the authenticator and RADIUS server, you can ensure that authentication packets traverse the network intact. This deliberate, application-layer approach provides a robust and reliable solution, preventing common and frustrating connectivity issues.
