# Wired Profile

To configure a Wired network profile in Jamf, please follow these instructions: configure all other options as per your requirements.

1. Log in to your Jamf Pro instance
2. Choose your correct deployment scope (Computers, Devices or Users). In this example, we have chosen a macOS device (Computers).
3. Navigate to the existing Configuration Profile from the previous step ([Trusted Root](server-trust.md)).
4. As **Network Interface** select the relevant ethernet, e.g. **First Active Ethernet**
5. Under **Network Security Settings** and **Protocols** select **TLS**

<figure><img src="../../../.gitbook/assets/image (414).png" alt=""><figcaption></figcaption></figure>

6. Navigate to **Network Security Settings** **> Trust**
7. As **Identity Certificate** select the client authentication certificate you have configured in your SCEP payload of this Configuration Profile before. In case you are using SCEPman as CA, please select the SCEP Proxy you have previously set up during the [configuration](https://docs.scepman.com/certificate-deployment/jamf/general) of SCEPman.
8. Under **Trusted Certificates** all certificates you have configured as **Certificate** payloads, will appear here. Check them all.
9. Next, under **Trusted Server Certificate Names** add the&#x20;
   * **Subject Alternative Name (SAN)**
   * and **Common Name (CN)** \
     \
     of your _active_ RADIUS [**Server Certificate**](../../admin-portal/settings/settings-server.md#server-certificates). Those properties can be found by expanding the active server certificate and copying the relevant values. **Please consider, that the common name is case-sensitive.**

<figure><img src="../../../.gitbook/assets/image (434).png" alt=""><figcaption></figcaption></figure>

10. Unselect **Allow Trust Exceptions**
11. Configure all other options as per your requirements.
12. Click **Save**
13. Under **Scope** assign the profile to the relevant audience

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
