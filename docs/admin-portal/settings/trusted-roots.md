# Trusted Certificates

## Types of Trusted Certificates

### Trusted CAs for Client Authentication

Trusted CAs for client authentication establish the trust between the CAs that issue client authentication certificates to your endpoint devices and RADIUSaaS.

### Trusted RadSec Connection Certificates

RadSec itself works with mutual certificate authentication (mTLS). This means, on the one hand your authenticators must [trust RADIUSaaS](settings-server.md#server-certificates) and on the other hand RADIUSaaS must know which authenticators to trust so that a valid RadSec connection can be established. The trusted RadSec connection certificates ensure RADIUSaaS only trusts those authenticators that you specify.

## **Pre-installed trusted RadSec Certificate**

Due to the [above](trusted-roots.md#trusted-radsec-connection-certificates), you will always see at least one RadSec trusted certificate that establishes the trust with your [RADIUS proxies](https://docs-preview.radiusaas.com/admin-portal/settings/settings-proxy) (effectively acting as RadSec clients). To ensure that your proxies are able to start up properly and are able to establish a connection to your instance, you cannot delete it.

## Add&#x20;

![Showing how to add trusted certificates](../../.gitbook/assets/2024-05-14\_11h50\_26.gif)

{% hint style="warning" %}
If you have a tiered PKI infrastructure (e.g. a **Microsoft legacy PKI**), remember to **upload** the **entire chain of trust**, i.e. the root CA certificate and all relevant issuing CA certificates (RADIUSaaS supports the upload of a combined file or separate files for each CA).
{% endhint %}

To add a new trusted certificate, follow these steps:

1. Click **Add**
2. Select if you want to import to trusted certificate&#x20;
   * from SCEPman (by providing the URL to your SCEPman instance), or
   * from any other CA (by uploading a PEM or DER-encoded certificate file)
3. Select the [type](trusted-roots.md#types-of-trusted-certificates) of trusted certificate:
   * [**Client authentication**](trusted-roots.md#trusted-cas-for-client-authentication)
   * [**RadSec**](trusted-roots.md#trusted-radsec-connection-certificates)
   * **Both** (helpful if the same CA is issuing your client authentication certificates as well as your RadSec client certificates)
4. Upload the certificate file by drag & drop (alternatively you can click in the blue area and select your file)
5. Select the certificate verification option:
   * **OCSP Autodetect**: RADIUSaaS will try to infer the OCSP responder URL from the client certificate's Authority Information Access (AIA) extension that is used for the network authentication. In case no OCSP responder URL is found or the OCSP responder is unavailable, RADIUSaaS will consider the [**Soft fail** ](trusted-roots.md#ocsp-soft-fail)configuration. Use this if your CA is **SCEPman**.
   * **OCSP**: Manually specify which OCSP responder URL will be used for any certificate that was issued by the trusted CA. If the OCSP responder is unavailable, RADIUSaaS will consider the [**Soft fail** ](trusted-roots.md#ocsp-soft-fail)configuration.
   * **CRL**: If selected, RADIUSaaS will use a CRL instead of OCSP to verify certificate issued by the CA. Specify the CRL encoding (**DER** or **PEM**) and the **CRL distribution points**.
6. Click **Save**

## Delete

To delete a certificate, expand the corresponding row, click **Delete** and confirm your choice.&#x20;

<figure><img src="../../.gitbook/assets/image (392).png" alt=""><figcaption><p>Showing deletion of a trusted certificate</p></figcaption></figure>

## OCSP Soft-fail

This setting determines how RADIUSaaS behaves, when the OCSP responder of your trusted CA cannot be reached.&#x20;

{% hint style="danger" %}
If you **disable** this setting, authentication requests will be **rejected** when your CA's OCSP responder cannot be reached.
{% endhint %}

{% hint style="info" %}
Note that this setting is only available when **OCSP Autodetect** or **OCSP** is selected for a certificate verification.&#x20;
{% endhint %}

By default, we **recommend enabling OCSP Soft fail** to increase the availability of the service by allowing authentication requests to be accepted even if the OCSP responder cannot be reached. With this **soft fail** mechanism, and in case OCSP is not reachable, RADIUSaaS will only check if the incoming certificate was signed by one of the [trusted CAs](trusted-roots.md) and processes any optional [Rules](rules/).

<figure><img src="../../.gitbook/assets/image (390).png" alt=""><figcaption><p>Showing OCSP Hard-Fail</p></figcaption></figure>

## Tiered PKI Hierarchy

In a tiered CA environment with multiple issuing CAs configured, certificates issued from the root CA or any issuing CA will get access regardless of whether the certificate was issued from the root or issuing CA. This means that trusting the root will automatically trust certificates issued by any of the issuing CAs. \
\
If access needs to be controlled based on the issuing CAs, this can be achieved by configuring [respective rules](rules/#certificate-based-authentication).

### Considerations

When uploading root and issuing CAs of a tiered PKI, please consider the following:

* The **root CA must be uploaded first**, before uploading any issuing CAs derived from this root CA.
* Root and issuing CAs must be uploaded in separate files. Uploading certificate chains from a single file may result in unexpected behaviour.

## Optional Settings

### Intune ID

It is possible to use the certificate which every Windows 10 machine receives from Intune when joining Microsoft Entra ID (Azure AD).

{% hint style="danger" %}
This setting is optional. In case you are not familiar with Intune Certificates, please do not configure them!
{% endhint %}

#### Why are we not recommending using Intune certificates for authentication purposes?

* Intune certificates are not supported by Microsoft for other purposes than management with Intune.
* Validity time for the Intune certificates is 1 year.
* There is no mechanism to revoke certificates (like OCSP).

Instead of using Intune certificates, we recommend using certificates from a PKI like [SCEPman](https://scepman.com/).

#### Configure Intune IDs

{% hint style="danger" %}
Use this setting with care. One of the following IDs **must** exist in the certificate extension 1.2.840.113556.5.14. All other certificates will **be rejected**.
{% endhint %}

To get your Intune Tenant ID, follow these steps:&#x20;

* Press **Windows + R** and enter **certlm.msc**
* Go to your personal certificates. There will be a certificate from one of the following issuers
  * SC Online Issuing
  * MDM Device Authority&#x20;
*   Open this certificate, go to **Details** and search for the extension&#x20;

    1.2.840.113556.5.14
* The printed HEX value is your Intune Tenant ID.&#x20;

**Example:**

The Tenant ID of the following certificate is: **bb4397cb6891c64db17f766487518a6a**

![Showing Certificate](<../../.gitbook/assets/image (250).png>)

## XML

Today, most MDM platforms provide a wizard-based method to deploy networking profiles (WiFi and LAN). If this is not possible, or if the wizard provides only limited configuration options, you may generate a raw-XML profile directly on the RADIUSaaS platform.

* To generate your **WiFi** XML profile, expand the **XML** section, enter your **SSID** and click **Download**.
* To generate your **Wired** XML profile, click **Download**.

<figure><img src="../../.gitbook/assets/2024-05-23_14h23_04 (1).gif" alt=""><figcaption><p>Showing downloading of WiFi profile as an XML file</p></figcaption></figure>

