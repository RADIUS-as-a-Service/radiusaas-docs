# macOS

## Configuration steps <a href="#before-creating-the-wi-fi-profile-create-a-trusted-root-certificate-profile-as-described-here-change" id="before-creating-the-wi-fi-profile-create-a-trusted-root-certificate-profile-as-described-here-change"></a>

1. Log in to [Microsoft Intune](https://intune.microsoft.com/)
2. Navigate to **Devices** and subsequently **Configuration profiles**
3. Then click **Create > New policy**
4. As **Platform** select **macOS**
5. Search the **Profile type** templates for **Wired network** and select it
6. Click **Create** and provide a descriptive name and optional **Description**
7. Choose your **Network Interface**
8. As **EAP type** choose **EAP - TLS**
9.  Next, as **Certificate server names** add the&#x20;

    * **Subject Alternative Name (SAN)**
    * and **Common Name (CN)**&#x20;

    of your _active_ RADIUS [**Server Certificate.**](../../../admin-portal/settings/settings-server.md#server-certificates) Those properties can be found by expanding the active server certificate and copying the relevant values. **Please consider, that the common name is case-sensitive.**

<figure><img src="../../../.gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

1. For the **Root certificates for server validation** select the Trusted certificate profile you have previously created for the RADIUS Server Certificate.
2. Finally, under **Client Authentication** select **Certificates as** **Authentication method**&#x20;

<figure><img src="../../../.gitbook/assets/image (1) (2).png" alt=""><figcaption><p>Showing wired network profile for macOS</p></figcaption></figure>
