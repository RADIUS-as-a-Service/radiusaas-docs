# Optional Settings

## OCSP Hard-Fail

{% hint style="danger" %}
In case you **enable** this setting, authentication requests will be **rejected** if your CA's OCSP responder is not available/reachable.
{% endhint %}

This setting determines how RADIUSaaS behaves, when the OCSP responder of your trusted CA cannot be reached.&#x20;

By default, we **recommend to disable OCSP hard-fail** to increase the availability of the service by allowing authentication requests to be accepted even if the OCSP responder cannot be reached. With this **soft-fail** mechanism, and in case OCSP is not reachable, RADIUSaaS will only check if the incoming certificate was signed by one of the [trusted CAs](../settings-trusted-roots/trusted-roots.md) (and process the optional [Rules](../rules/)).
