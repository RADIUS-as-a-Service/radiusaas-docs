# Windows

## Configurations Steps

1. Log in to your [Azure portal](https://portal.azure.com/)
2. Navigate to **Microsoft Intune** and click **Device** and subsequently **Configuration profiles**
3. Then click **Create profile**
4. As **Platform** select **Windows 10 and later**
5. Search the **Profile type** templates for **Wired network** and select it
6. Click **Create** and provide a descriptive name and optional **Description**
7. fill out the **Configuration settings** as it suits your environment
8. Configure the **Authentication Method** to **User** if you want to use user-type certificates for authentication or **Machine** if you would like to use device-type certificates for authentication.
9. Under **802.1X** make sure, that **Do not enforce** is selected. This way your network adapter will continue to work in environments (e.g. home office), where 802.1X is not available.
10. For **EAP Type** choose **EAP-TLS**
11. Next, as **Certificate server names** add the DNS name from your _active_ RADIUS [**Server Certificate**](../../../portal/settings/settings-server/certificates.md). This can be found by expanding the active Server Certificate and copying the **SAN** value. \
    ![](<../../../.gitbook/assets/image (82) (1) (1) (1).png>)
12. For the **Root certificates for server validation** select the Trusted certificate profile you have previously created for the RADIUS Server Certificate.
13. Under **Client Authentication** select **SCEP certificate** as **Authentication method**&#x20;
14. Finally, **Client certificate for client authentication (Identity certificate)** select the SCEP profile you would like to use for authentication.

![](<../../../.gitbook/assets/image (65) (1) (1).png>)

All other settings can be configured according to your own needs and preferences
