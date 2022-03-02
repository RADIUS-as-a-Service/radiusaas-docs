# Trusted Root

### Edit your downloaded certificate

If you've uploaded your own certificate or created your [own CA](../portal/settings-server.md#server-certificates) you will properly have some intermediates or the root certificate in the chain of certificates which will be [downloaded](../portal/settings-server.md#download). Intune will only use the user certificate if the whole file was uploaded. So you will have to edit the file, that the actual root certificate will be deployed to your clients. Open the downloaded file and remove the content which is marked in blue(in this sample).&#x20;

![](<../.gitbook/assets/image (55).png>)

### Adding a trusted root profile for your clients&#x20;

1. Log in to your [Azure portal](https://portal.azure.com)
2. Navigate to **Microsoft Intune** and click **Device configuration**
3. Then click **Create Profile**
4. Select the correct **Platform** for your device
5. As **Profile type** select **Trusted certificate**
6. Upload the Server [certificate from your portal](../portal/settings-server.md#download)

![](<../.gitbook/assets/image (45).png>)

