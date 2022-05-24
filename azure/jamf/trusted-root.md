# Trusted Root

### Part 1: Edit your downloaded Server Certificate

{% hint style="info" %}
Only relevant if you are bringing your own RADIUS server certificate or are using the Custom CA.
{% endhint %}

If you've uploaded your [**own certificate**](../../portal/settings/settings-server/certificates.md#bring-your-own-certificate) or created your [**Custom CA**](../../portal/settings/settings-server/certificates.md#custom-cas) as **Server Certificate**, you will under most circumstances see the entire certificate chain as part of the [downloaded](../../portal/settings/settings-server/certificates.md#download) Server Certificate file.&#x20;

If you upload this to Jamf, only the hierarchically last certificate will be pushed to your client, which is typically not the root certificate we intend to push to the client. To circumvent this, please open the downloaded file (it is a PEM-encoded \*.cer file) with a standard text editor and remove all certificates except for the root certificate. If you have leveraged the Custom CA, please remove the blue part as shown in the sample below.

![](<../../.gitbook/assets/image (55).png>)

### Part 2: Adding a Trusted Certificate Profile for your Clients&#x20;

{% hint style="warning" %}
Ensure, you have reviewed [Part 1](trusted-root.md#part-1-edit-your-downloaded-server-certificate).
{% endhint %}

1. Log in to your Jamf Pro instance.
2. Choose your correct deployment scope (Computers, Devices or Users). In this example, we have chosen a macOS device (Computers).
3. Add a new "**macOS Configuration Profile**" and choose "**Certificate**" as payload
4. Assign a meaningful configuration profile name, e.g. "RADIUSaaS Server Trust"
5. Provide a meaningful **Certificate Name**, e.g. "RADIUSaaS Server Root CA"
6. Upload the \*.cer file containing the RADIUS server certificate/trusted root the server certificate was signed with. A password is not required, since it is only the public key part that needs to be uploaded.
7. Select "**Allow all apps access**"
8. Click "**Save**"
9. Under "**Scope**" assign the profile to the relevant audience

![](<../../.gitbook/assets/image (63).png>)
