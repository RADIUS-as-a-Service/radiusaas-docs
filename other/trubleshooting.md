# Troubleshooting

## Connection Issues

### Client View

#### Wrong XML&#x20;

![](<../.gitbook/assets/image (43).png>)

Check that your client has a certificate to authenticate and that you are using the correct [WiFi configuration profile](../azure/microsoft-intune/wifi-profile/) or [XML](../portal/settings/settings-trusted-roots/xml.md#wifi).

#### Trusted Root issues&#x20;

![](<../.gitbook/assets/image (44).png>)

Check that you've done the following:&#x20;

* Told your RADIUS Server which certificates are allowed to connect as described [here](../portal/settings/settings-trusted-roots/trusted-roots.md#add)
* Imported the active RADIUS Server certificate as trusted root on your client as described [here](../azure/microsoft-intune/trusted-root.md#to-add-a-trusted-root-profile-for-your-clients)

Also check your [Logs](../portal/insights/log.md#logs). There is a detailed description of the error. Maybe it's [this](trubleshooting.md#fatal-decrypt-error) issue.

If your Clients need to verify on connecting the first time, and you're seeing this dialog:

![](<../.gitbook/assets/image (60).png>)

Make sure that you have referenced the RADIUS server certificate in your WiFi profile and provided the server certificate's SAN attribute (FQDN) and common name (CN):

<figure><img src="../.gitbook/assets/image (2) (2) (1).png" alt=""><figcaption></figcaption></figure>

### Server View

#### Unknown CA

If your [Logs](../portal/insights/log.md#logs) contain error messages similar to the ones shown below

```
Mon Jul 12 12:38:09 2021 : ERROR: (14872) eap_tls:   ERROR: SSL says error 20 : unable to get local issuer certificate
Mon Jul 12 12:38:09 2021 : ERROR: (14872) eap_tls: ERROR: TLS Alert write:fatal:unknown CA
Mon Jul 12 12:38:09 2021 : Error: tls: TLS_accept: Error in error
Mon Jul 12 12:38:09 2021 : Auth: (14872) Login incorrect (eap_tls: SSL says error 20 : unable to get local issuer certificate): [host/8dc38402-20fb-41db-a8f3-4e4e95637173/<via Auth-Type = eap>] (from client contoso port 1 cli 18-9K-EA-0H-7F-C5)
```

it can have the following root causes:&#x20;

* Client throws error --> `TLS Alert read:fatal:unknown CA`
  * Your Client doesn't know the **Server certificate** and rejects the connection. Check that you've added your **Server certificate** as described [here](../azure/microsoft-intune/trusted-root.md#adding-a-trusted-root-profile-for-your-clients).
  * You've changed/added a new **Server certificate** and your XML profile on the client is using the old one. In that case, please double-check that you've either updated your WiFi/Wired profile or re-generated your [XML](../portal/settings/settings-trusted-roots/xml.md#wifi) after adding the certificates and pushed that to your clients.&#x20;
* `Server throws error --> TLS Alert write:fatal:unknown CA`
  * Your RADIUS server doesn't know the issuer of the certificate which was used for authentication. Add your CA as described [here](../portal/settings/settings-trusted-roots/trusted-roots.md#add).

#### Fatal decrypt | Access denied

If your [Logs](../portal/insights/log.md#logs) contain error messages similar to the ones shown below

```
Wed Apr  7 08:14:39 2021 : Auth: (312) Login incorrect (eap_tls: TLS Alert write:fatal:decrypt error): [host/00128t09-cbna-469c-9768-2783d28eikl9/<via Auth-Type = eap>] (from client contoso port 1 cli 84-FD-D1-8C-0E-33)
Wed Apr  7 08:14:41 2021 : ERROR: (320) eap_tls: ERROR: TLS Alert write:fatal:decrypt error
Wed Apr  7 08:14:41 2021 : Error: tls: TLS_accept: Error in error
```

... then it is probably a bug of the TPM software on your Windows machines. More information on that can be found in the [SCEPman documentation](https://docs.scepman.com/certificate-deployment/microsoft-intune/windows-10#key-storage-provider-ksp-enroll-to-trusted-platform-module-tpm-ksp-otherwise-fail).

If you see something like this in your [Logs](../portal/insights/log.md#logs)

```
Wed Dec 14 07:24:24 2022 : ERROR: (95878) eap_tls: ERROR: (TLS) Alert read:fatal:access denied
Wed Dec 14 07:24:24 2022 : Auth: (95878) Login incorrect (eap_tls: (TLS) Alert read:fatal:access denied): [host/kad933-161d-4aa8-aeaa-b5a4d3d53b12]
Wed Dec 14 07:11:06 2022 : ERROR: (95717) eap_tls: ERROR: (TLS) Alert read:fatal:access denied
```

... there can be two reasons. One is that your WiFi profile is referencing the wrong root certificate for server validation. Please make sure that your profile is setup correctly. If it is and you are still facing this issue, try to set your KSP to [**Software KSP**](https://docs.scepman.com/certificate-deployment/microsoft-intune/windows-10#key-storage-provider-ksp-enroll-to-trusted-platform-module-tpm-ksp-otherwise-fail).&#x20;

{% hint style="warning" %}
The setting Key Storage Provider (KSP) determines the storage location of the private key for the end-user certificates. Storage in the TPM is more secure than software storage, because the TPM provides an additional layer of security to prevent key theft. However, there is **a bug in some older TPM firmware versions** that invalidates some signatures created with a TPM-backed private key. In such cases, the certificate cannot be used for EAP authentication as it is common for Wi-Fi and VPN connections. Affected TPM firmware versions include:

* STMicroelectronics: 71.12, 73.4.17568.4452, 71.12.17568.4100
* Intel: 11.8.50.3399, 2.0.0.2060
* Infineon: 7.63.3353.0
* IFX: Version 3.19 / Specification 1.2

If you use TPM with this firmware, either update your firmware to a newer version or select "Software KSP" as key storage provider.
{% endhint %}

#### Wrong Shared RADIUS Secret

If your [Logs](../portal/insights/log.md#logs) contain error messages similar to the ones shown below

```
Thu Jul 27 14:28:23 2023: message authenticator, wrong value
Thu Jul 27 14:28:23 2023: buf2radmsg: message authentication failed
Thu Jul 27 14:28:23 2023: radsrv: ignoring request from default (8.222.246.231), validation failed.
```

then one of your access points or switches that is trying to connect to your RADIUSaaS instance via a [RADIUS Proxy](../portal/settings/settings-proxy.md) is transmitting a mismatching [Shared Secret](../portal/settings/settings-server/ports-and-ip-addresses.md#shared-secrets).

To identify the affected access point or switch, first determine the RADIUS proxy by expanding the error message and searching for the `proxyip` property. Now that you know the proxy, use your inventory and knowledge of specific locations or groups of devices that cannot connect to your network to identify the misconfigured network device. Finally, update the shared secret to match the value configured in your RADIUSaaS instance for that proxy.

## Admin Portal Issues

### Login

In order to log in to the RADIUSaaS web portal ("RADIUSaas Admin Portal"), the following requirements have to be met:

* The UPN/email address you provided as technical admin has to be authenticatable against **any** Azure AD.
* The UPN/email address you provided as technical admin must have been registered on your RADIUSaaS instance as described [here](../portal/settings/permissions.md). In case it is the initial user, please [contact us](https://www.radius-as-a-service.com/help/) if you believe we registered the wrong user.
* The Azure AD user object behind the UPN/email address has to be entitled to grant the RADIUSaaS Enterprise Application the following permissions (see screenshot below):
  * **Read** the Basic User Profile
  * **Maintain** access to data you have given it access to (allow request of refresh token)\
    ![](../.gitbook/assets/Screenshot\_2022-04-11\_at\_09\_31\_26.png)
* In case your Azure AD user has no rights to grant the required permissions, no corresponding **Enterprise Application** will be auto-created in your Azure AD. To circumvent this, either ask you IT department to grant your user the needed permissions or alternatively, they may manually create the required Enterprise Application and assign your user to it.
* To manually create the Enterprise Application, please follow these steps:
  * **Create** a new Enterprise Application
  * Give it a **name** such as "RADIUSaaS Admin Center"
  * Enable **users sign-in**
  * Set the **Homepage URL** to [`https://radius-as-a-service.com`](https://radius-as-a-service.com)
  * Optionally, apply your **Conditional Access** policies
  * Configure the following permissions (either an Admin or User consent level):\
    <img src="../.gitbook/assets/image (78) (1) (1) (1).png" alt="" data-size="original">
  * Under **Users and groups** assign every relevant RADIUSaaS admin that shall be able to access the platform.
