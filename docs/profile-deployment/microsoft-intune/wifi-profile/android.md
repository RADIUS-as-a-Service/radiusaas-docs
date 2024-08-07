# Android

To use this configuration your RADIUS Server need a certificate which was issued by a CA and you have to reference the CA certificate in your WiFi profile. If you are not sure what type of certificate your server is using, contact us.&#x20;

Before creating the **Wi-Fi** profile, create a **Trusted root certificate** profile as described [**here**](../trusted-root.md#to-add-a-trusted-root-profile-for-your-clients). Change your **Platform** accordingly.

The following list and screenshot show you all necessary configurations:

1. Log in to [Microsoft Intune](https://intune.microsoft.com/)
2. Navigate to **Devices** and subsequently **Configuration profiles**
3. Then click **Create > New policy**
4. As **Platform** select your Android device type
5. Search the **Profile type** templates for **Wi-Fi** and select it
6. Click **Create** and provide a descriptive name and optional **Description**
7. As **Wi-Fi type** select **Enterprise**
8. Enter your **SSID**
9. As **EAP type** choose **EAP - TLS**
10. Next, as **Radius server name** add the&#x20;

    * **Subject Alernative Name (SAN)**
    * and **Common Name (CN)**&#x20;

    of your _active_ RADIUS [**Server Certificate**](../../../admin-portal/settings/settings-server.md#server-certificates) . Those properties can be found by expanding the active server certificate and copying the relevant values. **Please consider, that the common name is case-sensitive.**

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption><p>Showing certificate details</p></figcaption></figure>

11\. For the **Root certificates for server validation** select the Trusted certificate profile you have previously created for the RADIUS Server Certificate.

12\. under **Client Authentication** select **Certificates** as **Authentication method**&#x20;

13\. Finally, under **Certificates** select the SCEP profile you would like to use for authentication.

All other settings can be configured according to your own needs and preferences.

{% hint style="info" %}
Some Android kiosk devices require a value for **Identity privacy (outer identity)**. Please consider this when you are having issues authenticating against the WiFi network with such devices.
{% endhint %}

![](<../../../.gitbook/assets/2024-06-03\_21h31\_46 (1).png>)

## Common configuration issues

See [Troubleshooting](../../../other/trubleshooting.md#intune-configuration-issues).
