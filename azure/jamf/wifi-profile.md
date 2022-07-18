# WiFi Profile

To configure a WiFi profile in Jamf, please follow these instructions:

* Log in to your Jamf Pro instance.
* Choose your correct deployment scope (Computers, Devices or Users). In this example, we have chosen a macOS device (Computers).
* Add a new "**macOS Configuration Profile**" and choose "**Network**" as payload
* Assign a meaningful configuration profile name, e.g. "\<SSID> WiFi RADIUSaaS"
* As "**Network Interface**" select "**Wi-Fi**"
* Provide your "**Service Set Identifier (SSID)**"
* As "**Security Type**" select "**WPA2 Enterprise**"

![](<../../.gitbook/assets/image (80) (1) (1).png>)

* Under "**Network Security Settings**" and "**Protocols**" select "**TLS**"

![](<../../.gitbook/assets/image (79) (1) (1).png>)

* Navigate to "**Network Security Settings**" --> "**Trust**"
* As "**Identity Certificate**" select the client authentication certificate you would like to use for WiFi authentication. In case you are using SCEPman as CA, please select the SCEP Proxy you have previously set up during the [configuration](https://docs.scepman.com/certificate-deployment/jamf/general) of SCEPman.
* Under "**Trusted Server Certificate Names**" click "**Add**" and add the DNS name from your _active_ RADIUS [**Server Certificate**](../../portal/settings/settings-server/certificates.md). This can be found by expanding the active Server Certificate and copying the **SAN** value.&#x20;

![](<../../.gitbook/assets/image (62) (1).png>)

* Configure all other options as per your requirements.
* Click "**Save**"
* Under "**Scope**" assign the profile to the relevant audience
