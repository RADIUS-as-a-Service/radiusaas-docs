# MikroTik

{% hint style="info" %}
Please note that the below configuration was tested with RouterOS 6.47.4 and 6.49.11
{% endhint %}

To establish a valid RadSec connection, your MikroTik Access Points must trust the RADIUS Server Certificate and your RADIUS server must trust your RadSec client certificate. To establish the server trust , follow these steps:&#x20;

1. Download the root certificate of the CA that has issued your RADIUS server certificate as described [here](../../../admin-portal/settings/settings-server.md#download).
2. Log on to your MikroTik device, then upload the certificate from above to the MikroTik device using the **Files** menu on the left.
3. Once uploaded, switch to your **Terminal** tab on the top right and execute the following command to import this certificate to MikroTik's certificate store:

```
/certificate import file-name="RADIUS Customer CA - Contoso.cer"
```

4. If you have not already generated RadSec client certificate for your router, generate one as per the below example. For more information about creating certificates, click [here](https://wiki.mikrotik.com/wiki/Manual:Create\_Certificates). &#x20;

{% hint style="warning" %}
Ensure to monitor the expiry of your RadSec client certificate and renew it in due time to prevent service interruptions.
{% endhint %}

Example:&#x20;

```
/certificate add name=myCa common-name=myCa key-usage=key-cert-sign,crl-sign
/certificate add name=mikrotik-client common-name=mikrotik-client
/certificate sign mikrotik-client ca=myCa name=mikrotik-client
```

In the above example, the first line creates a root CA called **myCa**. The second line generates a client certificate for the MikroTik device, and the third line uses **myCa** (CA) to sign the **mikrotik-client** certificate generated in step 2. If all went well, you would end up with three certificates as shown below. Please ensure your MikroTik device trusts the relevant certificates (T flag in the green section). If that is not the case yet, set the flag using below command:

```
/certificate
set myCa trusted=yes
set "RADIUS Customer CA - Contoso.cer" trusted=yes
```

<figure><img src="../../../../.gitbook/assets/image (344).png" alt=""><figcaption></figcaption></figure>

5. Export the root CA certificate (`myCa`) that has issued your RadSec client certificate above:

```
/certificate export-certificate myCa
```

6. Download it from the **Files** menu and then upload the file to your RADIUS instance as a trusted[ RadSec connection certificate](../../../admin-portal/settings/settings-server.md#add).
7. Switch back to your WebFig, add a new RADIUS profile and enter the following information:

* Use the IP address from your [Server Settings ](../../../admin-portal/settings/settings-server.md)page.
* **Protocol:** radsec
* **Secret:** radsec
* **Authentication Port:** 2083
* **Accounting Port:** 2083
* **Certificate:** mikrotik-client (generated in step 4)&#x20;

![](../../../.gitbook/assets/2024-09-03\_14h43\_36.png)

8. Go to **Wireless** add a new **Security Profile** and enter the following information:&#x20;

* **Name:** on your behalf
* **Mode:** dynamic keys
* **EAP Methods:** passthrough
* **TLS Mode:** verify certificate
* **TLS Certificate:** the imported RADIUS Server certificate

![](<../../../../.gitbook/assets/image (158).png>)

9. Switch to your **WiFi Interfaces** and apply your **Security Profile** to the interface.

![](<../../../../.gitbook/assets/image (266).png>)
