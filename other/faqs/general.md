# General

## Authentication Frequency

#### How often is a device typically authenticating against RADIUSaaS?

This is difficult to answer as it depends on the behavior of your users, clients and networking gear (APs, NACs, switches). Additionally, it is **important** to note that RADIUSaaS will neither

* trigger an authentication, nor
* send a termination request via its accounting port to the client, potentially triggering a re-authentication.

If you feel your devices are authenticating very frequently (multiple times an hour) without the user constantly restarting the client, then this could be for the following reasons:

* The network controller doesn't authenticate the client fast enough for the client to join the network, so the client attempts to authenticate again. Check the network controller to see if/when it is receiving an answer from RaaS.&#x20;
* The network controller is re-initiating authentication. Check the network controller for what might be causing this.&#x20;
