---
description: >-
  Proxy settings are available under
  https://YOURNAME.radius-as-a-service.com/settings/proxy
---

# RADIUS Proxies

### Architecture

{% hint style="warning" %}
Since there are at least two RADIUS-speaking public IP addresses included in your subscription, we strongly recommend to configure those two IP addresses so they can be used as primary and secondary RADIUS server on your network gear and appliances. Thereby, the second public IP address should be located in a different geo-region than the primary address.
{% endhint %}

#### Performance

Each proxy can handle up to **1,500** **concurrent** connections flawlessly.&#x20;

This corresponds to a time-based performance of **10,000 authentications per minute per proxy**.

#### Scaling

We have never never seen any issues if you choose **Europe** as proxy location, no matter where your clients are located. Nonetheless it can increase your performance to choose the Proxy at one location which is as close as possible to your offices and sites.

To ensure smooth operation, consider the following number of proxies based on the number of users you have licensed (your needs may change if your offices are more globally distributed):

|       User       | Proxy Count |
| :--------------: | :---------: |
|     50 - 2500    |      2      |
|   2501 - 10,000  |      3      |
|  10,001 - 25,000 |      4      |
|  25,001 - 50,000 |      6      |
| 50,001 - 100,000 |      10     |

{% hint style="info" %}
In case your RADIUSaaS instance comes with a [Universal IP Address](settings-server/ports-and-ip-addresses.md#universal-ip-address-tcp-+-udp), your proxy count is reduced by one, since the Universal IP Address should be used as the primary RADIUS server.
{% endhint %}

#### Regions

You can deploy the proxy servers in the following regions:

* Canada
* Europe
* India
* Singapore
* UK
* USA

#### Load balancing

If you have a setup with more than 1,000 users, we highly recommend to ensure, that your network equipment will send equal amounts of authentications to each proxy.

For network equipment, where you can define the priority of the RADIUS servers, you can ensure load-balancing by defining different priority orders for your different network equipment instances.

**Example:** You have 5 WiFi controllers and 3 RADIUS Proxies. You may then configure the following priority orders in your WiFi controllers:

| Wifi Controller # | RADIUS Priority Order |
| ----------------- | --------------------- |
| 1 and 4           | 1, 2, 3               |
| 2 and 5           | 2, 3, 1               |
| 3                 | 3, 1, 2               |

#### Properties

The IP address, ports and shared secret of the RADIUS Proxies will be displayed under [Server Settings](settings-server/) --> [Ports and IP Addresses](settings-server/ports-and-ip-addresses.md).

### Add&#x20;

To Add a new proxy, simply click **Add**, choose your **Region** and click **Create.**&#x20;

![](<../../.gitbook/assets/image (76) (1) (1).png>)

\
After the installation has finished, it can take up to 15 minutes until your proxy has established a connection to your RADIUS server.

![](<../../.gitbook/assets/image (66) (1).png>)



### Delete

To **Delete** a proxy, simply click **Delete** of corresponding table row and confirm your choice.&#x20;

![](<../../.gitbook/assets/image (72) (1).png>)
