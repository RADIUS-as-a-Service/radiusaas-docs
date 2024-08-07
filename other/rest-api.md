---
description: >-
  The REST API documentation is available under
  https://YOURNAME.radius-as-a-service.com/docs/api
---

# REST API

RADIUSaaS exposes a REST API that allows you to automate most actions that would otherwise have to be performed through the RADIUSaaS Admin Portal UI.

{% hint style="warning" %}
Before executing any API call that leads to a configuration change, ensure you fully understand the implications. Incorrect use of the API may break the configuration of your service.
{% endhint %}

## Authentication

To authenticate a call to the REST API, populate an HTTP `Authorization` header with each request. This header must contain a valid [access token](../admin-portal/settings/permissions.md#access-tokens):

```
Authorization: Bearer
               <Access Token>
```

## API Reference

The API documentation contains a complete Swagger-based API reference for each API endpoint including

* available HTTP methods,
* HTTP response codes,
* JSON schemas / form data for request (bodies),
* JSON schemas of response bodies, and
* Request-response examples for some endpoints.

{% hint style="info" %}
It is not possible to trigger API calls directly through the API Reference.
{% endhint %}

<figure><img src="../.gitbook/assets/image (425).png" alt=""><figcaption></figcaption></figure>

## Scenarios

### Manage Username/Password Accounts for BYOD or Guest Access

The REST API can be used to automate the management of username/password accounts for BYOD or guest access scenarios.&#x20;

This may include the automatic provisioning of (WiFi) credentials during on-boarding of new students, tenants in a co-working space, ... as well as the automatic retirement of those accounts.

An example on how to use the REST API to provision a username/password account can be found in your API Reference under the **User** endpoints.

## Curl Examples

In general, there are two different content types for the REST API, either form data or JSON. You can find out which media type is required in the API documentation.&#x20;

If you are not familiar with the curl syntax, you can find two examples here:

#### JSON

```bash
curl -X "METHOD" "https://YOURNAME.radius-as-a-service.com/api/ROUTE/PATH" \
    -H 'Authorization: Bearer ACCESS_TOKEN' \
    -H 'Content-Type: application/json' \
    -d $'{
            NEEDED JSON DATA. Have a look at the documentation
        }'
```

#### Form Data

```bash
curl -X "METHOD" "https://YOURNAME.radius-as-a-service.com/api/ROUTE/PATH" \
    -H 'Authorization: Bearer ACCESS_TOKEN' \
    -F 'KEY=VALUE'\
    -F 'KEY=VALUE'
```
