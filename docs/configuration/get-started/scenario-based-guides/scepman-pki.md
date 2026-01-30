---
description: >-
  This article describes the configuration steps necessary to implement
  certificate-based WiFi authentication using SCEPman with Intune. For this
  demonstration, we will use a MikroTik access point.
---

# SCEPman PKI

{% stepper %}
{% step %}
### Deploy SCEPman Enterprise

{% hint style="warning" %}
Please note that this scenario requires Certificate Master, available with [SCEPman Enterprise Edition.](https://docs.scepman.com/editions#edition-comparison)
{% endhint %}

First and foremost, you will need to set up and configure your SCEPman PKI. Please use [documentation](https://docs.scepman.com/scepman-deployment/deployment-guides) relevant to your environment to perform the installation and configuration of SCEPman. Once completed, return to this article.
{% endstep %}

{% step %}
### Establish trust between RADIUSaaS and SCEPman

For RADIUSaaS to trust client authentication certificates issued by SCEPman PKI, you must add SCEPman's root CA certificate to the RADIUSaaS trust store following [these steps](../../../admin-portal/settings/trusted-roots.md#add).
{% endstep %}

{% step %}
### Configure the RADIUS Server Certificate

{% tabs %}
{% tab title="Using SCEPman Connection (Recommended)" %}
{% embed url="https://docs.radiusaas.com/admin-portal/settings/settings-server#scepman-connection" %}
{% endtab %}

{% tab title="Using Certificate Master" %}
In this example, we will use a RADIUS Server Certificate issued by SCEPman. Therefore, follow below steps:

1. Generate and upload your SCEPman-issued RADIUS **Server Certificate** as described [here](../../../admin-portal/settings/settings-server.md#scepman-issued-server-certificate).
2. Activate the SCEPman-issued RADIUS Server Certificate as described [here](../../../admin-portal/settings/settings-server.md#certificate-activation).

Once completed, your **Server Certificate** settings should look like this:

<figure><img src="../../../.gitbook/assets/image (489).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
### Configure your networking equipment

To configure your networking equipment (Wi-Fi access points, switches, or VPN gateways), follow [these steps](../generic-guide.md#step-4-network-equipment-configuration).

After successful completion of Steps 2 - 4, the **Trusted Certificates** page of your RADIUSaaS instance will look similar to the one below. Please note that in our example we have used a RadSec-enabled [MikroTik](../../access-point-setup/radsec-available/mikrotik.md) access point that leverages a SCEPman-issued RadSec **Client Certificate**.

<figure><img src="../../../.gitbook/assets/image (19) (1).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Configure Intune Profiles

To set up certificate-based Wi-Fi authentication, you will need to create and deploy a number of policies via Intune. These policies are as follow:

<table><thead><tr><th width="371">Profile Type</th><th>Purpose</th></tr></thead><tbody><tr><td>Trusted certificate</td><td>Deploy the Root CA certificate that has issued the RADIUS Server Certificate. <br><br>In this scenario, the relevant CA corresponds to the SCEPman Root CA. </td></tr><tr><td>SCEP certificate</td><td>Deploy the client authentication certificate.</td></tr><tr><td>Wi-Fi</td><td>Deploy the wireless network adapter settings.</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption><p>Relevant Intune Policies</p></figcaption></figure>

### Trusted certificate profiles

This profile was configured as part of the [SCEPman setup](https://docs.scepman.com/certificate-deployment/microsoft-intune).&#x20;

### SCEP certificate profile

This profile was configured as part of the [SCEPman setup](https://docs.scepman.com/certificate-deployment/microsoft-intune).

### Wi-Fi profile <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

Deploy the Wi-Fi adapter settings to your devices by following this article:&#x20;

{% content-ref url="../../../profile-deployment/microsoft-intune/wifi-profile/" %}
[wifi-profile](../../../profile-deployment/microsoft-intune/wifi-profile/)
{% endcontent-ref %}
{% endstep %}
{% endstepper %}
