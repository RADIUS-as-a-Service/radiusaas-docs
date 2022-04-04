# Meraki

## Proxy

Because Meraki is not able to speak RadSec, you'll have to create a [Proxy](../../../portal/settings-proxy.md) first.



## Meraki configuration&#x20;

1. In your Meraki dashboard go to **Wireless -> SSIDs**
2. Enable a new SSID and give it the name you want
   1.  Save your changes

       ![](<../../../.gitbook/assets/image (65) (1) (1) (1).png>)
3. Edit the Settings of your SSID
   1. Under **Network access** select _Enterprise with 'my RADIUS server'_![](<../../../.gitbook/assets/image (64) (1) (1) (1).png>)__
4. After that you can got to **RADIUS servers** and add your RADIUS servers. Use the IP Address of your **Proxy**, the port 1812 and the shared secret from your [Server Settings](../../../portal/settings-server/) page![](<../../../.gitbook/assets/image (62) (1) (1).png>)
5.  To test that the configuration works, you can add a user in your [Portal](../../../portal/users.md#add-a-new-user) and use the Meraki test function

    ![](<../../../.gitbook/assets/image (63) (1) (1).png>)
6. Save your changes
