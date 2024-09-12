# Server Trust

{% hint style="info" %}
This is only required if you are using a RADIUS server certificate that was issued by a CA that your clients do not already trust. For example, this is not needed if you are bringing your own server certificate issued by SCEPman.
{% endhint %}

## Part 1: Download the RADIUS Server Certificate

When [downloading](../../admin-portal/settings/settings-server.md#download) the Server certificate, use only the green-marked certificate. This will download the root CA certificate of the issuing CA.

<figure><img src="https://docs.radiusaas.com/~gitbook/image?url=https%3A%2F%2F1222554226-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FSWU1DQ4UGkqER7uGNUOm%252Fuploads%252F6PAKr5RsefmaAKR44h8i%252Fimage.png%3Falt%3Dmedia%26token%3Da7738e9d-f974-4162-a99a-e7c2b68b4b40&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=c7ddd6d1&#x26;sv=1" alt=""><figcaption></figcaption></figure>

## Part 2: Adding a Trusted Certificate Profile for your Endpoint Devices

{% hint style="warning" %}
Ensure, you have reviewed [Part 1](server-trust.md#part-1-download-the-radius-server-certificate).
{% endhint %}

In your Google **Admin console** (admin.google.com)  navigate to **Menu** > **Devices** > **Network** > **Certificates** > **ADD CERTIFICATE**

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>
