# RadSec

## Requirements

Before your access points are able to establish a valid RadSec session, there are requirements that must be met, regardless of which manufacturer your access points are from.

* Access Points require a valid **client certificate**
* Access Points must **trust the CA** that has issued your **RADIUS server certificate**.
* RADIUSaaS needs to **trust the CA** that has issued the client certificate on your Access Points.

