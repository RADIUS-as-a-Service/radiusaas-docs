# FortiGate

{% hint style="info" %}
To use the RadSec feature on your FortiGate APs, firmware FortiOS **7.4.0** or later is required.
{% endhint %}

## Prepare Certificates

1. **Download** the root certificate of the CA that has issued your RADIUS server certificate as described [here](../../../portal/settings/settings-server/certificates.md#download)
2. **Create** a client certificate for your Access Points. If you are using **SCEPman Certificate Master**, the process is described [here](https://docs.scepman.com/certificate-deployment/certificate-master/client-certificate-pkcs-12)
3. **Add** the root certificate of the CA that has issued the client certificate on your Access Point to your RADIUS instance under Server Settings -> RadSec connection certificates as described [here](../../../portal/settings/settings-server/certificates.md#radsec-connection-certificates)

## FortiGate configuration

To configure RadSec on your FortiGate AP please follow the steps below:

* Create new RADIUSServer and add your RadSec server ip address and secret, the fixed shared secret is "radsec"

<figure><img src="../../../.gitbook/assets/2023-08-28 10_56_04-Medienwiedergabe.png" alt=""><figcaption></figcaption></figure>

* Import the root certificate of your RaaS to FortiGate Certificates\
  System-> Certificates -> Import -> CA Certificate

The imported RootCA will be listed under "Remote CA Certificate"

<figure><img src="../../../.gitbook/assets/2023-08-28 10_57_07-Medienwiedergabe.png" alt=""><figcaption></figcaption></figure>

* Import the client certificate to your FortiGate (FortiOS 7.4.0 supports PKCS#12 certificates)\
  System -> Certificates -> Import -> Certificate

<figure><img src="../../../.gitbook/assets/2023-08-28 10_58_54-Medienwiedergabe.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/2023-08-28 10_59_21-Medienwiedergabe.png" alt=""><figcaption></figcaption></figure>

* Change the RADIUS config in your FortiGate to use it as client certificate
* If enabled, disable the server-identity-check on your FortiGate RADIUS configuration

## Links

Link to FortiGate's documentation for the RadSec configuration:\
[https://docs.fortinet.com/document/fortigate/7.4.0/administration-guide/729374/configuring-a-radsec-client-new](https://docs.fortinet.com/document/fortigate/7.4.0/administration-guide/729374/configuring-a-radsec-client-new)
