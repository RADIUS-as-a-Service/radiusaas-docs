# MikroTik

To establish a valid TLS connection, your client has to know the RADIUS Server Certificate and your RADIUS Server needs to know your Client Certificate. To import your Server Certificate, follow these steps:&#x20;

* Download your RADIUS [Server Certificate](../../../portal/settings/settings-server/#download)
* Import it to your Mikrotik client:
  * Upload the certificate with the files section
  * Switch to your WebFig and enter

```
/certificate import filename=yournameServercertificate.cer
```

If you have not already gotten a certificate for your router, generate one as described [here](https://wiki.mikrotik.com/wiki/Manual:Create\_Certificates).&#x20;

Example:&#x20;

```
/certificate add name=ca-template common-name=myCa key-usage=key-cert-sign,crl-sign
/certificate add name=mikrotik-client common-name=mikrotik-client
/certificate sign mikrotik-client ca=myCa name=mikrotik-client
```

Export your generated certificate:

```
/certificate export-certificate mikrotik-client
```

Upload the file to your RADIUS instance as as trusted[ RadSec connection certificate](../../../portal/settings/settings-server/#add).

Switch back to your WebFig, add a new RADIUS profile and enter the following information:

* Use the IP address from your [Server Settings ](../../../portal/settings/settings-server/)page
* **Protocol:** radsec
* **Secret:** radsec
* **Authentication Port:** 2083
* **Accounting Port:** 2083
* **Certificate:** the generated before&#x20;

![](<../../../.gitbook/assets/image (39) (1).png>)

Go to **Wireless** add a new **Security Profile** and enter the following information:&#x20;

* **Name:** on your behalf
* **Mode:** dynamic keys
* **EAP Methods:** passthrough
* **TLS Mode:** verify certificate
* **TLS Certificate:** the imported RADIUS Server certificate

![](<../../../.gitbook/assets/image (40).png>)



Switch to your **WiFi Interfaces** and apply your **Security Profile** to the interface.

![](<../../../.gitbook/assets/image (41) (1).png>)
