# Mac

#### Before creating the **Wi-Fi** profile, create a **Trusted root certificate** profile as described [**here**](../trusted-root.md#to-add-a-trusted-root-profile-for-your-clients). Change your **Platform** accordingly. <a href="#before-creating-the-wi-fi-profile-create-a-trusted-root-certificate-profile-as-described-here-change" id="before-creating-the-wi-fi-profile-create-a-trusted-root-certificate-profile-as-described-here-change"></a>

The following list and screenshot show you all necessary configurations:

1. Log in to your [Azure portal​](https://portal.azure.com)
2. Navigate to **Microsoft Intune(Endpoint Manager)** -> **Devices** -> **macOS** -> **Configuration profiles**
3. Then click **Create Profile**
4. As **Profile type** select **Wired Network**
5. Then set your Profile **Name**
6. Choose your **Network Interface**
7. As **EAP type** choose **EAP - TLS**
8. Next, as **Certificate server names** add the DNS name from your [**Server Certificate**](../../portal/settings-server.md#server-certificate)
9. Select the created **Trusted root certificate** profile
10. Finally under **Client Authentication** select your certificate profile

![](https://gblobscdn.gitbook.com/assets%2F-Lzl3JXanfpvdg6pLlGg%2F-MQQli8\_2rOzfole8NzB%2F-MQQyDJPOyNXpyQA0z0L%2Fimage.png?alt=media\&token=49a00314-2915-496f-a7ce-2bb85e319700)
