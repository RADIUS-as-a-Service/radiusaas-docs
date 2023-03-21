---
description: >-
  The Log Exporter allows you to push RADIUSaaS Logs to an external SIEM,
  monitoring and alerting system
---

# 🆕 Log Exporter

### General

Logs will be **fetched every 60 seconds** and sent to your configured **Export Target(s)**. Currently, the Log Exporter can connect to the following target systems:

* [Microsoft Teams Channel](teams.md)
* [Azure Log Analytics Workspace](log-analytics.md)
* [Generic Webhook (JSON)](generic-webhook.md)

The Log Exporter allows you to configure a specific **Message Filter** for each target. For example:&#x20;

* Send every entry where a user was not able to login to a **Log Analytics Workspace**
* Send every failed TCP connection to a **Microsoft Teams Channel**

### Message Filter

The **Message Filter** that can be configured for each target helps you to only receive those logs, that are really relevant for your monitoring and alerting system.

<figure><img src="../../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

The filter can be configured to only consider logs from certain sources/sub-system from the RADIUSaaS platform:

* Rule Engine
* Authorization System
* Proxy Authentication

Furthermore, the log level can be configured for each of those sub-systems.

If you are familiar with reading the RADIUSaaS' [raw log data](../../insights/log.md) and have already identified a set of  messages that are of interest for you, you can very easily derive from those messages the suitable filter settings for export. Therefore, below table provides a mapping from the log message origin (sub-system) to the `tags` property as well as from the log level to the `level` property of each log message.

| Filter               | Tag      | Level                                                                                                                                 |
| -------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Rule Engine          | `engine` | <p>Success = <code>INFO</code><br>Failed = <code>WARNING</code><br>Error = <code>ERROR</code></p>                                     |
| Authorization System | `detail` | <p>Requests = <code>debug</code><br>Success = <code>info</code><br>Failed = <code>warning</code><br>Error = <code>error</code></p>    |
| Proxy Authentication | `proxy`  | <p>Connections = <code>debug</code><br>Success = <code>info</code><br>Failed = <code>warning</code><br>Error = <code>error</code></p> |

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

### Message

No matter which target type(s) you have selected, you will have to edit the data template describing how the export message should be structured using **Jinja2** as template engine:\
[https://jinja.palletsprojects.com/en/3.1.x/templates/](https://jinja.palletsprojects.com/en/3.1.x/templates/)

The Log Exporter has access to every field in a log message that is hierarchically located under the`_source` property. It is made available through the `data` object in the **Message** editor.

<figure><img src="../../../.gitbook/assets/image (2) (3) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
