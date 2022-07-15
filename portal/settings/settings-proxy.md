---
description: >-
  If your network gear does not support RadSec natively, you are required to set
  up a proxy. Proxy settings are available under
  https://YOURNAME.radius-as-a-service.com/settings/proxy
---

# RADIUS Proxy

### Performance / Scaling

Each proxy can handle up to 1500 **concurrent** connections flawlessly.&#x20;

#### Scaling

We've never seen any issues if you choose Europe as proxy location, no matter where your clients are located. Nonetheless it can increase your performance to choose the Proxy at one location which is as near as possible to your offices.

To ensure smooth operation, consider the following number of proxies based on your users(your needs may change if your offices are more globally distributed):

|      User      | Proxy amount |
| :------------: | :----------: |
|    0 - 100     |       1      |
|   100 - 4000   |       2      |
|  4000 - 10000  |       4      |
| 10000 - 100000 |       8      |

### Add&#x20;

To Add a new proxy, simply click **Add**, choose your **Region** and click **Create.**&#x20;

![](<../../.gitbook/assets/image (76).png>)

****\
****After the installation has finished, it can take up to 15 minutes until your proxy has established a connection to your RADIUS server.

![](<../../.gitbook/assets/image (66).png>)



### Delete

To **Delete** a proxy, simply click **Delete** of corresponding table row and confirm your choice.&#x20;

![](<../../.gitbook/assets/image (72).png>)
