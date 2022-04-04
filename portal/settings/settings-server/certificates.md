# Certificates

On your **Server Settings** page there are two tables with certificate information. Both tables will contain at least one certificate to ensure a normal operation of your systems. &#x20;

#### List of all available Server Certificates

The [first table](./#server-certificates) shows all available certificates your RADIUS server is able to use.&#x20;

#### List of allowed RadSec Connection Certificates

The [second table](./#radsec-connection-certificates) contains all certificates that are allowed to establish a [RadSec](../../../details.md#what-is-radsec) connection.&#x20;

## Server Certificates

### Default Certificate

The certificate which will be created automatically during set up of your RADIUSaaS instance is a self-signed end user certificate.&#x20;

### Custom CAs

{% hint style="warning" %}
A custom CA or your own bought certificate is required if you are planning to authenticate Android devices.
{% endhint %}

In some cases, you might be required to create your own custom CA. For example, Android devices (version > Android 9) will not allow to install a end user certificate as trusted CA. So you need a CA certificate. To achieve this you're able to create your own CA in with your instance.

To create your custom CA, follow these simple steps:&#x20;

1. Click **Add**
2. Choose **Create your own CA**
3. Click on **Create**

![](<../../../.gitbook/assets/image (49).png>)

After the creation, you will see a new certificate available in your table:

![](<../../../.gitbook/assets/image (48).png>)

### Bring your own Certificate

In case you do not want to use any of the standard certificates which we are providing you can upload up to two of your own certificates.

1. Click **Add**
2. Choose **PEM encoded Certificate**
3. Copy & Paste your certificate or use the **Browse File** option
4. Enter the password of your **Private Key**&#x20;
5. Click **Save**

### Certificate Activation

As certificates expire from time to time or your preference on which certificates you would like to use change, it is important that you can control the certificate that your server is using. The **Active** column shows you the certificate your server is currently using. To change the certificate your server is using, expand the row of the certificate you would like to choose and click **Activate**.&#x20;

### Download

To download your **Server Certificate**  click **Download** in the corresponding row.

![](<../../../.gitbook/assets/image (46).png>)

## RadSec Connection Certificates

RadSec itself works with certificate authentication as well. Hence, your RADIUS server has to know who is allowed to establish a valid RadSec connection. Due to this requirement, you will always see at least one certificate in this table, which is the one related to your [RadSec proxy](../settings-proxy.md). To ensure that your proxies are able to start up properly and are able to establish a connection to your instance, you cannot delete it.&#x20;

### Add a new Certificate

To allow new clients to establish a RadSec connection to your instance, follow these steps:

1. Click **Add**
2. Copy & Paste your certificate or use the **Browse File** option
3. Click **Save**

After this you should see your imported certificate in your table.

![](<../../../.gitbook/assets/image (51).png>)

### Delete

To delete a certificate, expand the corresponding row, click **Delete** and confirm your choice.&#x20;

![](<../../../.gitbook/assets/image (52).png>)

## Certificate expiration&#x20;

Certificates will expire from time to time. Five months before your certificate is going to be expired, you dashboard will give you a hint that your certificate is about to expire.

![](<../../../.gitbook/assets/image (56).png>)

If you're seeing this triangle, follow this guide how you can change your certificate:&#x20;

{% content-ref url="../../../how-to-use/renew-certificate.md" %}
[renew-certificate.md](../../../how-to-use/renew-certificate.md)
{% endcontent-ref %}
