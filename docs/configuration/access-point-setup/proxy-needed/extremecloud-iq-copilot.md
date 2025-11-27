---
description: >-
  Unfortunately only `ExtremeCloud IQ SiteManager` is able to speak RadSec(as
  far we know). This site handles CoPilot
---

# ExtremeCloud IQ CoPilot

## ExtremeCloud IQ CoPilot configuration

1.  Go to **Configure > Common Objects > Authentication > External Radius Servers** and click **Add**&#x20;

    <figure><img src="../../../.gitbook/assets/image (224).png" alt=""><figcaption></figcaption></figure>
2.  &#x20;Enter the IP Object and give everything a descriptive name

    <figure><img src="../../../.gitbook/assets/image (353).png" alt=""><figcaption></figcaption></figure>



    <figure><img src="../../../.gitbook/assets/image (236).png" alt=""><figcaption></figcaption></figure>
3. Set the **Shared secret** which is displayed on your RaaS website and save the changes

<figure><img src="../../../.gitbook/assets/image (352).png" alt=""><figcaption></figcaption></figure>

4.  Go to **Configure > Network Policies > Add Network Policy**<br>

    <figure><img src="../../../.gitbook/assets/image (355).png" alt=""><figcaption></figcaption></figure>
5. After the policy has been created, you're able to create a **SSID.** Click **Add** on **Wireless Networks**
6.  Choose your preferred **SSID** Name and select **Enterprise** for **SSID Authentication** <br>

    <figure><img src="../../../.gitbook/assets/image (228).png" alt=""><figcaption></figcaption></figure>
7.  Under **Authentication Settings** add the radius server which we've added in step 3 and save everything<br>

    <figure><img src="../../../.gitbook/assets/image (246).png" alt=""><figcaption></figcaption></figure>
8.  Deploy the **Network Policy** to the Access Points<br>

    <figure><img src="../../../.gitbook/assets/image (186).png" alt=""><figcaption></figcaption></figure>



