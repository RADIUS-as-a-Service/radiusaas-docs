# Server Trust

{% hint style="info" %}
This is only required if you are using a RADIUS server certificate that was issued by a CA that your clients do not already trust. For example, this is not needed if you are bringing your own server certificate issued by SCEPman.
{% endhint %}

### Part 1: Download the RADIUS Server Certificate

When [downloading](../../admin-portal/settings/settings-server.md#download) the Server certificate, use only the green-marked certificate. This will download the root CA certificate of the issuing CA.

<figure><img src="../../../.gitbook/assets/image (367).png" alt=""><figcaption></figcaption></figure>

### Part 2: Adding a Trusted certificate profile for your dndpoint devices&#x20;

{% hint style="warning" %}
Ensure, you have reviewed [Part 1](trusted-root.md#edit-your-downloaded-certificate).
{% endhint %}

1. Log in to [Microsoft Intune](https://intune.microsoft.com/)
2. Navigate to **Devices** and subsequently **Configuration profiles**
3. Then click **Create > New policy**
4. Select the correct **Platform** for your device
5. Search the **Profile type** templates for **Trusted certificate** and select it
6. Click **Create** and provide a descriptive name and optional **Description**
7. In the second step, upload the \*.cer file containing the RADIUS server certificate/trusted root the server certificate was signed with.

![](<../../../.gitbook/assets/image (291).png>)

