# Log Analytics

## Prerequisites

To ingest RADIUSaaS logs into an **Azure Log Analytics Workspace**, please make sure that a workspace was already created and that you have the following **required values** available:

* Customer ID
* Shared Key
* Event Name

## Configuration Steps

Follow these steps to add a **Log Analytics** export target:

* Navigate to your **RADIUSaaS Admin Portal**
* Click **+ Export Target**
* Select **Azure Loganalytics Exporter**
* Provide a **Name** and **Description**
* Configure the **Message Filters** to your needs
* Structure the **Data** to be ingested into your Log Analytics Workspace

{% hint style="warning" %}
Some data which you might send to your Log Analytics workspace will include new line characters. \
To get a valid JSON for every entry, the template engine has a global **tojson** **parser** which will apply for all variables you access.&#x20;

Therefore, **do not quote any jinja variable**.
{% endhint %}

**Correct**

![](<../../../.gitbook/assets/image (5) (2).png>)

**Wrong**

![](<../../../.gitbook/assets/image (1) (1) (1).png>)
