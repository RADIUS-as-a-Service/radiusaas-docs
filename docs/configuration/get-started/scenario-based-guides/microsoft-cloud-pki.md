---
description: >-
  This page describes the configuration steps necessary to implement
  certificate-based Wi-Fi authentication using Cloud PKI and Intune.
---

# Microsoft Cloud PKI

To set up certificate-based Wi-Fi authentication, we need to create a number of profiles and deploy them via Intune. These profiles are:

| Profile Type        | Purpose                                                                      |
| ------------------- | ---------------------------------------------------------------------------- |
| Trusted certificate | Deploy the Root CA certificate                                               |
| Trusted certificate | Deploy the Issuing CA certificate                                            |
| Trusted certificate | Deploy the Root CA certificate that has issued the RADIUS Server Certificate |
| SCEP certificate    | Enroll the client authentication certificate                                 |
| Wi-Fi               | Deploy the wireless netword adapter settings                                 |

<figure><img src="../../../../.gitbook/assets/image (345).png" alt=""><figcaption><p>Relevant Intune Profiles</p></figcaption></figure>

### Step 1: Create a Root CA in Intune admin center <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

Before you can issue certificates to managed devices, you need to create a root CA in your tenant to act as the trust anchor. To create a root CA in Intune admin center, please follow [this ](https://learn.microsoft.com/en-gb/mem/intune/protect/microsoft-cloud-pki-configure-ca)Microsoft guide.&#x20;

{% hint style="info" %}
Please take note of the CRL distribution point as you will need this later in [Step 6](microsoft-cloud-pki.md#step-6.-configuring-your-radiusaas-instance).&#x20;
{% endhint %}

### Step 2: Create an Issuing CA in Intune admin center <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

An issuing CA is required to issue certificates for Intune-managed devices. Cloud PKI automatically provides a SCEP service that acts as a certificate registration authority. It requests certificates from the issuing CA on behalf of Intune-managed devices using a SCEP profile. To create an issuing CA, please follow [this ](https://learn.microsoft.com/en-gb/mem/intune/protect/microsoft-cloud-pki-configure-ca#step-2-create-issuing-ca-in-admin-center)Microsoft guide.&#x20;

{% hint style="info" %}
Please take note of the CRL distribution point as you will need this later in [Step 6](microsoft-cloud-pki.md#step-6.-configuring-your-radiusaas-instance).
{% endhint %}

<figure><img src="../../../../.gitbook/assets/image (346).png" alt=""><figcaption><p>Root and Issuing CAs</p></figcaption></figure>

### Step 3: Create a SCEP device certificate profile in Intune admin center <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

To issue certificates, you must create a trusted certificate profile for your root and issuing CAs. The trusted certificate profile establishes trust with the Cloud PKI certificate registration authority supporting the SCEP protocol. A trusted certificate profile required for each platform (Windows, Android, iOS/iPad, macOS) that's issuing Cloud PKI SCEP certificates.&#x20;

To create a trusted certificate profile in Intune admin center, first take a copy of the SCEP URI from **Home** > **Tenant admin** > **Cloud PKI** > **Contoso Issuing CA** > **Properties** > **SCEP URI.**

Next, go to **Home** > **Devices** > **Windows** > **Configuration profiles > Create** > **New Policy** with the following parameters:&#x20;

* Platform = Windows 10 and later
* Profile type = Template
* Template name = SCEP certificate

Next, configure the template according to the screenshot below making sure you attached the **Contoso Root Certificate** created earlier in step 1 and the **SCEP URI** you took a copy of above.

<figure><img src="../../../../.gitbook/assets/image (347).png" alt=""><figcaption><p>SCEP Device Certificate Configuration</p></figcaption></figure>

### Step 4: Establish Trust between RADIUSaaS and the Microsoft Cloud PKI <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

Configure RADIUSaaS to trust client authentication certificates issued by the Microsoft Cloud PKI. Since the cloud PKI requires a tiered CA structure, you must upload both, Root CA and Issuing CA (i.e the complete chain of trust). To achieve this, please follow below steps:

1. Navigate to [Trusted Certificates](../../../admin-portal/settings/trusted-roots.md).
2. [Upload](../../../admin-portal/settings/trusted-roots.md#add) the Contoso Cloud PKI **Root CA** selecting **Client Authentication** in the upload process.
3. As verification method select **CRL** along with **DER** encoding.
4. Use the copied CRL distribution point URL of the Root CA in the **CRL Distribution Points** URL input field.\
   ![](<../../../../.gitbook/assets/image (348).png>)
5. Upload the Contoso Cloud PKI **Issuing CA** selecting **Client Authentication** in the upload process.
6. Again, select **CRL** as verification method along with **DER** encoding.
7. Use the copied CRL distribution point URL of the Issuing CA in the **CRL Distribution Points** URL input field.\
   ![](<../../../../.gitbook/assets/image (349).png>)

### Step 5: Configure the RADIUS Server Certificate

To establish server trust between your endpoint devices and RADIUSaaS, follow [these instructions](../generic-guide.md#step-3-radius-server-certificate-configuration).

### Step 6: Configure your Networking Equipment

To configure out WiFi access points, follow [these steps](../generic-guide.md#step-4-network-equipment-configuration).

After successful completion of Steps 4 - 6, the **Trusted Certificates** page of your RADIUSaaS instance will look similar to the one below. Please note that in our example we have used a RadSec-enabled [MikroTik](../../access-point-setup/radsec-available/mikrotik.md) access point.

<figure><img src="../../../../.gitbook/assets/image (350).png" alt=""><figcaption><p>Trusted Certificates Overview required for the Microsoft Cloud PKI.</p></figcaption></figure>

### Step 7: Configure Intune Profiles

To push the remaining artifacts to your endpoint devices via Intune, follow below steps:

{% content-ref url="../../../profile-deployment/microsoft-intune/trusted-root.md" %}
[trusted-root.md](../../../profile-deployment/microsoft-intune/trusted-root.md)
{% endcontent-ref %}

{% content-ref url="../../../profile-deployment/microsoft-intune/wifi-profile/" %}
[wifi-profile](../../../profile-deployment/microsoft-intune/wifi-profile/)
{% endcontent-ref %}
