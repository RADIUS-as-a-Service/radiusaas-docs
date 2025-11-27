# Teams

## Prerequisites

To ingest RADIUSaaS logs into a **Microsoft Teams Channel**, only Microsoft Teams itself as well as privileges to add **connectors** to the relevant **channel** are required.

## Configuration steps

Follow those steps to push RADIUSaaS logs to a **Microsoft Teams Channel**:

1. Navigate to **Microsoft Teams**
2. Click on the **Channel** > **More options** > **Manage team**

<figure><img src="../../../.gitbook/assets/image (464).png" alt=""><figcaption></figcaption></figure>

3. Go to **Apps**
4. Click **+ Get more apps**

<figure><img src="../../../.gitbook/assets/image (466).png" alt=""><figcaption></figcaption></figure>

5. Search for "**Incoming Webhook**"
6. Click **Add** then **Add to a team**

<figure><img src="../../../.gitbook/assets/image (468).png" alt=""><figcaption></figcaption></figure>

7. Select your team
8. Click **Set up a connector**

<figure><img src="../../../.gitbook/assets/image (470).png" alt=""><figcaption></figcaption></figure>

9. Provide a **Name** for the connector
10. Upload a logo
11. Click **Create** to generate the **Webhook Connector URL**

<figure><img src="../../../.gitbook/assets/image (472).png" alt="" width="554"><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (474).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Make sure you copy the Url.&#x20;
{% endhint %}

Now it is time to configure the export target in the Log Exporter.&#x20;

* Navigate to your **RADIUSaaS Admin Portal > Log Exporter**
* Click **+ Add** > **Teams**
* Provide a **Name** and **Description**
* Configure the **Message Filters** to your needs
* Paste the **Webhook Connector URL** in the field **Teams channel Webhook URL**
* Configure the **Message** to be sent to your Microsoft Teams Channel

<figure><img src="../../../.gitbook/assets/image (475).png" alt=""><figcaption></figcaption></figure>
