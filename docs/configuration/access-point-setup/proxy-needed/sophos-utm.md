# Sophos UTM

{% hint style="info" %}
The Sophos appliance does not allow to assign different RADIUS servers to different SSIDs. So be aware of that if you are migrating from a onPremises RADIUS server to RADIUS-as-a-Service. May talk with our support team to plan your migration.&#x20;
{% endhint %}

## Sophos configuration

### RADIUS profile

Please go through the following steps and configurations to create a RADIUS profile:

1. In Sophos navigate to **Definitions & Users** and select **Authentication Services** and select the tab **Servers**
2. Click **New Authentication Server**
3. Fill the forms with all your information

![](../../../.gitbook/assets/sophos_1.png)

![](../../../.gitbook/assets/sophos_2.png)

### SSID configuration&#x20;

Perform the following steps and configurations for SSID:

1. Navigate to **Wireless Protection**, select **Global Settings** and go to the tab **Advanced**
2. Select the created RADIUS profile
3. Then navigate to your **SSID** settings which can be found under **Wireless Protection > Wireless Networks**
4. Create a SSID with the **Encryption Mode, WPA2/WPA-Enterprise**

![](../../../.gitbook/assets/sophos_3.png)

![](../../../.gitbook/assets/sophos_4.png)
