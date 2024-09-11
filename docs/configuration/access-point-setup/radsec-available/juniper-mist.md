# Juniper Mist

## Prepare Certificates

1. Download the root certificate of the CA that has issued your RADIUS server certificate as described [here](../../../admin-portal/settings/settings-server.md#download).
2. Create a RadSec client certificate for your Access Points. If you are using **SCEPman Certificate Master**, the process is described [here](https://docs.scepman.com/certificate-deployment/certificate-master/client-certificate-pkcs-12).&#x20;

{% hint style="warning" %}
Ensure to monitor the expiry of your RadSec client certificate and renew it in due time to prevent service interruptions.
{% endhint %}

3. Add the root certificate of the CA that has issued the RadSec client certificate to your RADIUS instance as described [here](../../../admin-portal/settings/trusted-roots.md#add) and select **RadSec** for the trusted certificate type.

## Mist Configuration

{% hint style="info" %}
Below settings are the necessary settings to establish a functional RadSec connection with our service. Configure any other settings at your discretion.
{% endhint %}

1. Navigate to your Mist configuration plane.
2.  To configure the relevant certificates, navigate to **Organization** **>** **Settings**.

    <figure><img src="../../../../.gitbook/assets/image (126).png" alt=""><figcaption></figcaption></figure>
3.  Add the root certificate of the CA that has issued your RADIUS server certificate under **RadSec Certificate**.

    <figure><img src="../../../../.gitbook/assets/image (117).png" alt=""><figcaption></figcaption></figure>


4.  Add your RadSec client certificate (created in step 2 under [Prepare Certificates](juniper-mist.md#prepare-certificates)) to **AP RadSec Certificate**.

    <figure><img src="../../../../.gitbook/assets/image (137).png" alt=""><figcaption></figcaption></figure>
5.  Go to **Site >** **WLANs**.

    <figure><img src="../../../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>
6. Create a new WLAN if you have not already created one for which you want to leverage RADIUS authentication against our service.
7.  Select **Enterprise (802.1X)** as **Security Type**.

    <figure><img src="../../../../.gitbook/assets/image (112).png" alt=""><figcaption></figcaption></figure>
8.  Under **RadSec**, select **Enabled** and set the **Server Name** to the **SAN** attribute of your RADIUS Server certificate.

    <figure><img src="../../../../.gitbook/assets/image (106).png" alt=""><figcaption></figcaption></figure>
9.  For **Server Addresses** use either the IP address or the DNS name of your [RadSec service endpoint](../../../admin-portal/settings/settings-server.md#properties).



    <figure><img src="../../../../.gitbook/assets/image (316).png" alt=""><figcaption></figcaption></figure>

### A complete Run-Through

<figure><img src="../../../../.gitbook/assets/Kapture 2023-02-23 at 16.01.24.gif" alt=""><figcaption></figcaption></figure>
