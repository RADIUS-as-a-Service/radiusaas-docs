# Apple Devices

The following list and screenshot show you all necessary configuration steps:

Before creating the **Wi-Fi** profile, create a **Trusted root certificate** profile as described [**here**](../trusted-root.md#to-add-a-trusted-root-profile-for-your-clients). Change your **Platform** accordingly.

1. Log in to your [Azure portal​](https://portal.azure.com)
2. Navigate to **Microsoft Intune(Endpoint Manager)** -> **Devices** -> **iOS/iPadOS(macOS)** -> **Configuration profiles**
3. Then click **Create Profile**
4. As **Platform** select **iOS/iPadOS(macOS)**
5. As **Profile type** select **Wi-Fi**
6. Then as **Wi-Fi type** choose **Enterprise**
7. As **EAP type** choose **EAP - TLS**
8. Next, as **Certificate server names** add the DNS name from your [**Server Certificate**](../../portal/settings-server.md#server-certificate)****
9. Select the created **Trusted root certificate** profile
10. Finally under **Client Authentication** select your certificate profile

![](https://gblobscdn.gitbook.com/assets%2F-Lzl3JXanfpvdg6pLlGg%2F-MOVVibjdKvffuvYDUQN%2F-MOVWWYeIrF3mkO8l97x%2Frj\_wifi-ios.png?alt=media\&token=a8f9ca64-559f-4fc3-a8c3-8bba9bc4f0b7)
