---
description: >-
  If your network gear does not support RadSec natively, you are required to set
  up a proxy. Proxy settings are available under
  https://YOURNAME.radius-as-a-service.com/settings/proxy
---

# RADIUS Proxy

### Architecture

{% hint style="warning" %}
Since there are at least two proxies part of your license, we strongly recommend to set up two proxies in different regions for redundancy. Most network gear vendors support the configuration of a primary and secondary (backup) RADIUS server, which we recommend to leverage with the two proxies.
{% endhint %}

#### Performance

Each proxy can handle up to 1500 **concurrent** connections flawlessly.&#x20;

This corresponds to a time-based performance of **10.000 authentications per minute per proxy**.

#### Scaling

We've never seen any issues if you choose Europe as proxy location, no matter where your clients are located. Nonetheless it can increase your performance to choose the Proxy at one location which is as near as possible to your offices.

To ensure smooth operation, consider the following number of proxies based on your users(your needs may change if your offices are more globally distributed):

|       User       | Proxy amount |
| :--------------: | :----------: |
|     50 - 2500    |       2      |
|   2501 - 10,000  |       3      |
|  10,001 - 25,000 |       4      |
|  25,001 - 50,000 |       6      |
| 50,001 - 100,000 |      10      |

#### Regions

You can deploy the proxy servers in the following regions:

* Canada
* Europe
* India
* Singapore
* UK
* USA

#### Load balancing

If you have a setup with >1000 users, we highly recommend to ensure, that your network equipment will send equal amounts of authentications to each proxy.

For network equipment, where you can define the priority of the RADIUS services, you can ensure load-balancing by defining different priority orders for your different network equipment instances.

Example: You have 5 WIFI controllers and 3 RADIUSaaS proxies. You may configure the following priority orders in your WIFI controllers:

| Wifi Controller # | RADIUS Priority Order |
| ----------------- | --------------------- |
| 1 and 4           | 1, 2, 3               |
| 2 and 5           | 2, 3, 1               |
| 3                 | 3, 2, 1               |

### Add&#x20;

To Add a new proxy, simply click **Add**, choose your **Region** and click **Create.**&#x20;

![](<../../.gitbook/assets/image (76) (1) (1).png>)

****\
****After the installation has finished, it can take up to 15 minutes until your proxy has established a connection to your RADIUS server.

![](<../../.gitbook/assets/image (66) (1).png>)



### Delete

To **Delete** a proxy, simply click **Delete** of corresponding table row and confirm your choice.&#x20;

![](<../../.gitbook/assets/image (72) (1).png>)
