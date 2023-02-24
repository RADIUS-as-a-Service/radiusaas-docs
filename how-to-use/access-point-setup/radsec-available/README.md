# RadSec

## Requirements

In order for your access points to establish a valid RadSec session, there are requirements that must be met, regardless of which manufacturer your access points are from

* Access Points need a valid **Client certificate**
* Access Points need to trust the root certificate of your RADIUS Server
* RADIUS Server need to trust the CA of where your Access Points get their certificate from

