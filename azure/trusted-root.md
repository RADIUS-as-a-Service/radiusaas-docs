# Trusted Root

### Part 1: Edit your downloaded Server Certificate

{% hint style="info" %}
Only relevant if you are bringing your own RADIUS server certificate or are using the Custom CA.
{% endhint %}

If you've uploaded your [**own certificate**](../portal/settings/settings-server/certificates.md#bring-your-own-certificate) or created your [**Custom CA**](../portal/settings/settings-server/certificates.md#custom-cas) as **Server Certificate**, you will under most circumstances see the entire certificate chain as part of the [downloaded](../portal/settings/settings-server/certificates.md#download) Server Certificate file.&#x20;

If you upload this to Intune, only the hierarchically last certificate will be pushed to your client, which is typically not the root certificate we intend to push to the client. To circumvent this, please open the downloaded file (it is a PEM-encoded \*.cer file) with a standard text editor and remove all certificates except for the root certificate. If you have leveraged the Custom CA, please remove the blue part as shown in the sample below.

![](<../.gitbook/assets/image (55).png>)

### Part 2: Adding a Trusted Certificate Profile for your Clients&#x20;

{% hint style="warning" %}
Ensure, you have reviewed [Part 1](trusted-root.md#edit-your-downloaded-certificate).
{% endhint %}

1. Log in to your [Azure portal](https://portal.azure.com)
2. Navigate to **Microsoft Intune** and click **Device** and subsequently **Configuration profiles**
3. Then click **Create profile**
4. Select the correct **Platform** for your device
5. Search the **Profile type** templates for **Trusted certificate** and select it
6. Click **Create** and provide a descriptive name and optional **Description**
7. In the second step, upload the \*.cer file containing the RADIUS server certificate/trusted root the server certificate was signed with.

![](<../.gitbook/assets/image (45).png>)

