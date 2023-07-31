---
description: >-
  This page gives you guidance on how to interpret one of the below error
  messages.
---

# Log

### No EAP session matching state

<figure><img src="../../.gitbook/assets/image (9) (3).png" alt=""><figcaption></figcaption></figure>

Corresponding error messages:

* "No EAP session matching state"
* "Unable to set parent list"

The RADIUS protocol is stateless and since it is normally transmitted over UDP, it has to validate if all packages have arrived, if their integrity persists, if they can be correctly assembled in the correct order, etc., essentially mitigating the flaws of the UDP protocol.

Each authentication request goes through a process of steps that must be performed (e.g. association, request, challenge, challenge-response). For these steps, both sides (client and server) have a state that is included in the packet sent and a state that is stored locally. These states must match the step that client and server are currently in, respectively.

The error message indicates that one of the actors (client, server) has sent or received a packet that does not match the expected state. These errors happen from time to time as UDP cannot guarantee that packets are always delivered (correctly or at all). In most cases this is not a problem  because the RADIUS protocol handles these errors and re-initiates the connection without the computer that is trying to log in noticing.

If the devices are able to connect eventually without noticeable delays, such errors can be ignored. If this is not the case, try increasing your EAP timeouts.



### Proxy Session Resumption

<figure><img src="../../.gitbook/assets/image (7) (1) (2).png" alt=""><figcaption></figcaption></figure>

Your proxy will create a TCP session with your RadSec instance. This session has to be reinitiated every 30 seconds. As long as there is no error within those messages, these log entries are expected.

### Error in error

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

On the one hand, your RadSec Server is globally available, so anyone on the Internet can try to connect to it. On the other hand, there are internal metrics that check if your RadSec Server is available. These checks are currently based on a TCP connection. This connection is not in RADIUS format and therefore results in an error log entry that you can see.

This message basically means that a connection was made to your RadSec port, but it did not communicate with it correctly. In the future the check will be a valid RADIUS message, but for now you don't have to worry about it.
