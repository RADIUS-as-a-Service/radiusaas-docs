# Certificates

On your **Server Settings** page there are two tables with certificate information. Both tables will contain at least one certificate to ensure a normal operation of your systems. &#x20;

#### List of all available Server Certificates

The [first table](./#server-certificates) shows all available certificates your RADIUS server is able to use.&#x20;

#### List of allowed RadSec Connection Certificates

The [second table](./#radsec-connection-certificates) contains all certificates that are allowed to establish a [RadSec](../../../details.md#what-is-radsec) connection.&#x20;

## Server Certificates

### Default Certificates

By default, RADIUSaaS generates a RADIUS server certificate signed by a Certificate Authority (CA) that is available on our service solely for this very purpose. We refer to it as the **Custom CA**. The Custom CA is unique for each customer.

### Custom CAs

To create your Custom CA, follow these simple steps:&#x20;

1. Click **Add**
2. Choose **Let RaaS create a CA for you**
3. Click on **Create**

![](<../../../.gitbook/assets/image (73) (1) (1).png>)

After the creation, you will see a new certificate available in your table:

![](<../../../.gitbook/assets/image (84) (1).png>)

### Bring your own Certificate

In case you do not want to use any of the standard certificates which we are providing, you can upload up to two of your own certificates.

#### SCEPman Server Certificate

You may leverage SCEPman Certificate Master to generate a server certificate for you. Please follow those steps:

1. Navigate to your SCEPman Certificate Master web portal.
2. Generate a server certificate as described [here](https://docs.scepman.com/certificate-deployment/certificate-master/tls-server-certificate-pkcs-12) and provide any FQDN you want. We recommend to adapt the SAN of the default server certificate, i.g. `radius.<your RADIUSaaS instance name>.net`.
3.  Set the **Download file format** to **PEM with certificate chain** and download the certificate. <mark style="color:red;">**Important:**</mark> <mark style="color:red;"></mark><mark style="color:red;">Take temporary note of the password since it cannot be recovered from Certificate Master.</mark>\


    <figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>
4.  Navigate to your RADIUSaaS instance and upload the server certificate file. Subsequently, provide the password and click **Save**.\


    <figure><img src="../../../.gitbook/assets/image (2) (5).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Please note: By default, SCEPman Certificate Master issues certificates that are valid for 730 days. If you'd like to change this, please refer to SCEPman's [documentation](https://docs.scepman.com/advanced-configuration/application-settings/certificates#appconfig-validityperioddays).
{% endhint %}

#### Add the Certificate

To add your own server certificate, e.g. one issued by SCEPman, please follow those steps.

1. Click **Add**
2. Choose **PEM encoded Certificate**
3. Copy & Paste your certificate or use the **Browse File** option
4. Enter the password of your **Private Key**&#x20;
5. Click **Save**

### Certificate Activation

As certificates expire from time to time or your preference on which certificates you would like to use change, it is important that you can control the certificate that your server is using. The **Active** column shows you the certificate your server is currently using. To change the certificate your server is using, expand the row of the certificate you would like to choose and click **Activate**.&#x20;

### Download

To download your **Server Certificate**  click **Download** in the corresponding row.

![](<../../../.gitbook/assets/image (68) (1).png>)

It will open a dialog, and show the complete certificate path. The **root certificate** will always be marked in green.

![](<../../../.gitbook/assets/image (75) (1).png>)

## RadSec Connection Certificates

RadSec itself works with certificate authentication as well. Hence, your RADIUS server has to know who is allowed to establish a valid RadSec connection. Due to this requirement, you will always see at least one certificate in this table, which is the one related to your [RadSec proxy](../settings-proxy.md). To ensure that your proxies are able to start up properly and are able to establish a connection to your instance, you cannot delete it.&#x20;

### Add a new Certificate

To allow new clients to establish a RadSec connection to your instance, follow these steps:

1. Click **Add**
2. Copy & Paste your certificate or use the **Browse File** option
3. Click **Save**

After this you should see your imported certificate in your table.

![](<../../../.gitbook/assets/image (62).png>)

### Delete

To delete a certificate, expand the corresponding row, click **Delete** and confirm your choice.&#x20;

![](<../../../.gitbook/assets/image (77) (1) (1).png>)

## Certificate expiration&#x20;

Certificates will expire from time to time. Five months before your certificate is going to be expired, you dashboard will give you a hint that your certificate is about to expire.

![](<../../../.gitbook/assets/image (74) (1).png>)

If you're seeing this triangle, follow this guide how you can change your certificate:&#x20;

{% content-ref url="../../../how-to-use/renew-certificate.md" %}
[renew-certificate.md](../../../how-to-use/renew-certificate.md)
{% endcontent-ref %}
