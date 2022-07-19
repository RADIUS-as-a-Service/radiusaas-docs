# macOS

## Configuration Steps <a href="#before-creating-the-wi-fi-profile-create-a-trusted-root-certificate-profile-as-described-here-change" id="before-creating-the-wi-fi-profile-create-a-trusted-root-certificate-profile-as-described-here-change"></a>

1. Log in to your [Azure portal](https://portal.azure.com/)
2. Navigate to **Microsoft Intune** and click **Device** and subsequently **Configuration profiles**
3. Then click **Create profile**
4. As **Platform** select **macOS**
5. Search the **Profile type** templates for **Wired network** and select it
6. Click **Create** and provide a descriptive name and optional **Description**
7. Choose your **Network Interface**
8. As **EAP type** choose **EAP - TLS**
9. Next, as **Certificate server names** add the DNS name from your _active_ RADIUS [**Server Certificate**](../../../portal/settings/settings-server/certificates.md). This can be found by expanding the active Server Certificate and copying the **SAN** value. \
   ![](<../../../.gitbook/assets/image (77) (1) (1) (1).png>)
10. For the **Root certificates for server validation** select the Trusted certificate profile you have previously created for the RADIUS Server Certificate.
11. Finally, under **Client Authentication** select **Certificates** as **Authentication method**&#x20;

![](<../../../.gitbook/assets/image (62) (1) (1).png>)
