# Troubleshooting

## Client View

### Wrong XML&#x20;

![](<../.gitbook/assets/image (43).png>)

Check that your client has a certificate to authenticate. And you're using the correct [XML](../portal/settings/settings-trusted-roots/xml.md#wifi)

### Trusted Root issues&#x20;

![](<../.gitbook/assets/image (44).png>)

Check that you've done the following:&#x20;

* Told your RADIUS Server which certificates are allowed to connect as described [here](../portal/settings/settings-trusted-roots/trusted-roots.md#add)
* Imported the active RADIUS Server certificate as trusted root on your client as described [here](../azure/trusted-root.md#to-add-a-trusted-root-profile-for-your-clients)

Also check your [Logs](../portal/insights/log.md#logs). There is a detailed description of the error. May it's [this](trubleshooting.md#fatal-decrypt-error) issue.



If your Clients need to verify on connecting the first time, and you're seeing this dialog:

![](<../.gitbook/assets/image (60).png>)

Make sure that you have referenced the Server certificate in your WiFi Profile:

![](<../.gitbook/assets/image (61).png>)

## RADIUS Server view

### Unknown CA

if you see something like this in your [Logs](../portal/insights/log.md#logs):

```
Mon Jul 12 12:38:09 2021 : ERROR: (14872) eap_tls:   ERROR: SSL says error 20 : unable to get local issuer certificate
Mon Jul 12 12:38:09 2021 : ERROR: (14872) eap_tls: ERROR: TLS Alert write:fatal:unknown CA
Mon Jul 12 12:38:09 2021 : Error: tls: TLS_accept: Error in error
Mon Jul 12 12:38:09 2021 : Auth: (14872) Login incorrect (eap_tls: SSL says error 20 : unable to get local issuer certificate): [host/8dc38402-20fb-41db-a8f3-4e4e95637173/<via Auth-Type = eap>] (from client contoso port 1 cli 18-9K-EA-0H-7F-C5)
```

It can be one of this options:&#x20;

1. Your RADIUS server don't know the issuer of the certificate which was used for authentication. Add your CA as described [here](../portal/settings/settings-trusted-roots/trusted-roots.md#add).
2. Your Client don't know the **Server certificate** and rejects the connection. Check that you've added your **Server certificate** as described [here](../azure/trusted-root.md#adding-a-trusted-root-profile-for-your-clients).
3. You've changed/added a new **Server certificate** and your XML profile on the client is wrong. Check that you've generated your [XML](../portal/settings/settings-trusted-roots/xml.md#wifi) after adding the certificates and push that to your clients.&#x20;

### Fatal decrypt error

If you can see something like this in your [Logs](../portal/insights/log.md#logs):

```
Wed Apr  7 08:14:39 2021 : Auth: (312) Login incorrect (eap_tls: TLS Alert write:fatal:decrypt error): [host/00128t09-cbna-469c-9768-2783d28eikl9/<via Auth-Type = eap>] (from client contoso port 1 cli 84-FD-D1-8C-0E-33)
Wed Apr  7 08:14:41 2021 : ERROR: (320) eap_tls: ERROR: TLS Alert write:fatal:decrypt error
Wed Apr  7 08:14:41 2021 : Error: tls: TLS_accept: Error in error
```

... then it is probably a bug of the TPM software on your Windows machines. More information on that can be found in the [SCEPman documentation](https://docs.scepman.com/certificate-deployment/microsoft-intune/windows-10).

{% hint style="warning" %}
The setting Key Storage Provider (KSP) determines the storage location of the private key for the end-user certificates. Storage in the TPM is more secure than software storage, because the TPM provides an additional layer of security to prevent key theft. However, there is **a bug in some older TPM firmware versions** that invalidates some signatures created with a TPM-backed private key. In such cases, the certificate cannot be used for EAP authentication as it is common for Wi-Fi and VPN connections. Affected TPM firmware versions include:

* STMicroelectronics: 71.12, 73.4.17568.4452, 71.12.17568.4100
* Intel: 11.8.50.3399, 2.0.0.2060
* Infineon: 7.63.3353.0

If you use TPM with this firmware, either update your firmware to a newer version or select "Software KSP" as key storage provider.
{% endhint %}
