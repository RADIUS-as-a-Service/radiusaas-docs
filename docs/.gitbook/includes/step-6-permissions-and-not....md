---
title: 'Step 6: Permissions and Not...'
---

# Step 6: Permissions and Notification Contacts

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

First, review your [Permissions](../../admin-portal/settings/permissions.md) to ensure the right persons in your organization have the right level of administrative access to your RADIUSaaS instance.

{% hint style="success" %}
To **prevent yourself from being locked** out of your RADIUSaaS instance, always ensure that either

* at least two user identities or
* one service account

are configured as [Administrators](../../admin-portal/settings/permissions.md#administrators).
{% endhint %}

Next, ensure that we are able to contact you in case we have important technical information to share by reviewing the [Technical Contacts](../../admin-portal/settings/permissions.md#technical-contacts) section.

{% hint style="success" %}
For us to **reliably deliver important information** to you via email, always ensure that either

* at least two email addresses of individuals or
* one shared mailbox / distribution list

are configured.
{% endhint %}

# Step 7: Rules

{% hint style="info" %}
This is an **optional** step.
{% endhint %}

If you would like to configure additional rules, for example to assign VLAN IDs or limit authentication requests to certain trusted CAs or WiFi access points, please check out the RADIUSaaS Rule Engine.

{% content-ref url="../../admin-portal/settings/rules/" %}
[rules](../../admin-portal/settings/rules/)
{% endcontent-ref %}
