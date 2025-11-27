# Meraki

{% hint style="warning" %}
Starting with firmware version **MR 30.X**, Meraki APs support RadSec. Hence, we recommend to update your firmware version and follow the [RadSec guide for Meraki](../radsec-available/meraki.md) instead.
{% endhint %}

## Meraki configuration&#x20;

1. In your Meraki **Dashboard** go to **Wireless > SSIDs**
2. Enable a new SSID and give it the name you want
   1.  Save your changes

       ![](<../../../.gitbook/assets/image (269).png>)
3. Edit the Settings of your SSID
   1. Under **Network access** select **Enterprise with my RADIUS server**![](<../../../.gitbook/assets/image (219).png>)
4. After that, go to **RADIUS servers** and add your RADIUS servers. Use the **IP Address** of your [**Proxy**](../../../admin-portal/settings/settings-server.md#properties-1), the **Port** 1812 and the **Shared Secret** from your [**Server Settings**](../../../admin-portal/settings/settings-server.md) page![](<../../../.gitbook/assets/image (291).png>)
5.  Configure **EAP parameters and timeouts** according to [this ](https://docs.radiusaas.com/other/faqs/general)reference guide by going to **Wireless** > **Radius** > **Advanced RADIUS settings.** Once configured, it should look similar to the screenshot below.&#x20;

    <figure><img src="../../../.gitbook/assets/2024-05-17_11h10_26.png" alt=""><figcaption><p>Showing <strong>EAP parameters and timeouts</strong></p></figcaption></figure>
6.  To **test** that the configuration works, you can add a user in your [Portal](../../../admin-portal/users.md#add-a-new-user) and use the Meraki test function

    ![](<../../../.gitbook/assets/image (255).png>)
7. **Save** your changes.
