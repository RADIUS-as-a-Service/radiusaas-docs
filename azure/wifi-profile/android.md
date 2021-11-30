# Android

{% hint style="warning" %}
Since a recent update of Android 11, the WiFi profile settings in Intune are functionally broken.
{% endhint %}

If your clients use Android 10 or lower, you can use the following configuration:&#x20;

{% hint style="info" %}
Android Version 10 and higher do not allow to add a user certificates - which your radius server certificate is - as Certificate Authority. Therefore let the section **Root certificate for server validation** untouched.
{% endhint %}

The following list and screenshot show you all necessary configurations:

1. Log in to your [Azure portal](https://portal.azure.com)​
2. Navigate to **Microsoft Intune(Endpoint Manager)** -> **Devices** -> **Android** -> **Configuration profiles**
3. Then click **Create Profile**
4. As **Platform** select **Android Enterprise**
5. As **Profile type** select **Wi-Fi**
6. Then as **Wi-Fi type** choose **Enterprise**
7. As **EAP type** choose **EAP - TLS**
8. Finally select your certificate profile under **Client Authentication**

![](https://gblobscdn.gitbook.com/assets%2F-Lzl3JXanfpvdg6pLlGg%2F-MRzTCnyTXIc508RLjzv%2F-MRzWMMOFKRnAWGMCG10%2Fandroid-enterprise-eap-tls-settings.png?alt=media\&token=c08d2827-cf3a-42ae-ac45-54764d8d581f)
