---
description: >-
  This screenshots have been made in corporation with one of our customers.
  Please excuse incompleteness in regard to navigating in the Aruba portal.
---

# Aruba

## Prepare Certificates

1. **Download** the root certificate of the CA that has issued your RADIUS server certificate as described [here](../../../portal/settings/settings-server/certificates.md#download).
2. **Create** a client certificate for your Access Points. If you are using **SCEPman Certificate Master**, the process is described here: [https://docs.scepman.com/certificate-deployment/certificate-master/client-certificate-pkcs-12](https://docs.scepman.com/certificate-deployment/certificate-master/client-certificate-pkcs-12)
3. **Add** the root certificate of the CA that has issued the client certificate on your Access Point to your RADIUS instance as described [here](../../../portal/settings/settings-server/certificates.md#radsec-connection-certificates).

## Aruba configuration

For general information on how to import certificates on your Aruba platform, please refer to their documentation:

{% embed url="https://www.arubanetworks.com/techdocs/ArubaOS_85_Web_Help/Content/arubaos-solutions/manage-utilities/impo-cert.htm" %}

1.  **Import** the root certificate of the CA that has issued your RADIUS server certificate with the type **CA certificate**

    <figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>
2.  **Import** your Access Point **client certificate** (created in step 2 of [Prepare Certificates](aruba.md#prepare-certificates)) with the type **Server certificate**

    <figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>
3.  Under **Access Points** --> **Security** select your imported client certificate from step 2 for **RadSec** and the RADIUS CA certificate for **RadSec Certificate Authority**

    <figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>


4.  For the **RADIUS** Server configuration, enable **RadSec** and choose either the [**IP address**](../../../portal/settings/settings-server/ports-and-ip-addresses.md) or the [**DNS** **name**](../../../portal/settings/settings-server/ports-and-ip-addresses.md) of your RadSec service endpoint.

    <figure><img src="../../../.gitbook/assets/image (2) (3).png" alt=""><figcaption></figcaption></figure>
