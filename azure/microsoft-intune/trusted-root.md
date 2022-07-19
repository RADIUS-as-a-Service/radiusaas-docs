# Trusted Root

### Part 1: Use only the root certificate

{% hint style="info" %}
Only relevant if you are bringing your own RADIUS server certificate or are using the Custom CA.
{% endhint %}

When downloading the Server certificate, use only the green marked certificate

![](<../../.gitbook/assets/image (78) (1).png>)

### Part 2: Adding a Trusted Certificate Profile for your Clients&#x20;

{% hint style="warning" %}
Ensure, you have reviewed [Part 1](trusted-root.md#edit-your-downloaded-certificate).
{% endhint %}

1. Log in to your [Azure portal](https://portal.azure.com/)
2. Navigate to **Microsoft Intune** and click **Device** and subsequently **Configuration profiles**
3. Then click **Create profile**
4. Select the correct **Platform** for your device
5. Search the **Profile type** templates for **Trusted certificate** and select it
6. Click **Create** and provide a descriptive name and optional **Description**
7. In the second step, upload the \*.cer file containing the RADIUS server certificate/trusted root the server certificate was signed with.

![](<../../.gitbook/assets/image (45).png>)

