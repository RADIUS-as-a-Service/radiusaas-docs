# Server Trust

{% hint style="info" %}
This is only required if you are using a RADIUS server certificate that was issued by a CA that your clients do not already trust. For example, this is not needed if you are bringing your own server certificate issued by SCEPman.
{% endhint %}

### Part 1: Download the RADIUS Server Certificate

When [downloading](../../admin-portal/settings/settings-server.md#download) the Server certificate, use only the green-marked certificate. This will download the root CA certificate of the issuing CA.

<figure><img src="../../.gitbook/assets/image (395).png" alt=""><figcaption><p>Showing download of root CA</p></figcaption></figure>

### Part 2: Trust the Server Certificate

1. Log in to your Jamf Pro instance.
2. Choose your correct deployment scope (Computers, Devices or Users). In this example, we have chosen a macOS device (Computers).
3. Navigate to an existing Configuration Profile or create a new one and give the profile a meaningful name, e.g. "Corp-WiFi-802.1X".
4. Add a **Certificate** payload
5. Provide a meaningful **Certificate Name**, e.g. "RADIUSaaS Server Root CA"
6. Under **Select Certificate Option** select **Upload**
7. Upload the \*.cer file containing the certificate you have downloaded in Part 1. A password is not required, since it is only the public key part that is contained in the file and that needs to be uploaded.
8. Select **Allow all apps access**
9. Unselect **Allow export from keychain**
10. Click **Save**
11. Under **Scope** assign the profile to the relevant audience

<figure><img src="../../.gitbook/assets/image (425).png" alt=""><figcaption></figcaption></figure>
