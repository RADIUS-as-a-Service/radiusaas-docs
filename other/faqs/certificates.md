---
description: >-
  This page summarizes common certificate errors that may occur while
  interacting with your instance and how to interpret them.
---

# Certificates

### Certificate Chain could not be verified

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

When you want to use your own Server certificate, your RADIUS server needs the complete chain in order to let other participants (Proxy, RadSec clients, endpoints which tries to connect) verify the identity.  \
If you see this message, either copy & paste the CA certificate below the Server certificate in the textfield or create a PCKS8 certificate bundle which includes all certificates.
