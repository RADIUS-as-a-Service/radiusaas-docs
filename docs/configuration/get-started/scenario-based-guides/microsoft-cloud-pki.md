---
description: >-
  This document describes the configuration steps necessary to implement
  certificate-based WiFi authentication using Microsoft Cloud PKI with Intune.
---

# Microsoft Cloud PKI

{% hint style="info" %}
It is assumed that the Microsoft Cloud PKI hosts both the Root and Issuing CA. For scenarios involving BYOCAs please refer to Microsoft's online resources or the web.
{% endhint %}

## Step 1: Deploy a Microsoft Cloud PKI

### Create a Root CA in Intune admin center <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

Before you can issue certificates to managed devices, you need to create a root CA in your tenant to act as the trust anchor. To create a root CA in Intune admin center, please follow [this ](https://learn.microsoft.com/en-gb/mem/intune/protect/microsoft-cloud-pki-configure-ca)Microsoft guide.&#x20;

{% hint style="info" %}
Please take note of the CRL distribution point as you will need this later in [Step 2](microsoft-cloud-pki.md#step-1-create-root-ca-in-admin-center-2).&#x20;
{% endhint %}

### Create an Issuing CA in Intune admin center <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

An issuing CA is required to issue certificates for Intune-managed devices. Cloud PKI automatically provides a SCEP service that acts as a certificate registration authority. It requests certificates from the issuing CA on behalf of Intune-managed devices using a SCEP profile. To create an issuing CA, please follow [this ](https://learn.microsoft.com/en-gb/mem/intune/protect/microsoft-cloud-pki-configure-ca#step-2-create-issuing-ca-in-admin-center)Microsoft guide.&#x20;

{% hint style="info" %}
Please take note of the CRL distribution point as you will need this later in [Step 2](microsoft-cloud-pki.md#step-1-create-root-ca-in-admin-center-2).
{% endhint %}

<figure><img src="../../../../.gitbook/assets/image (346).png" alt=""><figcaption><p>Root and Issuing CAs</p></figcaption></figure>

## Step 2: Establish trust between RADIUSaaS and the Microsoft Cloud PKI <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

Configure RADIUSaaS to trust client authentication certificates issued by the Microsoft Cloud PKI. Since the cloud PKI requires a tiered CA structure, you must upload both, Root CA and Issuing CA (i.e. the complete chain of trust). To achieve this, please follow below steps:

1. Navigate to [Trusted Certificates](../../../admin-portal/settings/trusted-roots.md).
2. [Upload](../../../admin-portal/settings/trusted-roots.md#add) the Contoso Cloud PKI **Root CA** selecting **Client Authentication** in the upload process.
3. As verification method select **CRL** along with **DER** encoding.
4. Use the copied CRL distribution point URL of the Root CA in the **CRL Distribution Points** URL input field.\
   ![](<../../../../.gitbook/assets/image (348).png>)
5. Upload the Contoso Cloud PKI **Issuing CA** selecting **Client Authentication** in the upload process.
6. Again, select **CRL** as verification method along with **DER** encoding.
7. Use the copied CRL distribution point URL of the Issuing CA in the **CRL Distribution Points** URL input field.\
   ![](<../../../../.gitbook/assets/image (349).png>)

## Step 3: Configure the RADIUS Server Certificate

To establish server trust between your endpoint devices and RADIUSaaS, follow [these instructions](../generic-guide.md#step-3-radius-server-certificate-configuration).

## Step 4: Configure your networking equipment

To configure your networking equipment (WiFi access points, switches, or VPN gateways), follow [these steps](../generic-guide.md#step-4-network-equipment-configuration).

After successful completion of Steps 2 - 4, the **Trusted Certificates** page of your RADIUSaaS instance will look similar to the one below. Please note that in our example we have used a RadSec-enabled [MikroTik](../../access-point-setup/radsec-available/mikrotik.md) access point.

<figure><img src="../../../../.gitbook/assets/image (350).png" alt=""><figcaption><p>Trusted Certificates Overview required for the Microsoft Cloud PKI.</p></figcaption></figure>

## Step 5: Configure Intune Profiles

To set up certificate-based WiFi authentication, we need to create a number of profiles and deploy them via Intune. These profiles are:

| Profile Type        | Purpose                                                                       |
| ------------------- | ----------------------------------------------------------------------------- |
| Trusted certificate | Deploy the Root CA certificate.                                               |
| Trusted certificate | Deploy the Issuing CA certificate.                                            |
| Trusted certificate | Deploy the Root CA certificate that has issued the RADIUS Server Certificate. |
| SCEP certificate    | Enroll the client authentication certificate.                                 |
| Wi-Fi               | Deploy the wireless network adapter settings.                                 |

<figure><img src="../../../../.gitbook/assets/image (345).png" alt=""><figcaption><p>Relevant Intune Profiles</p></figcaption></figure>

### Trusted certificate profiles

#### Microsoft Cloud PKI

Deploy the root CA and issuing CA certificates created in [Step 1](microsoft-cloud-pki.md#step-1.-deploy-a-microsoft-cloud-pki) via a **Trusted certificate** profile to your devices by navigating to the **Intune admin center** and then to **Home** > **Devices** > **Windows** > **Configuration profiles > Create** > **New Policy** with the following parameters:&#x20;

* Platform = Windows 10 and later
* Profile type = Template
* Template name = Trusted certificate.

Upload the relevant certificate file (\*.cer) in the respective profile:

* Root CA certificate created [here](microsoft-cloud-pki.md#step-1-create-root-ca-in-admin-center)
* Issuing CA certificate created [here](microsoft-cloud-pki.md#step-1-create-root-ca-in-admin-center-1)

{% hint style="info" %}
Note that you have to use the same group for assigning the Trusted certificate and SCEP profiles. Otherwise, the Intune deployment might fail.
{% endhint %}

This must be repeated for every device platform that shall be using the service (e.g. Windows, macOS, ...)

#### RADIUS server trust

Next, push the root CA certificate that has issued your RADIUS Server Certificate as described here:

{% content-ref url="../../../profile-deployment/microsoft-intune/trusted-root.md" %}
[trusted-root.md](../../../profile-deployment/microsoft-intune/trusted-root.md)
{% endcontent-ref %}

### SCEP certificate profile

To create a **SCEP certificate** profile in Intune admin center, first take a copy of the SCEP URI from **Home** > **Tenant admin** > **Cloud PKI** > **Contoso Issuing CA** > **Properties** > **SCEP URI.**

Next, go to **Home** > **Devices** > **Windows** > **Configuration profiles > Create** > **New Policy** with the following parameters:&#x20;

* Platform = Windows 10 and later
* Profile type = Template
* Template name = SCEP certificate

Next, configure the template according to the screenshot below making sure you attached the **Contoso Root Certificate** created earlier in step 1 and the **SCEP URI** you took a copy of above.

<figure><img src="../../../../.gitbook/assets/image (347).png" alt=""><figcaption><p>SCEP Device Certificate Configuration</p></figcaption></figure>

This must be repeated for every device platform that shall be using the service (e.g. Windows, macOS, ...)

### Wi-Fi profile <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

Deploy the WiFi adapter settings to your devices by following this article:

{% content-ref url="../../../profile-deployment/microsoft-intune/wifi-profile/" %}
[wifi-profile](../../../profile-deployment/microsoft-intune/wifi-profile/)
{% endcontent-ref %}
