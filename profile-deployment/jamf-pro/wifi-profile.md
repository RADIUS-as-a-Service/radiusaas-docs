# WiFi Profile

{% hint style="warning" %}
We strongly recommend to **not use any spaces** or **special characters** in your **SSID**.
{% endhint %}

To configure a WiFi profile in Jamf, please follow these instructions:

* Log in to your Jamf Pro instance
* Choose your correct deployment scope (Computers, Devices or Users). In this example, we have chosen a macOS device (Computers).
* Navigate to the existing Configuration Profile from the previous step ([Trusted Root](server-trust.md))
* Add a **Network** payload
* As **Network Interface** select **Wi-Fi**
* Provide your **Service Set Identifier (SSID)**
* As **Security Type** select **Any (Enterprise)**

<figure><img src="../../.gitbook/assets/image (413).png" alt=""><figcaption></figcaption></figure>

* Under **Network Security Settings** and **Protocols** select **TLS**

<figure><img src="../../.gitbook/assets/image (410).png" alt=""><figcaption></figcaption></figure>

* Navigate to **Network Security Settings** **>** **Trust**
* As **Identity Certificate** select the client authentication certificate you have configured in your SCEP payload of this Configuration Profile before. In case you are using SCEPman as CA, please select the SCEP Proxy you have previously set up during the [configuration](https://docs.scepman.com/certificate-deployment/jamf/general) of SCEPman.
* Under **Trusted Certificates** all certificates you have configured as **Certificate** payloads, will appear here. Check them all.
*   Next, under **Trusted Server Certificate Names** add the&#x20;

    * **Subject Alernative Name (SAN)**
    * and **Common Name (CN)**&#x20;

    of your _active_ RADIUS [**Server Certificate.**](../../admin-portal/settings/settings-server.md#server-certificates) Those properties can be found by expanding the active server certificate and copying the relevant values. **Please consider, that the common name is case-sensitive.**

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

* Unselect **Allow Trust Exceptions**
* Configure all other options as per your requirements.
* Click **Save**
* Under **Scope** assign the profile to the relevant audience

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>
