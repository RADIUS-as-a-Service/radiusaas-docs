---
description: >-
  This guide is applicable for both scenarios: using user- or device-type
  certificates for WiFi authentication.
---

# iOS/iPadOS & macOS

## Configuration Steps

1. Log in to your [Azure portal](https://portal.azure.com)
2. Navigate to **Microsoft Intune** and click **Device** and subsequently **Configuration profiles**
3. Then click **Create profile**
4. As **Platform** select **iOS/iPadOS** or **macOS**
5. Search the **Profile type** templates for **Wi-Fi** and select it
6. As **Profile type** select **Wi-Fi**
7. Click **Create** and provide a descriptive name and optional **Description**
8. As **Wi-Fi type** select **Enterprise**
9. Enter your **SSID**. The **Network name** can assume the same name.
10. Select the applicable **Security type** (iOS/iPadOS only)
11. Then for **EAP type** choose **EAP - TLS**
12. Next, as **Certificate server names** add the DNS name from your _active_ RADIUS [**Server Certificate**](../../portal/settings/settings-server/certificates.md). This can be found by expanding the active Server Certificate and copying the **SAN** value. \
    ![](<../../.gitbook/assets/image (73).png>)
13. For the **Root certificates for server validation** select the Trusted certificate profile you have previously created for the RADIUS Server Certificate.
14. Under **Client Authentication** select **Certificates** as **Authentication method**&#x20;
15. Finally, under **Certificates** select the SCEP profile you would like to use for authentication.

All other settings can be configured according to your own needs and preferences.

![](<../../.gitbook/assets/image (77) (1).png>)
