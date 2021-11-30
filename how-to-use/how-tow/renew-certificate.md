---
description: >-
  This page describes the process to renew a certificate without connection
  disruption for your clients
---

# Renew Certificate

You'll need to answer the following questions before continuing:&#x20;

* Do you want to buy your own certificate or do you want to use the free certificate which your instance can create for you?
* Are you deploying user- or device certificates&#x20;

### RADIUS Server part 1/2

1. Create/Upload a new certificate(download the certificate afterwards, you will need it for the Azure profiles).
   1. If you want to use the free certificates, create your CA as described [here](../../portal/settings-server.md#custom-cas). If you want to use your own certificate, select **PEM encoded Certificate** in the **Add** certificate dialog, select the certificate name and upload the public and private key.
2. Generate your new XML if you're deploying **device certificates** as described [here](../../portal/settings-trusted-roots/xml.md#wifi).

### Azure

1. Deploy your new **Server certificate** to your clients as described [here](../../azure/trusted-root.md#adding-a-trusted-root-profile-for-your-clients) as new profile.
2. Update your WiFi Profile
   1. If you're using device certificate, upload your new XML file. For user certificate **add** not replace the new **Trusted root certificate** profile.
3. Wait until all your clients received the updated profiles&#x20;

### WiFi infrastructure

{% hint style="info" %}
This step is only necessary if you're using [RadSec](../../details.md#what-is-radsec)
{% endhint %}

Upload your new **Server certificate** to your Access Points.&#x20;

### RADIUS Server part 2/2

{% hint style="danger" %}
Do this **only** if you are **sure** that all your clients received the updated profiles. They will not be able to connect if they didn't received them.
{% endhint %}

1. Activate your new certificate as described [here](../../portal/settings-server.md#certificate-activation).





\


