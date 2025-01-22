# UniFi

{% hint style="info" %}
RADIUS over TLS (RADSEC) has been added to **UniFi Network 8.4** and newer versions. Please have your controller and network devices **up-to-date** before following this guide.
{% endhint %}

{% hint style="warning" %}
Customers have reported **delays** between activating the RadSec feature on the Unify Dashboard and becoming functional.
{% endhint %}

## Prepare certificates

To establish a valid RadSec connection, your Access Points must trust the **RADIUS Server Certificate** and your RADIUS server must trust your **RadSec Client Certificate**. To achieve this,

1.  Download the root certificate of the CA that has issued your active **RADIUS Server Certificate** as described [here](../../../admin-portal/settings/settings-server.md#download).\
    In this example SCEPman is used as Root CA and has issued the RADIUS server certificate. So, we download the root CA certificate from SCEPman portal:\
    \
    ![](<../../../.gitbook/assets/image (1) (1) (1) (1) (1).png>)\
    Afterwards, please convert your certificate to Base-64. This can be easily done via Windows Certificate Export Wizard, OpenSSL or other tools:\


    <figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>
2.  Create a **RadSec Client Certificate** for your access points. If you are using **SCEPman Certificate Master**, the process is described [here](https://docs.scepman.com/certificate-deployment/certificate-master/client-certificate-pkcs-12).\
    In this example we generate a certificate in the format "PEM". Please note down the password, as we need this later.\


    <figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>
3.  Split the generated certificate into the private key (named "priv.key" in this example) and the certificate (named "clientcert.cer"). This can be easily done via a text editor:\


    <figure><img src="../../../.gitbook/assets/Screenshot 2024-09-24 101150.png" alt=""><figcaption></figcaption></figure>
4.  Add the root certificate of the CA that has issued the **RadSec Client Certificate** to your RADIUS instance as described [here](../../../admin-portal/settings/trusted-roots.md#add) and select **RadSec** under **Use for**.\
    In case the **RadSec Client Certificate** has been issued by SCEPman (this example) and you already trust the SCEPman Root CA for client authentication, simply edit the trusted SCEPman Root CA certificate and select **Both** under **Use for**:\
    \


    <figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

## UniFi configuration

{% hint style="info" %}
Below settings are the necessary settings to establish a functional RadSec connection with our service. Configure any other settings at your discretion.
{% endhint %}

1. Navigate to your Unifi Network controller and open **Settings** **> Profiles > RADIUS**.
2.  Create a new profile or update an existing one:\


    <figure><img src="../../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>
3. Fill in the required information:
   1. **RADIUS Assigned VLAN Support**: optional / if needed
   2. **RADIUS Settings**:
      1. **TLS**: Enable the checkbox.
      2. **Authentication Servers**:\
         \- **Server IP Address/es**: Provide the IP address of your [RadSec service endpoint](../../../admin-portal/settings/settings-server.md#properties).\
         \- **Port**: 2083.\
         \- **Shared Secret**: `radsec`.
      3. **Client Certificate**: Upload the **RadSec Client Certificate** (obtained from step 3 [here](unifi.md#prepare-certificates)).
      4. **Private Key**: Upload the private key of your **RadSec Client Certificate** (obtained from step 3 [here](unifi.md#prepare-certificates)).
      5. **Private Key Password**: as noted down.
      6. **CA Certificate**: Upload the Root certificate of the CA that has issued your **RADIUS Server Certificate** (obtained from step 1 [here](unifi.md#prepare-certificates)).
      7. **Accounting**: Enable the checkbox.
      8. **RADIUS Accounting Server**:\
         \- **Server IP Address/es**: Provide the IP address of your [RadSec service endpoint](../../../admin-portal/settings/settings-server.md#properties).\
         \- **Port**: 2083\
         \- **Shared Secret**: `radsec`
      9.  **Interim Update Interval**: optional / if needed\


          <figure><img src="../../../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>
4.  Assign this profile to the desired WiFi profile:\


    <figure><img src="../../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>
5. Give your Access Points some time to apply the new configuration:\
   \
   ![](<../../../.gitbook/assets/image (9) (1).png>)

### Reference: UniFi Help Center

[UniFi Gateway - Configuring a RADIUS Server](https://help.ui.com/hc/en-us/articles/360015268353-UniFi-Gateway-Configuring-a-RADIUS-Server)
