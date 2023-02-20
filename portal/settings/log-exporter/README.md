---
description: The Log Exporter will send your RADIUS Logs to the defined endpoints
---

# 🆕 Log Exporter

### General

Logs will be fetched every 60 seconds and send to your configured endpoints. You're able to select a specific Message filter for every target. For example:&#x20;

* Send every entry where a user was not able to login to Loganalytics
* Send every failed TCP connection to Teams&#x20;

### Message filter

Each target has a Message filter to not send every log entry to your Targets

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Those filter represent the **tag** and **level** of your Logs

| Filter               | Tag    | Level                                                                              |
| -------------------- | ------ | ---------------------------------------------------------------------------------- |
| Rule Engine          | engine | <p>Success = INFO<br>Failed = WARNING<br>Error = ERROR</p>                         |
| Authorization System | detail | <p>Requests = debug<br>Success = info<br>Failed = warning<br>Error = error</p>     |
| Proxy Authentication | proxy  | <p>Connections = debug<br>Susscess = info<br>Failed = warning<br>Error = error</p> |

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>



### Message

No matter which target type you've selected, you will have to edit the data template how the destination message should look like using Jinja2 as template engine.\
[https://jinja.palletsprojects.com/en/3.1.x/templates/](https://jinja.palletsprojects.com/en/3.1.x/templates/)

&#x20;

You can access every field under `_source` from your log under the **data** object.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
