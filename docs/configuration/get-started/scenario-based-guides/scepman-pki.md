---
description: >-
  This article describes the configuration steps necessary to implement
  certificate-based WiFi authentication using SCEPman with Intune. For this
  demonstration, we will use a MikroTik access point.
---

# SCEPman PKI

## Step 1: Deploy SCEPman Enterprise

{% hint style="warning" %}
Please note that this scenario requires SCEPman Enterprise Edition.
{% endhint %}

First and foremost, you will need to set up and configure your SCEPman PKI. Please use [documentation](https://docs.scepman.com/scepman-deployment/deployment-guides) relevant to your environment to perform the installation and configuration of SCEPman. Once completed, return to this article.

## Step 2: Establish trust between RADIUSaaS and SCEPman <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

For RADIUSaaS to trust client authentication certificates issued by your SCEPman PKI, you must add SCEPman's root CA certificate to the RADIUSaaS trust store. Therefore, follow [these steps](../../../admin-portal/settings/trusted-roots.md#add).

## Step 3: Configure the RADIUS Server Certificate

In this example, we will use a RADIUS Server Certificate issed by SCEPman. Therefore, follow below steps:

1. Generate and upload your SCEPman-issued RADIUS Server Certificate as described [here](../../../admin-portal/settings/settings-server.md#scepman-issued-server-certificate).
2. Activate the SCEPman-issued RADIUS Server Certificate as described [here](../../../admin-portal/settings/settings-server.md#certificate-activation).

Once completed, you **Server Certificate** settings should look like this:

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

## Step 4: Configure your networking equipment

To configure your networking equipment (WiFi access points, switches, or VPN gateways), follow [these steps](../generic-guide.md#step-4-network-equipment-configuration).

After successful completion of Steps 2 - 4, the **Trusted Certificates** page of your RADIUSaaS instance will look similar to the one below. Please note that in our example we have used a RadSec-enabled [MikroTik](../../access-point-setup/radsec-available/mikrotik.md) access point that leverages a SCEPman-issued RadSec Client Certificate.

<figure><img src="../../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

## Step 5: Configure Intune Profiles

To set up certificate-based WiFi authentication, you will need to create and deploy a number of policies via Intune. These policies are as follow:

| Profile Type        | Purpose                                                                                                                                                             |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Trusted certificate | <p>Deploy the Root CA certificate that has issued the RADIUS Server Certificate. <br><br>In this scenario, the relevant CA corresponds to the SCEPman Root CA. </p> |
| SCEP certificate    | Deploy the client authentication certificate.                                                                                                                       |
| Wi-Fi               | Deploy the wireless network adapter settings.                                                                                                                       |

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Relevant Intune Policies</p></figcaption></figure>

### Trusted certificate profiles

You should have already configured the relevant profile as part of the [SCEPman setup](https://docs.scepman.com/certificate-deployment/microsoft-intune).

### SCEP certificate profile

This profile should have been configured as part of the [SCEPman setup](https://docs.scepman.com/certificate-deployment/microsoft-intune), too.

### Wi-Fi profile <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

Deploy the WiFi adapter settings to your devices by following this article:

{% content-ref url="../../../profile-deployment/microsoft-intune/wifi-profile/" %}
[wifi-profile](../../../profile-deployment/microsoft-intune/wifi-profile/)
{% endcontent-ref %}
