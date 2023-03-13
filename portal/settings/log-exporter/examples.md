---
description: >-
  This page provides some real world scenarios giving you guidance on how to
  configure the Log Exporter for your scenario.
---

# Examples

## Example 1: General Authentication Information

### Scope and Assumptions

The scope of the query provided below is as follows:

* The admin is interested in understanding which users and/or devices are authenticating (successful or unsuccessful) and to built frequency statistics based on that
* No VLAN tagging is used
* Only certificate-based authentication is used (no username-password-based authentication)

### Target

[Log Analytics](log-analytics.md) or [General Webhook](generic-webhook.md)

### Message Filter Configuration

#### Rule Engine

| Log Level | Enabled                               |
| --------- | ------------------------------------- |
| Success   | <mark style="color:red;">False</mark> |
| Failed    | <mark style="color:red;">False</mark> |
| Error     | <mark style="color:red;">False</mark> |

#### Authorization System

| Log Level | Enabled                                |
| --------- | -------------------------------------- |
| Requests  | <mark style="color:red;">False</mark>  |
| Success   | <mark style="color:green;">True</mark> |
| Failed    | <mark style="color:green;">True</mark> |
| Error     | <mark style="color:red;">False</mark>  |

#### Proxy Authentication

| Log Level   | Enabled                               |
| ----------- | ------------------------------------- |
| Connections | <mark style="color:red;">False</mark> |
| Success     | <mark style="color:red;">False</mark> |
| Failed      | <mark style="color:red;">False</mark> |
| Error       | <mark style="color:red;">False</mark> |

### Data Configuration

{% code lineNumbers="true" %}
```json
{
    "Decision": {{ data.get('Packet-Type') }},
    "Level": {{ data.level }},
    "IP": {{ data.get('Packet-Dst-Address') }},
    "Username": {{ data.get('User-Name') }},
    {% raw %}
{% if data.get('TLS-OCSP-Cert-Valid') != None %}
        "OCSPStatus": {{ data.get('TLS-OCSP-Cert-Valid') }},
    {% endif %}
    {% if data.level == "warning" %}
      "FailReason": {{ data.get('Module-Failure-Message') }},
    {% endif %}
{% endraw %}
    "Datetime" : {{ data.Datetime }}
}
```
{% endcode %}

## Example 2: Detailed Authentication Information&#x20;

### Scope and Assumptions

The scope of the query provided below is as follows:

* The admin is interested in understanding which users and/or devices are authenticating (successful or unsuccessful)
* The OCSP response of the CA if certificates are used
* The used Access Point (via MAC address)
* The RADIUS [Rule](../rules/) that was triggered / VLAN that was tagged
* Both, certificate-based authentication and username-password-based authentication are considered

### Target

[Log Analytics](log-analytics.md) or [General Webhook](generic-webhook.md)

### Message Filter Configuration

#### Rule Engine

| Log Level | Enabled                                |
| --------- | -------------------------------------- |
| Success   | <mark style="color:green;">True</mark> |
| Failed    | <mark style="color:green;">True</mark> |
| Error     | <mark style="color:red;">False</mark>  |

#### Authorization System

| Log Level | Enabled                               |
| --------- | ------------------------------------- |
| Requests  | <mark style="color:red;">False</mark> |
| Success   | <mark style="color:red;">False</mark> |
| Failed    | <mark style="color:red;">False</mark> |
| Error     | <mark style="color:red;">False</mark> |

#### Proxy Authentication

| Log Level   | Enabled                               |
| ----------- | ------------------------------------- |
| Connections | <mark style="color:red;">False</mark> |
| Success     | <mark style="color:red;">False</mark> |
| Failed      | <mark style="color:red;">False</mark> |
| Error       | <mark style="color:red;">False</mark> |

### Data Configuration

<pre class="language-json" data-line-numbers><code class="lang-json">{
    "Decision": {{ data.get('Engine-Decision') }},
    "Datetime" : {{ data.Datetime }},
<strong>    "Level": {{ data.level }},
</strong>    "Authtype": {{ data.get('Auth-Source-Type') }},
    "Client-MAC": {{ data.get('Client-MAC') }},
    "Username": {{ data.get('User-Name') }},
    "Applied-Rule": {{ data.get('Applied-Rule') }},
    "VLAN": {{ data.get('Assigned-VLAN', 'No VLAN assigned') }}
    {% if data.get('Auth-Source-Type') == "WiFi" %}
        "SSID": {{ data.get('SSID') }},
        "AP-MAC": {{ data.get('AP-MAC') }},
    {% endif %}
    {% if data.get('Authtype') == "Certificate" %}
        "OCSPStatus": {{ data.get('OCSP-Response', "Not performed") }},
    {% endif %}
    {% if data.level == "WARNING" %}
        "FailReason": {{ data.get('Reject-Description') }}
    {% endif %}
}
</code></pre>

## Example 3: General Error Notifications

### Scope and Assumptions

The scope of the query provided below is as follows:

* The admin is interested in receiving pro-active notifications about errors on the RADIUSaaS platform for the operations team.

### Target

[Teams](teams.md)

### Message Filter Configuration

#### Rule Engine

| Log Level | Enabled                               |
| --------- | ------------------------------------- |
| Success   | <mark style="color:red;">False</mark> |
| Failed    | <mark style="color:red;">False</mark> |
| Error     | <mark style="color:red;">False</mark> |

#### Authorization System

| Log Level | Enabled                                |
| --------- | -------------------------------------- |
| Requests  | <mark style="color:red;">False</mark>  |
| Success   | <mark style="color:red;">False</mark>  |
| Failed    | <mark style="color:red;">False</mark>  |
| Error     | <mark style="color:green;">True</mark> |

#### Proxy Authentication

| Log Level   | Enabled                                |
| ----------- | -------------------------------------- |
| Connections | <mark style="color:red;">False</mark>  |
| Success     | <mark style="color:red;">False</mark>  |
| Failed      | <mark style="color:red;">False</mark>  |
| Error       | <mark style="color:green;">True</mark> |

### Data Configuration

{% code lineNumbers="true" %}
```
The RADIUS system has issues!
Message: {{ data.get('message') }}

Raw data:
{{ data }}
```
{% endcode %}
