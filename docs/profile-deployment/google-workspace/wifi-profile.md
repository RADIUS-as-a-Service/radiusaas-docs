# WiFi Profile

{% hint style="warning" %}
We strongly recommend to **not use any spaces** or **special characters** in your **SSID**.
{% endhint %}

To configure a WiFi profile in Google Workspaces for your Chromebooks, please follow these instructions:

1. In your Google **Admin console** (at admin.google.com)  > Go to **Menu** > **Devices** > **Network** > **Create a Wi-Fi network**.
2. In the **Platform access** section, check the box for **Enabled for Chromebooks (by device)**.
3. In the **Details** section, set the following:

| Details                                    | Value                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Name**                                   | Your WiFi name (we recommend to match it with the SSID).                                                                                                                                                                                                                                                                                                                                                                 |
| **SSID**                                   | Your SSID.                                                                                                                                                                                                                                                                                                                                                                                                               |
| **Automatically connect**                  | Enable the checkbox.                                                                                                                                                                                                                                                                                                                                                                                                     |
| **Security Type**                          | **WPA/WPA2/WPA3 Enterprise (802.1X)**                                                                                                                                                                                                                                                                                                                                                                                    |
| **Extensible Authentication Protocol**     | **EAP-TLS**                                                                                                                                                                                                                                                                                                                                                                                                              |
| **Maximum TLS Version**                    | **1.2**                                                                                                                                                                                                                                                                                                                                                                                                                  |
| **Username**                               | Anonymous                                                                                                                                                                                                                                                                                                                                                                                                                |
| **Server Certificate Authority**           | Reference the certificate profile containing your [RADIUS server certificate root CA](server-trust.md).                                                                                                                                                                                                                                                                                                                  |
| **Server Certificate Domain Suffix Match** | <p>Add the</p><ul><li><strong>Subject Alternative Name (SAN)</strong></li><li>and <strong>Common Name (CN)</strong></li></ul><p>of your <em>active</em> RADIUS <a href="../../admin-portal/settings/settings-server.md#server-certificates"><strong>Server Certificate.</strong></a> These properties can be found by expanding the active server certificate and copying the relevant values. See screenshot below.</p> |
| **SCEP profile**                           | Reference your [SCEP certificate profile](https://docs.scepman.com/certificate-deployment/static-certificates/google-workspace/chromeos#add-a-scep-profile).                                                                                                                                                                                                                                                             |

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption><p>Showing <strong>Subject Alternative Name (SAN)</strong> and <strong>Common Name (CN)</strong> </p></figcaption></figure>

4. Click **Save**.

<figure><img src="../../.gitbook/assets/image (12) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (14) (1).png" alt=""><figcaption></figcaption></figure>
