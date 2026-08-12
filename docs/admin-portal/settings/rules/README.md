---
description: >-
  This is the documentation of the RADIUSaaS Rule Engine, which allows you to
  add another layer of security by defining rules that further restrict network
  access requests or by assigning VLAN IDs.
---

# Rules

## General&#x20;

The Rule Engine is a second layer of security that sits behind credential authentication. Once a device or user presents a valid **certificate** (checked against your **Trusted Roots**) or a valid **Username/Password** pair, the Rule Engine decides whether that specific request is actually allowed onto the network, and what it receives in return, such as a VLAN ID or additional RADIUS attributes.

### Default rule

To avoid disruption of any existing instance or in case you do not want to use the Rule Engine at all, any authentication is allowed if no rule is defined by default. This is realized through our default rule **Any authentication allowed**.

{% hint style="warning" %}
The default rule **Any authentication allowed** still requires the presence of valid authentication credentials for a successful network authentication.
{% endhint %}

### Order of rule execution

Every incoming request is checked against your enabled rules from top to bottom. The first rule whose **medium**, **authentication method** and **filters** all match the request wins, and decides the outcome. Rules positioned below that match are never evaluated for that request.

If you have multiple rules configured, they will be applied in the order you see in your web portal - from top to bottom.&#x20;

The only exception is the **Any authentication allowed** rule, that will be handled as last step in case it is configured. This is especially helpful during a ramp-in scenario, where you might not be certain that your rules cover all use-cases or locations. All authentication request rejected by the prior rules will then still be accepted by the default rule. In the dashboard you are then able to observe the devices/users failing for all other rules and correct/extend the rules accordingly.&#x20;

In case you end up having a large number of rules, we recommend - for the sake of maintaining high performance - to order the rules in a way that the most likely rules are hit first.

## More Information

Find more details on the rules and how to use them in the following pages:

{% content-ref url="general-structure.md" %}
[general-structure.md](general-structure.md)
{% endcontent-ref %}

{% content-ref url="groups.md" %}
[groups.md](groups.md)
{% endcontent-ref %}

{% content-ref url="certificate-extensions.md" %}
[certificate-extensions.md](certificate-extensions.md)
{% endcontent-ref %}

{% content-ref url="attribute-catalogue.md" %}
[attribute-catalogue.md](attribute-catalogue.md)
{% endcontent-ref %}
