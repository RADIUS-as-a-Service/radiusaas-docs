---
description: >-
  This article demonstrates how to use the provided API for external monitoring
  of your RADIUSaaS instance.
---

# External Monitoring

## Overview

The monitoring endpoint of your RADIUSaaS instance allows you to perform the following tasks in your own 3rd party monitoring solution:

* Monitor uptime of your RadSec endpoints.
* Monitor uptime of your RADIUS proxies.
* Monitor expiry of your RADIUS Server Certificate and its issuing CA.

If feasible in your monitoring solution, aggregation and metrics can be built around those monitors to trigger automated alerts.&#x20;

## API Schema Defition

Please refer to the [API documentation](./#api-reference) in your RADIUSaaS Admin Portal for detailed information on the schema of the `/status` endpoint.

## API Examples

{% hint style="info" %}
Please note that we are unable to provide support for 3rd party monitoring solutions and you will need to bring your own expertise beyond the scope of this article.&#x20;
{% endhint %}

{% stepper %}
{% step %}
### Create an Access Token as described [here](../../admin-portal/settings/permissions.md#access-tokens).
{% endstep %}

{% step %}
### Retrieve Data

To retrieve data from the API endpoint, authenticate your requests using the access token created previously:

{% tabs %}
{% tab title="PowerShell" %}
1. **Store the Access Token**

Store the access token in a PowerShell variable for easy reference. Replace `your_access_token` with the actual token.

```powershell
$accessToken = "your_access_token"
```

2. **Make the API Request**

Use PowerShell's `Invoke-RestMethod` to send a request to the desired API. Ensure to include the access token in the request header.

```powershell
$url = "https://contoso.radius-as-a-service.com/api/status"
$headers = @{
    Authorization = "Bearer $accessToken"
}
$response = Invoke-RestMethod -Uri $url -Headers $headers -Method Get
```
{% endtab %}

{% tab title="cURL" %}
```
curl -i https://contoso.radius-as-a-service.com/api/status \ -H "Authorization: Bearer [your_access_token]"
```
{% endtab %}

{% tab title="Python" %}
```
import requests

url = "https://contoso.radius-as-a-service.com/api/status"
headers = {
    "Authorization": "Bearer your_access_token"
}

response = requests.get(url, headers=headers)

print(response.status_code)
print(response.text)
```
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
### Process the data according to your needs

#### Example 1 - Show information about the RadSec servers:

```
$response.radsecservers

cluster_name                     : eu1
ip                               : 20.113.8.151
name                             : radius-server-contoso-main
radius-server-contoso-main-state : True
state                            : True
```

#### Example 2 - Show information about the RADIUS proxies:

```
$response.proxies

ip                                       : 142.93.161.44
location                                 : Europe (Frankfurt)
name                                     : radius-proxy-contoso-142.93.161.44
radius-proxy-contoso-142.93.161.44-state : True
state                                    : True

ip                                     : 209.38.81.0
location                               : Australia (Sydney)
name                                   : radius-proxy-contoso-209.38.81.0
radius-proxy-contoso-209.38.81.0-state : True
state                                  : True
```

#### Example 3 - Show certificate information:

```
$response.certificates | Format-List

contoso-certificate--Proxycertificate-state : True
name                                        : contoso-certificate--Proxycertificate
state                                       : True
validity_days_left                          : 2570

contoso-certificate-Customer-CA-state : True
name                                  : contoso-certificate-Customer-CA
state                                 : True
validity_days_left                    : 6900
```

{% hint style="info" %}
Please consider that the **certificates** will be **checked every 10 hours,** and the **data** is **cached for 60 seconds**. Consequently, after renewing an expired certificate, it may take several hours for the status to be updated.
{% endhint %}
{% endstep %}
{% endstepper %}
