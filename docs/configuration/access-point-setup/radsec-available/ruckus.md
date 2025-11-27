# Ruckus

{% hint style="info" %}
The following guide was created using Ruckus **Virtual SmartZone Essentials** version **6.1.2.0.441**.
{% endhint %}

## Prepare certificates

To establish a valid RadSec connection, your Access Points must trust the **RADIUS Server Certificate** and your RADIUS server must trust your **RadSec Client Certificate**. To achieve this,

1. Download the root certificate of the CA that has issued your active **RADIUS Server Certificate** as described [here](../../../admin-portal/settings/settings-server.md#download).
2. Create a **RadSec Client Certificate** for your Ruckus SmartZone. If you are using **SCEPman Certificate Master**, the process is described [here](https://docs.scepman.com/certificate-deployment/certificate-master/client-certificate-pkcs-12).&#x20;
3. **Split the generated certificate** into the private key (named "priv.key" in this example) and the certificate (named "clientcert.cer"). In case the **RadSec Client Certificate** was downloaded in the **PKCS#12/.pfx** format, this can be easily done, e.g. using OpenSSL:\
   \
   Private Key:\
   `openssl pkcs12 -in <your-radsec-client-cert>.pfx -nocerts -nodes -out priv.key`\
   \
   Certificate without Private Key:\
   `openssl pkcs12 -in <your-radsec-client-cert>.pfx -clcerts -nokeys -out clientcert.cer`&#x20;

{% hint style="warning" %}
Ensure to monitor the expiry of your **RadSec Client Certificate** and renew it in due time to prevent service interruptions.
{% endhint %}

3. Add the root certificate of the CA that has issued the **RadSec Client Certificate** to your RADIUS instance as described [here](../../../admin-portal/settings/trusted-roots.md#add) and select **RadSec** under **Use for**. \
   In case the **RadSec Client Certificate** has been issued by SCEPman and you already trust the SCEPman Root CA for client authentication, simply edit the trusted SCEPman Root CA certificate and select **Both** under **Use for**.&#x20;

## Ruckus SmartZone (SZ) configuration

{% hint style="info" %}
In the following, Ruckus SZ is configured as an **authentication proxy**, i.e. RADIUS authentication requests are routed from the WAPs to the Ruckus SZ and centrally forwarded to RADIUSaaS.
{% endhint %}

For general information on how to import certificates to the Ruckus SmartZone, please refer to their documentation:

{% embed url="https://docs.commscope.com/bundle/sz-700-controlleradminguide-sz300sz144vsz/page/GUID-5EA43C47-3B01-4908-B5B6-0961CF283504.html" %}

1. Import the root certificate of the CA that has issued your **RADIUS Server Certificate** (downloaded during step 1 [here](ruckus.md#prepare-certificates)) by navigating to **Administration >** **System > Certificates > SZ Trusted CA Certificates/Chain (external)**.
2.  Click **Import**, provide a **Name** and optional **Description** for your RADIUS CA certificate and upload the certificate file by clicking **Browse** next to **Root CA Certificate**. In case you are bringing your own **RADIUS Server Certificate** and in case it has been issued by an intermediate CA, please also upload all intermediate certificates under **Intermediate CA Certificates**.<br>

    <figure><img src="../../../.gitbook/assets/Screenshot_2024-08-21_at_12_54_18.jpg" alt="" width="375"><figcaption></figcaption></figure>
3. Next, click **Validate** and **OK**.
4. Import your **RadSec Client Certificate** (obtained from step 3 [here](ruckus.md#prepare-certificates)) by navigating to **Administration >** **System > Certificates > SZ as Client Certificate**.
5.  Click **Import** and provide a **Name** and optional **Description** for your **RadSec Client Certificate**. Then upload the public portion of your **RadSec Client Certificate** by clicking **Browse** next to **Client Certificate**. Finally, upload the private key by clicking **Browse** next to **Private Key**.<br>

    <figure><img src="../../../.gitbook/assets/Screenshot_2024-08-21_at_13_10_30 (1).jpg" alt=""><figcaption></figcaption></figure>
6. For the RADIUS server configuration, navigate to **Security > Authentication > Proxy (SZ Authenticator)**.
7. Click **Create** and provide a **Name**, optional **Friendly Name** and **Description** for the RADIUS profile.
8. Select **RADIUS** as **Service Protocol**.
9. Under **RADIUS Service Options**, configure the following settings:

| **Encryption**                            | Enable                                                                                                                                                                                                                           |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CN/SAN Idenity**                        | <p>Provide the CN/SAN attribute of your <strong>RADIUS Server Certificate</strong>. </p><p><img src="../../../.gitbook/assets/image (391).png" alt="Showing SAN to be used as Certificate server name" data-size="original"></p> |
| **OCSP Validation**                       | <p>Default: Disabled<br>In case you are bringing your own <strong>RADIUS Server Certificate</strong> and the CA that has issued it allows for its revocation, provide the OCSP Responder URL of your CA here.</p>                |
| **Client Certificate**                    | Select the **RadSec Client Certificate** uploaded to Ruckus SZ previously.                                                                                                                                                       |
| **Server Certificate**                    | Disable                                                                                                                                                                                                                          |
| **RFC5580 Out of Band Location Delivery** | Disable                                                                                                                                                                                                                          |

10. Next, under **Primary Server**, for the **IP Address/FQDN** choose either the IP address or the DNS name of your [RadSec service endpoint](../../../admin-portal/settings/settings-server.md#properties). For the **Port** select **2083**. <br>

    <figure><img src="../../../.gitbook/assets/Screenshot_2024-08-21_at_13_29_21 (1).jpg" alt=""><figcaption></figcaption></figure>
11. Click **OK**.
