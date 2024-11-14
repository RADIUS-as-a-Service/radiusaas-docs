# FortiNet

{% hint style="info" %}
To use the RadSec feature on your FortiGate for WiFi (with FortiAPs), firmware FortiOS **7.6.0** or later is required on the FortiGate.
{% endhint %}

## Prepare certificates

To establish a valid RadSec connection, your Access Points must trust the **RADIUS Server Certificate** and your RADIUS server must trust your **RadSec Client Certificate**. To achieve this,

1. Download the root certificate of the CA that has issued your active **RADIUS Server Certificate** as described [here](../../../admin-portal/settings/settings-server.md#download).
2. Create a **RadSec Client Certificate** for your WAPs (centrally managed via FortiGate). If you are using **SCEPman Certificate Master**, the process is described [here](https://docs.scepman.com/certificate-deployment/certificate-master/client-certificate-pkcs-12). FortiGate accepts the **PKCS#12** format for RadSec client certificates.

{% hint style="warning" %}
Ensure to monitor the expiry of your **RadSec Client Certificate** and renew it in due time to prevent service interruptions.
{% endhint %}

3. Add the root certificate of the CA that has issued the **RadSec Client Certificate** to your RADIUS instance as described [here](../../../admin-portal/settings/trusted-roots.md#add) and select **RadSec** under **Use for**.\
   In case the **RadSec Client Certificate** has been issued by SCEPman and you already trust the SCEPman Root CA for client authentication, simply edit the trusted SCEPman Root CA certificate and select **Both** under **Use for**.&#x20;

## FortiGate configuration

{% hint style="info" %}
Below settings are the necessary settings to establish a functional RadSec connection with our service. Configure any other settings at your discretion.
{% endhint %}

### Via UI

To configure RadSec on your FortiGate UI please follow the steps:

* Create a new **RADIUS Server** and add your [RadSec server IP address](../../../admin-portal/settings/settings-server.md#radsec-tcp) under **IP/Name**. For the **Secret**, use "radsec".

<figure><img src="../../../../.gitbook/assets/2023-08-28 10_56_04-Medienwiedergabe.png" alt=""><figcaption></figcaption></figure>

* Import the [root CA of your RADIUS Server Certificate](../../../admin-portal/settings/settings-server.md#download) to the FortiGate **Certificates** under **System > Certificates > Import > CA Certificate**. The imported root CA will be listed under **Remote CA Certificate**.

<figure><img src="../../../../.gitbook/assets/2023-08-28 10_57_07-Medienwiedergabe.png" alt=""><figcaption></figcaption></figure>

* Import the **RadSec Client Certificate** to your FortiGate under\
  **System > Certificates > Import > Certificate**.

<figure><img src="../../../../.gitbook/assets/2023-08-28 10_58_54-Medienwiedergabe.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/2023-08-29 09_36_11-FortiGate.png" alt=""><figcaption></figcaption></figure>

* Modify the RADIUS server configuration in your FortiGate to use it as RadSec client certificate.
* If enabled, please disable the **server-identity-check** in your FortiGate RADIUS server configuration.

### Via command line

To configure RadSec on your FortiGate using the command line, please use these below sets of instructions:

#### RADIUS server configuration

```python
config user radius

    edit "radiusaas"

        set server "radsec-CLIENTNAME.radius-as-a-service.com"

        set secret radsec

        set acct-interim-interval 600

        set radius-port 2083

        set transport-protocol tls

        set ca-cert "CA_Cert"

        set client-cert "certificate-fortigate"

        set server-identity-check disable

    next

end

# Name the root CA of your RADIUS Server Certificate
set ca-cert "CA_Cert"

# Name the RadSec client certificate
set client-cert "certificate-fortigate"
```

#### WiFi SSID configuration

```python
config wireless-controller vap

    edit "client-wireless"

        set ssid "client-wireless"

        set security wpa2-only-enterprise

        set pmf enable

        set 80211k disable

        set 80211v disable

        set auth radius

        set radius-server "radiusaas"

        set local-bridging enable

        set schedule "always"

    next

end
```

### References

Link to FortiGate's documentation for the RadSec configuration:\
[https://docs.fortinet.com/document/fortigate/7.6.0/administration-guide/729374/configuring-a-radsec-client](https://docs.fortinet.com/document/fortigate/7.6.0/administration-guide/729374/configuring-a-radsec-client)
