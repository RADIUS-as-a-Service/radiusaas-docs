---
description: >-
  This guide is applicable for both scenarios: using user- or device-type
  certificates for WiFi authentication.
---

# Windows

## Configuration Steps

1. Log in to your [Azure portal](https://portal.azure.com)
2. Navigate to **Microsoft Intune** and click **Device** and subsequently **Configuration profiles**
3. Then click **Create profile**
4. As **Platform** select **Windows 10 and later**
5. Search the **Profile type** templates for **Wi-Fi** and select it
6. Click **Create** and provide a descriptive name and optional **Description**
7. As **Wi-Fi type** select **Enterprise**
8. Enter your **SSID**. The **Connection Name** can assume the same name.
9. Then for **EAP type** choose **EAP - TLS**
10. Next, as **Certificate server names** add the DNS name from your [**Server Certificate**](../../portal/settings-server/certificates.md). This can be found by expanding the active Server Certificate and copying the **SAN** value.&#x20;
11. For the **Root certificates for server validation** select the Trusted certificate profile you have previously created for the RADIUS Server Certificate.
12. Under **Client Authentication** select **SCEP certificate** as **Authentication method**&#x20;
13. Finally, **Client certificate for client authentication (Identity certificate)** select the SCEP profile you would like to use for authentication.

All other settings can be configured according to your own needs and preferences.

![](<../../.gitbook/assets/Screenshot 2022-04-04 at 16.28.13 (1).png>)
