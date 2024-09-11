# Aruba

## Prepare Certificates

To establish a valid RadSec connection, your Access Points must trust the RADIUS Server Certificate and your RADIUS server must trust your RadSec client certificate.

1. Download the root certificate of the CA that has issued your RADIUS server certificate as described [here](../../../admin-portal/settings/settings-server.md#download).
2. Create a RadSec client certificate for your WAPs (centrally managed via Aruba Central). If you are using **SCEPman Certificate Master**, the process is described [here](https://docs.scepman.com/certificate-deployment/certificate-master/client-certificate-pkcs-12).&#x20;

{% hint style="warning" %}
Ensure to monitor the expiry of your RadSec client certificate and renew it in due time to prevent service interruptions.
{% endhint %}

3. Add the root certificate of the CA that has issued the RadSec client certificate to your RADIUS instance as described [here](../../../admin-portal/settings/trusted-roots.md#add) and select **RadSec** under **Use for**.\
   In case the RadSec client certificate has been issued by SCEPman and you already trust the SCEPman Root CA for client authentication, simply edit the trusted SCEPman Root CA certificate and select **Both** under **Use for**.&#x20;

## Aruba Central Configuration

{% hint style="info" %}
Below settings are the necessary settings to establish a functional RadSec connection with our service. Configure any other settings at your discretion.
{% endhint %}

For general information on how to import certificates to your Aruba platform, please refer to their documentation:

{% embed url="https://www.arubanetworks.com/techdocs/ArubaOS_85_Web_Help/Content/arubaos-solutions/manage-utilities/impo-cert.htm" %}

1.  Import the root certificate of the CA that has issued your RADIUS server certificate with the type **CA certificate**.

    <figure><img src="../../../../.gitbook/assets/image (151).png" alt=""><figcaption></figcaption></figure>
2.  Import the RadSec client certificate (created in step 2 under [Prepare Certificates](aruba.md#prepare-certificates)) with the type **Server certificate**.

    <figure><img src="../../../../.gitbook/assets/image (134).png" alt=""><figcaption></figcaption></figure>
3.  Under **Access Points >** **Security** select the imported RadSec client certificate for **RadSec** and the RADIUS root CA certificate for **RadSec Certificate Authority**.

    <figure><img src="../../../../.gitbook/assets/image (133).png" alt=""><figcaption></figcaption></figure>
4.  For the RADIUS server configuration, enable **RadSec** and choose either the IP address or the DNS name of your [RadSec service endpoint](../../../admin-portal/settings/settings-server.md#properties).

    <figure><img src="../../../../.gitbook/assets/image (131).png" alt=""><figcaption></figcaption></figure>
