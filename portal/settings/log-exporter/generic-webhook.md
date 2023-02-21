# Generic Webhook

## Prerequisites

To ingest RADIUSaaS logs into a generic **webhook** that accepts **JSON-structured HTTP bodies**, a suitable webhook including a **publicly available URL** are required as well as **authentication credentials,** if applicable.

The RADIUSaaS Log Exporter allows authentication via a **static API key** (URL-, header-, or body-encoded).

## Configuration Steps

Follow these steps to add a **Generic Webhook** export target:

* Navigate to your **RADIUSaaS Admin Portal**
* Click **+ Export Target**
* Select **Generic Webhook**
* Provide a **Name** and **Description**
* Configure the **Message Filters** to your needs
* Paste your webhook URL in the field **Webhook URL**
* Specify the **HTTP method** that shall be used for transmitting the data (POST, GET, PUT)
* Add relevant **HTTP headers**, e.g. for authentication purposes
* Configure the message **Body** to be sent to your webhook

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

