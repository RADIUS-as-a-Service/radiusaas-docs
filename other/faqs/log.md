---
description: Questions regarding common log entries
---

# Log

### EAP Session is not matching state

<figure><img src="../../.gitbook/assets/image (9) (3).png" alt=""><figcaption></figcaption></figure>

The RADIUS protocol is stateless and since it is normally transmitted over UDP, it has to take care of which packet has arrived, if it is corrupted, etc. So each request goes through a process of steps that must be performed (e.g. association, request, challenge, challenge-response). For these steps, both sides (client and server) have a state that is included in the packet sent and a state that is stored locally. These states must match the step they are currently in.

The error message means that one of the actors (client, server) has sent or received a packet that does not match the expected state. These errors happen from time to time because it is UDP and some packets get lost or other things happen. In most cases this is not a problem either, because the RADIUS protocol handles these errors and re-initiates the connection without the computer that is trying to log in noticing.

If their devices can connect, such errors can be ignored. If this is not the case, try increasing your EAP timeouts.



### Proxy session resumption

<figure><img src="../../.gitbook/assets/image (7) (1) (2).png" alt=""><figcaption></figcaption></figure>

Your proxy will create a TCP session with your RadSec instance. This session has to be reinitiated every 30 seconds. As long as there is no error within those messages, these log entries will be expected.