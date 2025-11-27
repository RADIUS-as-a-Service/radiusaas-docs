# RadSec

<figure><img src="../../../.gitbook/assets/image (23) (1).png" alt=""><figcaption><p>Showing TLS outer tunnel and required certificates</p></figcaption></figure>

Before your access points are able to establish a valid **RadSec connection** (which can be considered an mTLS connection), the following requirements must be met regardless of the manufacturer of the access point.

* Access Points require a valid **client certificate** (typically referred to as "RadSec Client Certificate" or "RadSec Certificate"). This client certificate must have the **EKU Client Authentication** (1.3.6.1.5.5.7.3.2) and **not Server Authentication**.
* Access Points must **trust the CA** that issued the **RADIUS Server Certificate**.
* RADIUSaaS must **trust the CA** that issued the **RadSec client certificate** on your access points.

{% hint style="info" %}
The RadSec protocol requires a shared secret to compute the MD5 integrity checks.\
The [RadSec RFC](https://datatracker.ietf.org/doc/html/rfc6614) defines this shared secret as the literal string "radsec".
{% endhint %}
