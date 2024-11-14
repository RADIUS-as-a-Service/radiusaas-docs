# UniFi

{% hint style="warning" %}
Starting with **UniFi Network 8.4** RadSec is supported. We recommend to follow the [RadSec guide for UniFi](../radsec-available/unifi.md) instead.
{% endhint %}

## UniFi configuration

### RADIUS profile

Please go through the following steps and configurations to create a RADIUS profile:

1. In UniFi navigate to **Settings** and select **Profiles**
2. Click **Create a new RADIUS Profile**
3. Fill the forms with all your information

![](<../../../../.gitbook/assets/image (180).png>)

### SSID <a href="#ssid" id="ssid"></a>

Do the following steps and configurations for SSID:

1. In UniFi navigate to **Settings** and select **Wireless Networks**
2. Click **Create new Wireless Network**
3. Enter a name for **Name/SSID**
4. For **Security** choose **WPA Enterprise**
5. Finally as **RADIUS Profile** select the created profile

![](https://gblobscdn.gitbook.com/assets%2F-Lzl3JXanfpvdg6pLlGg%2F-M03hV6tYhKuZqKfxnpF%2F-M03l0lPBQzneR9sw0mC%2Fimage.png?alt=media\&token=162f4892-09ba-448a-8cf7-4e12d6bb614c)
