# Android

To use this configuration your RADIUS Server need a certificate which was issued by a CA and you have to reference the CA certificate in your WiFi profile. If you are not sure what type of certificate your server is using, contact us.&#x20;

Before creating the **Wi-Fi** profile, create a **Trusted root certificate** profile as described [**here**](../trusted-root.md#to-add-a-trusted-root-profile-for-your-clients). Change your **Platform** accordingly.

The following list and screenshot show you all necessary configurations:

1. Log in to your [Azure portal](https://portal.azure.com)​
2. Navigate to **Microsoft Intune(Endpoint Manager)** -> **Devices** -> **Android** -> **Configuration profiles**
3. Then click **Create Profile**
4. As **Platform** select your Android device type
5. As **Profile type** select **Wi-Fi**
6. Then as **Wi-Fi type** choose **Enterprise**
7. As **EAP type** choose **EAP - TLS**
8. Next, as **Certificate server names** add the DNS name from your [**Server Certificate**](../../portal/settings-server.md#server-certificate)****
9. Select the created **Trusted root certificate** profile
10. Finally under **Client Authentication** select your certificate profile



![](../../.gitbook/assets/ksnip\_20220106-104756.png)
