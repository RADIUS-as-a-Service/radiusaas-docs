---
description: >-
  This page provides some real world scenarios giving you guidance on how to
  configure the Log Exporter for your scenario.
---

# Examples

## Example 1: General authentication information

### Scope and assumptions

The scope of the query provided below is as follows:

* the admin is interested in understanding which users/devices are authenticating (accepted or rejected) and to built frequency statistics based on that
* no VLAN tagging is used
* only certificate-based authentication is used (no username-password-based authentication)

### Target

[Log Analytics](log-analytics.md) or [General Webhook](generic-webhook.md)

### Message Filter configuration

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
    "Decision": {{ data.get("Packet-Type") }},
    "Level": {{ data.level }},
    "IP": {{ data.get("Packet-Dst-Address") }},
    "Username": {{ data.get("User-Name") }},
    {% raw %}
{% if data.get("TLS-OCSP-Cert-Valid") != None %}
        "OCSPStatus": {{ data.get("TLS-OCSP-Cert-Valid") }},
    {% endif %}
    {% if data.level == "warning" %}
      "FailReason": {{ data.get("Module-Failure-Message") }},
    {% endif %}
{% endraw %}
    "Datetime" : {{ data.Datetime }}
}
```
{% endcode %}

## Example 2: Detailed authentication information&#x20;

### Scope and assumptions

The scope of the query provided below is as follows:

* the admin is interested in understanding which users/devices are authenticating via certificate or username & password (accepted or rejected)
* username and certificate details with OCSP response
* SSID and used Access Point (MAC address)
* RADIUSaaS [Rule](../rules/) that was triggered, if applicable: assigned VLAN
* correlation ID for further investigation

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

### Data configuration

{% code lineNumbers="true" %}
```json
{
    "Decision": {{ data.get("Engine-Decision") }},
    "Datetime" : {{ data.Datetime }},
    "Level": {{ data.level }},
    "Authtype": {{ data.get("Authtype") }},
    "Client-MAC": {{ data.get("Client-MAC") }},
    "Username": {{ data.get("User-Name") }},
    "Applied-Rule": {{ data.get("Applied-Rule") }},
    "VLAN": {{ data.get("Assigned-VLAN", "No VLAN assigned") }},
    "Auth-Source-Type": {{ data.get("Auth-Source-Type") }},
    {% raw %}
{% if data.get("Auth-Source-Type") == "WiFi" %}
        "SSID": {{ data.get("SSID") }},
        "AP-MAC": {{ data.get("AP-MAC") }},
    {% endif %}
    {% if data.get("Authtype") == "Certificate" %}
        "Certificate-CommonName": {{ data.get("Certificate-Details", {}).get("TLS-Cert-Common-Name") }},
        "Certificate-Serial": {{ data.get("Certificate-Details", {}).get("TLS-Client-Cert-Serial") }},
    {% endif %}
    {% if data.get("Verify-Result") != None %}
        "Verify-Result": {{ data.get("Verify-Result") }},
        "Verify-Status": {{ data.get("Verify-Status") }},
        "Verify-Type": {{ data.get("Verify-Type") }},
        "Verify-Description": {{ data.get("Verify-Description") }},
    {% endif %}
    {% if data.get("Reject-Description") != None %}
        "Reject-Description": {{ data.get("Reject-Description") }},
    {% endif %}
{% endraw %}
    "GKG-Correlation-Id": {{ data.get("GKG-Correlation-Id") }}
}
```
{% endcode %}

## Example 3: General error notifications

### Scope and assumptions

The scope of the query provided below is as follows:

* the admin is interested in receiving pro-active notifications about errors on the RADIUSaaS platform for the operations team.

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

### Data configuration

#### Teams

{% code lineNumbers="true" %}
```
The RADIUS system has issues!
Message: {{ data.get('message') }}

Raw data:
{{ data }}
```
{% endcode %}
