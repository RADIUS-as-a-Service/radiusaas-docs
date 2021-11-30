# Windows

No matter which certificate type you are deploying, make sure to import your Server Certificate as trusted root as described [here](../trusted-root.md#to-add-a-trusted-root-profile-for-your-clients).

## Certificate Type

For Windows Devices there are two options to create the WiFi Policy and it depends on User or Device certificates which you are deploying which needs to be used.

If User certificates where deployed, follow the steps under [User Certificates](windows.md#user-certificates). With [Device Certificates](windows.md#device-certificates), use the WiFi XML which you get in your [Portal](../../portal/settings-trusted-roots/xml.md#wifi).

### Device Certificates&#x20;

The following list and screenshot show you all necessary configurations:

1. Log in to your [Azure portal](https://porta.azure.com)
2. Navigate to **Microsoft Intune(Endpoint Manager)** -> **Devices** -> **Windows**  -> **Configuration profiles**
3. Then click **Create Profile**
4. As **Platform** select **Windows 8.1 and later**
5. As **Profile type** select **Wi-Fi import**
6. Finally enter a **Connection name** and upload the Wi-Fi XML which matches the value of the first **\<name>** tag of your XML

![](<../../.gitbook/assets/image (38).png>)

### User Certificates

The following lists and screenshot show you all necessary configurations:

1. Navigate to **Microsoft Intune(Endpoint Manager)** -> **Devices** -> **Windows** -> **Configuration profiles**
2. Then click **Create Profile**
3. As **Platform** select **Windows 10 and later**
4. As **Profile type** select **Wi-Fi**
5. As **Wi-Fi type** select **Enterprise**
6. Then for **EAP type** choose **EAP - TLS**
7. Next, as **Certificate server names** add the DNS name from your [**Server Certificate**](../../portal/settings-server.md#server-certificate)
8. Select the created RADIUS certificates in **Root certificates for server validation**
9. Finally as **Client Authentication** select your SCEPman user certificate profile

All other settings can be configured according to your own needs and preferences.

![](https://gblobscdn.gitbook.com/assets%2F-Lzl3JXanfpvdg6pLlGg%2F-MOVUukmvRB1ro-KX5on%2F-MOVVQtXF-vADh51U\_Bs%2Frj-wifi\_user-profile.png?alt=media\&token=9c40b778-f822-4b68-ac23-8a03f9c03175)
