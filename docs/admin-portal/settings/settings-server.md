---
description: >-
  Server settings are available under
  https://YOURNAME.radius-as-a-service.com/settings/server
---

# Server Settings

## Ports & IP Addresses

### Overview

RADIUSaaS operates a RadSec service to provide secure cloud-based authentication for its users. In addition, for those customers who are unable to utilise RadSec in their network environment, e.g. due to hardware and software limitations, RADIUSaaS provides RADIUS Proxies, that handle the protocol conversion from RADIUS to RadSec.&#x20;

Both RadSec and RADIUS service offer public IP address that enable your network appliances and services to communicate with our service from anywhere via the internet. These services operate on their unique registered ports. &#x20;

## RadSec / TCP

<figure><img src="../../../.gitbook/assets/image (386).png" alt=""><figcaption><p>Showing RadSec IP and port</p></figcaption></figure>

#### **RadSec DNS**

The DNS entry through which the RadSec service can be reached.&#x20;

#### **Server IP Addresses**

This is the public IP address of the RadSec service.

#### **RadSec Ports**

This is the registered port for RadSec: 2083

### Failover & Redundancy

In cases where customers require higher levels of redundancy, multiple RadSec endpoints can be configured for your instance providing an additional IP addresses. Please note that there is an additional cost for this service.

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Showing two public IP addresses, one for each of the RadSec services.</p></figcaption></figure>

{% hint style="info" %}
It is important to note that RADIUSaaS **does NOT provide failover** between RadSec endpoints. Instead, this failover is typically implemented on your network equipment as shown in below example using Meraki.&#x20;

It is recommended to configure your failover scenario using IP addresses rather than DNS for better visibility and less reliance on an additional service (DNS).&#x20;
{% endhint %}

In this configuration, the two RadSec IP addresses are listed in order of preference. When Meraki is unable to reach one of the IP addresses, it will typically try two more times and moves on to the next one. For more information regarding the failover capability of your Meraki (or other) system, please consult your own resources.

<figure><img src="../../.gitbook/assets/2025-01-24_15h14_06.png" alt=""><figcaption><p>Showing multiple RadSec servers in order of priority (Meraki).</p></figcaption></figure>

## RadSec Settings

{% hint style="info" %}
The following settings control certain aspects of the RadSec connection to your RADIUSaaS instance.
{% endhint %}

### Maximum TLS Version

This setting controls the maximum TLS version for your RadSec interface. The minimum version is fixed at 1.2, the default maximum is set to 1.3.

TLS 1.3 offers several advantages over 1.2, including the post-handshake authentication mechanism, which allows requesting additional credentials before completing the handshake. This is important for the verification checks for RadSec certificates setting discussed next.

### Verification checks for RadSec certificates

{% hint style="info" %}
This setting determines whether a revocation check should be performed for all RadSec connections. The method for verifying the revocation check differs slightly from that used for client authentication certificates.
{% endhint %}

For proper RadSec operation, your network devices, such as Access Points, Switches, and VPN Servers, must initially perform a (mutual) TLS handshake to forward Access-Request messages to the RadSec Server. To check the revocation status of a RadSec client certificate during the handshake, the certificate must be sent within the TLS tunnel.

#### **TLS 1.2**

In TLS 1.2, there is no method to request the RadSec client certificate during the handshake, so the handshake may complete before RADIUSaaS has fully authorised it, and your network device may perceive the channel being open and forwards requests even if this is not the case on our side. Your network device's RadSec client certificate is not transmitted until a client device authenticates, and we cannot check the revocation status of the certificate until then, which can lead to authentication timeouts or rejections because the client has to restart the authentication completely.

{% hint style="info" %}
To mitigate the above behaviour, the verification checks is deactivated when the maximum TLS version is set to 1.2. Please note you can manually reactive it later.&#x20;
{% endhint %}

#### **TLS 1.3**

TLS 1.3 allows explicitly requesting the RadSec client certificate before completing the handshake. This ensures that if the verification status is 'revoked', the handshake will fail immediately.&#x20;

{% hint style="info" %}
This setting is automatically enabled when the maximum TLS version is set to 1.3.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (461).png" alt=""><figcaption></figcaption></figure>

## RADIUS / UDP

This section is available when you have configured at least on [RADIUS Proxy](settings-proxy.md). For each proxy, a separate public IP address is available. The public IP addresses in this section support the RADIUS protocol only and thus listen on ports 1812/1813.

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

### **Server IP Addresses and Location**

{% hint style="warning" %}
These IP addresses only listen on [RADIUS](../../details.md#what-is-radius) over UDP ports 1812/1813.
{% endhint %}

Geo-location of the RADIUS Proxy/Proxies as well as the respective public IP address(es).

### **Shared Secrets**

The shared secret for the respective RADIUS Proxy. By default, all RADIUS Proxies are initialized with the same shared secret.

<figure><img src="../../../.gitbook/assets/2024-05-24_18h45_54.gif" alt=""><figcaption><p>Showing changing of shared secrets per proxy</p></figcaption></figure>

### **Ports**

This section displays the standard ports for the RADIUS authentication (1812) and RADIUS accounting (1813) services.

### **Failover & Redundancy**

#### Proxy Redundancy

Note that a single RADIUSaaS Proxy does not provide redundancy. To ensure redundancy, set up multiple RADIUSaaS Proxies as described [here](settings-proxy.md#load-balancing).

#### RadSec Service Redundancy for Proxies

When using RADIUSaaS with multiple RadSec instances, Proxies are automatically configured to connect to all available RadSec instances. A RADIUSaaS Proxy will prioritize connecting to the nearest regional RadSec Service. If that service is unavailable, it will switch to another available RadSec Service.&#x20;

## Server Certificates

### Customer-CA

By default, RADIUSaaS generates a **RADIUS Server Certificate** signed by a Certificate Authority (CA) that is available on our service solely for this very purpose. We refer to it as the **Customer-CA**. The Customer-CA is unique for each customer.

To create your Customer-CA, follow these simple steps:&#x20;

1. Navigate to **Settings** > **Server Settings**
2. Click **Add**
3. Choose **Let RaaS create a CA for you**
4. Click on **Save**
5. After the creation, you will see a new certificate available under Server Certificates

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

### Bring your own certificate

In case you do not want to use the **Customer CA**, you can upload up to two of your own certificates.

#### SCEPman-issued server certificate

Please follow these steps to leverage SCEPman Certificate Master to generate a new server certificate:

1. Navigate to your SCEPman Certificate Master web portal.
2. Select Request Certificate on the left
3. Select **(Web) Server** on the top
4. Select **Form**
5. Enter all Subject Alternative Names (SANs) that the certificate shall be valid for separated by commas, semicolons, or line breaks. Generate a server certificate as described [here](https://docs.scepman.com/certificate-deployment/certificate-master/tls-server-certificate-pkcs-12) and provide any FQDN you want. We recommend adapting the SAN of the default server certificate, e.g. `radsec-<your RADIUSaaS instance name>.radius-as-a-service.com`.
6. Set the **Download file format** to **PEM**&#x20;
7. Select **Include Certificate Chain** and download the certificate.&#x20;
8. **Submit** the request to download the new server certificate.

{% hint style="warning" %}
**Important**: Take temporary note of the password since it cannot be recovered from Certificate Master.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption><p>Create a new server certificate</p></figcaption></figure>

### Upload the new server certificate to **RADIUSaaS**

To add your server certificate created in above steps, navigate to **RADIUSaaS instance** > **Settings** > **Server Settings** > **Add** then

8. Choose **PEM or PKCS#12 encoded Certificate** (If you selected PKCS#12 in step 5, this contains both public and private key)
9. Drag & drop your certificate file or click to browse for it
10. Enter the password of your **Private Key**&#x20;
11. Click **Save**

<figure><img src="../../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Please note: By default, SCEPman Certificate Master issues certificates that are valid for 730 days. If you'd like to change this, please refer to SCEPman's [documentation](https://docs.scepman.com/advanced-configuration/application-settings/certificates#appconfig-validityperioddays).
{% endhint %}

### Certificate activation

{% hint style="warning" %}
Ensure to monitor the expiry of your server certificate and renew it in due time to prevent service interruptions.
{% endhint %}

As certificates expire from time to time or your preference on which certificates you would like to use change, it is important that you can control the certificate that your server is using. The **Active** column shows you the certificate your server is currently using. To change the certificate your server is using, expand the row of the certificate you would like to choose and click **Activate**.&#x20;

### Download

To download your **Server Certificate,** you have two options:&#x20;

1. Click **Download CA Certificate** on the top. This will directly download the **trusted root CA** of the currently **active** server certificate. &#x20;
2. &#x20;Click the **download** icon in the corresponding row.

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

**Option 2** will open a dialog showing the complete certificate path. The **root certificate** will always be marked in green.

<figure><img src="../../../.gitbook/assets/image (49).png" alt=""><figcaption><p>Showing the root certificate in green</p></figcaption></figure>

For both options, the downloaded root certificate is encoded in base64 (PEM). In case your device (e.g. WiFi controller) needs a binary coding (DER), you can convert it using [OpenSSL](https://openssl.org/):

```sh
openssl x509 -inform pem -in <DOWNLOADED_FILE> -outform der -out <CONVERTED_FILE>
```

### Delete

To delete a certificate, expand the corresponding row, click **Delete** and confirm your choice.&#x20;

### Certificate Expiration

{% hint style="danger" %}
Do not let the RADIUS Server Certificate expire. It will break the authentication.
{% endhint %}

Certificates expire from time to time. Five months before your certificate is going to expire, your dashboard will give you a hint by displaying a warning sign next to it.

![Screenshot showing certificate expiration](<../../../.gitbook/assets/image (111).png>)

If the triangle is diplayed next to the active RADIUS Server Certificate, follow this guide to update it:&#x20;

{% content-ref url="../../configuration/renew-certificate.md" %}
[renew-certificate.md](../../configuration/renew-certificate.md)
{% endcontent-ref %}
