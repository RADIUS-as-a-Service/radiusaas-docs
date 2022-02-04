# FAQ

## Is the UDP-based RADIUS protocol secure?

We are recommending to use the modern RadSec protocol to authentication against RADIUSaaS. However, there are many network infrastructure components still out there, which do not support RadSec.

The following diagram shows the RADIUS authentication flow:

![](../.gitbook/assets/radius-authentication-sequence.png)

In the first authentication sequence, the communication is secured by an MD5 based hashing algorithm. No secrets are transported in this phase.

In the second sequence, a TLS-based EAP (e.g. EAP-TLS) encrypts the traffic. The EAP-TLS traffic it tunneled in the UDP traffic. If you use certificate based authentication, no secrets are transported in this phase.

{% hint style="success" %}
Conclusion: UDP-based RADIUS authentication with RADIUSaaS is secure, since&#x20;

* the relevant traffic is encrypted\
  and
* additionally there are no secrets transported.
{% endhint %}
