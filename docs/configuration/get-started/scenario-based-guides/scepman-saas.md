---
description: >-
  This article describes the configuration steps necessary to implement
  certificate-based WiFi authentication using SCEPman SaaS with Intune.
---

# SCEPman SaaS

{% stepper %}
{% step %}
### Configure SCEPman SaaS

First and foremost, you will need to set up and configure your SCEPman PKI. Please refer to this guide to complete this step. Once completed, return to this article.

{% content-ref url="../../scepman-saas/" %}
[scepman-saas](../../scepman-saas/)
{% endcontent-ref %}
{% endstep %}

{% step %}
### Establish Trust between RADIUSaaS and SCEPman

For RADIUSaaS to trust client authentication certificates issued by SCEPman PKI, you must add SCEPman's root CA certificate to the RADIUSaaS trust store following these steps:

{% embed url="https://docs.radiusaas.com/admin-portal/settings/trusted-roots#add" %}
{% endstep %}

{% step %}
### Configure the RADIUS Server Certificate

Leverage SCEPman to fully automate the lifecycle of your RADIUSaaS Server Certificate by setting up **SCEPman Connection** as belo&#x77;**:**

Navigate to **SCEPman Connection** section under **Settings** > **Server Settings** and click on **Setup Connection**.

<figure><img src="../../../.gitbook/assets/image (515).png" alt=""><figcaption></figcaption></figure>

Then you will get

<figure><img src="../../../.gitbook/assets/image (517).png" alt=""><figcaption></figcaption></figure>

Once you click Yes, RADIUSaaS will request a server certificate from SCEPman, activate it, and renew it in time before it expires. Everything happens automatically, without any interaction required from your side.
{% endstep %}

{% step %}
### Configure your Networking Equipment

To configure your networking equipment (WiFi access points, switches, or VPN gateways), follow these steps:

{% embed url="https://docs.radiusaas.com/configuration/get-started/generic-guide#network-equipment-configuration" %}

After completing the previous steps, the **Trusted Certificates** page of your RADIUSaaS instance will look similar to the one shown below. Please note that in our example we have used a RadSec-enabled [MikroTik](../../access-point-setup/radsec-available/mikrotik.md) access point that leverages a SCEPman-issued RadSec **Client Certificate**.

<figure><img src="../../../.gitbook/assets/image (19) (1).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Configure Intune Profiles

To set up certificate-based Wi-Fi authentication, you will need to create and deploy several policies in Intune. These policies are as follows:

<table><thead><tr><th width="371">Profile Type</th><th>Purpose</th></tr></thead><tbody><tr><td>Trusted certificate</td><td>Deploy the Root CA certificate that has issued the RADIUS Server Certificate. <br><br>In this scenario, the relevant CA corresponds to the SCEPman Root CA. </td></tr><tr><td>SCEP certificate</td><td>Deploy the client authentication certificate.</td></tr><tr><td>Wi-Fi</td><td>Deploy the wireless network adapter settings.</td></tr></tbody></table>

<figure><img src="../../../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption><p>Relevant Intune Policies</p></figcaption></figure>

### Trusted Certificate Profiles

This profile was configured as part of the [SCEPman setup](https://docs.scepman.com/certificate-deployment/microsoft-intune).&#x20;

### SCEP Certificate Profile

This profile was configured as part of the [SCEPman setup](https://docs.scepman.com/certificate-deployment/microsoft-intune).

### WiFi Profile <a href="#step-1-create-root-ca-in-admin-center" id="step-1-create-root-ca-in-admin-center"></a>

Deploy the Wi-Fi adapter settings to your devices by following this article:&#x20;

{% content-ref url="../../../profile-deployment/microsoft-intune/wifi-profile/" %}
[wifi-profile](../../../profile-deployment/microsoft-intune/wifi-profile/)
{% endcontent-ref %}
{% endstep %}

{% step %}
### Permissions and Technical Contacts

{% include "../../../.gitbook/includes/permissions-and-notificatio....md" %}
{% endstep %}

{% step %}
### Rules

This is an **optional** step.

If you would like to configure additional rules, for example to assign VLAN IDs or limit authentication requests to certain trusted CAs or WiFi access points, please check out the RADIUSaaS Rule Engine.

{% content-ref url="../../../admin-portal/settings/rules/" %}
[rules](../../../admin-portal/settings/rules/)
{% endcontent-ref %}
{% endstep %}
{% endstepper %}

