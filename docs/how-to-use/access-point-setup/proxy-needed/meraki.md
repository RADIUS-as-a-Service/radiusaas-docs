# Meraki

{% hint style="warning" %}
Starting with firmware version **MR 30.X**, Meraki APs support RadSec. Hence, we recommend to update your firmware version and follow the [RadSec guide for Meraki](../radsec-available/meraki.md) instead.
{% endhint %}

## Meraki Configuration&#x20;

1. In your Meraki **Dashboard** go to **Wireless > SSIDs**
2. Enable a new SSID and give it the name you want
   1.  Save your changes

       ![](<../../../.gitbook/assets/image (65) (1) (1) (1) (1) (1) (1).png>)
3. Edit the Settings of your SSID
   1. Under **Network access** select **Enterprise with my RADIUS server**![](<../../../.gitbook/assets/image (64) (1) (1) (1) (1) (1).png>)
4. After that, go to **RADIUS servers** and add your RADIUS servers. Use the **IP Address** of your [**Proxy**](../../../portal/settings/settings-server/ports-and-ip-addresses.md#radius-ip-address-udp), the **Port** 1812 and the **Shared Secret** from your [**Server Settings**](../../../portal/settings/settings-server/) page![](<../../../.gitbook/assets/image (62) (1) (1) (1) (1).png>)
5.  To **test** that the configuration works, you can add a user in your [Portal](../../../portal/users.md#add-a-new-user) and use the Meraki test function

    ![](<../../../.gitbook/assets/image (63) (1) (1) (1).png>)
6. **Save** your changes.
