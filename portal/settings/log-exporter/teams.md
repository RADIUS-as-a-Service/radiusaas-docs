# Teams

## Prerequisites

To ingest RADIUSaaS logs into a **Microsoft Teams Channel**, only Microsoft Teams itself as well as privileges to add **connectors** to the relevant **channel** are required.

## Configuration Steps

Follow those steps to push RADIUSaaS logs to a **Microsoft Teams Channel**:

* Navigate to **Microsoft Teams**
* Right-click on the **Channel** and select **Connectors**

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

* Create an **Incoming Webhook** connector by searching for this connector type and subsequently clicking **Configure**

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

* Provide a **Name** for the connector

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

* Click **Create** to generate the **Webhook Connector URL**

Now it is time to configure the export target in the Log Exporter. Therefore,

* Navigate to your **RADIUSaaS Admin Portal**
* Click **+ Export Target**
* Select **Teams Exporter**
* Provide a **Name** and **Description**
* Configure the **Message Filters** to your needs
* Paste the **Webhook Connector URL** in the field **Teams channel Webhook URL**
* Configure the **Message** to be sent to your Microsoft Teams Channel

<figure><img src="../../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>
