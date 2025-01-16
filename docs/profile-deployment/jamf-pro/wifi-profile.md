# WiFi Profile

{% hint style="warning" %}
We strongly recommend to **not use any spaces** or **special characters** in your **SSID**.
{% endhint %}

To configure a WiFi profile in Jamf, please follow these instructions:

1. Log in to your Jamf Pro instance
2. Choose your correct deployment scope (Computers, Devices or Users). In this example, we have chosen a macOS device (Computers).
3. Navigate to the existing Configuration Profile from the previous step ([Trusted Root](server-trust.md))
4. Add a **Network** payload
5. As **Network Interface** select **Wi-Fi**
6. Provide your **Service Set Identifier (SSID)**
7. As **Security Type** select **Any (Enterprise)**

<figure><img src="../../../.gitbook/assets/image (413).png" alt=""><figcaption></figcaption></figure>

8. Under **Network Security Settings** and **Protocols** select **TLS**

<figure><img src="../../../.gitbook/assets/image (410).png" alt=""><figcaption></figcaption></figure>

9. Navigate to **Network Security Settings** **>** **Trust**
10. As **Identity Certificate** select the client authentication certificate you have configured in your SCEP payload of this Configuration Profile before. In case you are using SCEPman as CA, please select the SCEP Proxy you have previously set up during the [configuration](https://docs.scepman.com/certificate-deployment/jamf/general) of SCEPman.
11. Under **Trusted Certificates** all certificates you have configured as **Certificate** payloads, will appear here. Check them all.
12. Next, under **Trusted Server Certificate Names** add the&#x20;
    * **Subject Alternative Name (SAN)**
    * and **Common Name (CN)** \
      \
      of your _active_ RADIUS [**Server Certificate.**](../../admin-portal/settings/settings-server.md#server-certificates) Those properties can be found by expanding the active server certificate and copying the relevant values.&#x20;

{% hint style="warning" %}
Note that the Common Name is case-sensitive.&#x20;

Please make sure there are no spaces or hidden characters at the beginning or end of the Trusted Server Certificate Names input field in the JAMF interface.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

13. Unselect **Allow Trust Exceptions**
14. Configure all other options as per your requirements.
15. Click **Save**
16. Under **Scope** assign the profile to the relevant audience

<figure><img src="../../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>
