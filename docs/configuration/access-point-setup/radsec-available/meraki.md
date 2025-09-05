# Meraki



{% hint style="info" %}
To use the RadSec feature on your Meraki APs, firmware version **MR 30.X** or later is required.
{% endhint %}

{% hint style="warning" %}
Customers have reported **delays** between activating the RadSec feature on the Meraki Dashboard and becoming functional.
{% endhint %}

## Prepare certificates

{% hint style="warning" %}
The Meraki platform does not allow you to generate RadSec client certificates from a CA of your choice. Instead, you must use Meraki's built-in **Organization CA** that is unique to your Meraki Organization.
{% endhint %}

Download the root certificate of the CA that has issued your active **RADIUS Server Certificate** as described [here](../../../admin-portal/settings/settings-server.md#download). You will need to upload it to your Meraki console later on.

## Meraki configuration

{% hint style="info" %}
Below settings are the necessary settings to establish a functional RadSec connection with our service. Configure any other settings at your discretion.
{% endhint %}

{% hint style="danger" %}
Ensure to **disable the OCSP revocation check of the RadSec client certificate** as described in step 13 of this guide.
{% endhint %}

{% stepper %}
{% step %}
### Navigate to your Meraki Dashboard

In your Dashboard, select **Wireless > Access Control** and ensure that you have switched to the **new UI version** of the Access control site:

<figure><img src="../../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Configure the RADIUS server in your SSID

Select the **SSID** you wish to configure RADIUS authentication for (or navigate to **Wireless > Configure > SSIDs** to create a new SSID first).

In the **Security** section of your SSID, select **Enterprise with** and in the dropdown **my RADIUS server:**

<figure><img src="../../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

Under **RADIUS**, click **Add server**. Configure the **Host IP or FQDN** to match the IP address or the DNS name of your [RadSec service endpoint](../../../admin-portal/settings/settings-server.md#properties), set the **Port** to 2083 and  set the **Secret** value to "radsec" and activate the **RadSec** checkbox.

<figure><img src="../../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Configure EAP Timeouts

Configure **EAP parameters and timeouts** according to [this](../../../other/faqs/general.md#timers-and-timeouts) reference guide by going to **Wireless** > **Radius** > **Advanced RADIUS settings.** Once configured, it should look similar to the screenshot below. Click **Save** to apply the settings.

<figure><img src="../../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Add RadSec Server Certificate

To upload and generate the required certificates that make the RadSec connection functional, navigate to **Organization > Certificates**.

<figure><img src="../../../.gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

In the top table, click **Upload certificate** and provide the root certificate of the CA that has signed your [**RADIUS Server Certificate**](../../../admin-portal/settings/settings-server.md#server-certificates), which you should have already downloaded in this [step](meraki.md#prepare-certificates). Your Meraki APs now trust your RADIUS server.

{% tabs %}
{% tab title="Using RADIUSaaS Customer CA" %}
<figure><img src="../../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Using SCEPman or other CA" %}
<figure><img src="../../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
### Create Organization CA

Under **RadSec AP Certificates**, first create an **Organization CA** by clicking **Generate CA**. This CA is unique to your Meraki Organization.\
The Meraki platform will now automatically generate RadSec client certificates for all your APs signed by this CA. The lifetime of the certificate is very long (> 50 years), i.e. you do not have to worry about renewing them.

Download the root CA certificate of your **Organization CA** of your Meraki system by clicking **Download CA**.

<figure><img src="../../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
You will not be able to upload RadSec client certificates of another CA with Meraki. Instead the Organization CA, which is unique to your organization, will provide all your access points with suitable certificates.
{% endhint %}
{% endstep %}

{% step %}
### Trust Organization CA in RADIUSaaS

Now, upload the downloaded CA certificate to your [Trusted Certificates in your RADIUSaaS web console](../../../admin-portal/settings/trusted-roots.md#add) and select **RadSec** under **Use for**.

<figure><img src="../../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>


{% endstep %}

{% step %}
### Disable Revocation Check for RadSec Certificates

Finally, disable the [revocation check for the RadSec client certificates](../../../admin-portal/settings/settings-server.md#verification-check-for-radsec-certificates) on your RADIUSaaS instance (this does not adversely affect security as the Meraki Organization CA does not allow to revoke RadSec client certificates). Therefore, navigating to your RADIUSaaS instance and then **Settings > Server Settings** and disable the checkbox **Verification check for RadSec certificates**.

<figure><img src="../../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Test Configuration

To test that the configuration works, you can add a user in your [Portal](../../../admin-portal/users.md#add-a-new-user) and use the Meraki test function.

<figure><img src="../../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

### References

Link to Meraki's documentation for the RadSec configuration: [https://documentation.meraki.com/MR/Encryption\_and\_Authentication/MR\_RADSec](https://documentation.meraki.com/MR/Encryption_and_Authentication/MR_RADSec)
