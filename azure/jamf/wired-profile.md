# Wired Profile

To configure a Wired network profile in Jamf, please follow these instructions:gure all other options as per your requirements.

* Log in to your Jamf Pro instance
* Choose your correct deployment scope (Computers, Devices or Users). In this example, we have chosen a macOS device (Computers).
* Navigate to the existing Configuration Profile from the previous step ([Trusted Root](trusted-root.md)).
* As "**Network Interface**" select the relevant ethernet, e.g. "**First Active Ethernet**"
* Under "**Network Security Settings**" and "**Protocols**" select "**TLS**"

<figure><img src="../../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>

* Navigate to "**Network Security Settings**" --> "**Trust**"
* As "**Identity Certificate**" select the client authentication certificate you have configured in your SCEP payload of this Configuration Profile before. In case you are using SCEPman as CA, please select the SCEP Proxy you have previously set up during the [configuration](https://docs.scepman.com/certificate-deployment/jamf/general) of SCEPman.
* Under "**Trusted Certificates**" all certificates you have configured as "**Certificate**" payloads, will appear here. Check them all.
* Under "**Trusted Server Certificate Names**" click "**Add**" and add the DNS name from your _active_ RADIUS [**Server Certificate**](../../portal/settings/settings-server/certificates.md). This can be found by expanding the active Server Certificate and copying the **SAN** value. Additionally, add a second entry for the common name (CN) of the _active_ RADIUS Server Certificate.

<figure><img src="../../.gitbook/assets/image (3) (1) (2).png" alt=""><figcaption></figcaption></figure>

* Unselect "**Allow Trust Exceptions**"
* Configure all other options as per your requirements.
* Click "**Save**"
* Under "**Scope**" assign the profile to the relevant audience

<figure><img src="../../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>
