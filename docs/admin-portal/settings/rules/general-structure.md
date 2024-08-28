---
description: Rules allow further restriction
---

# General Structure

## Rule Collection

{% hint style="info" %}
We recommend providing descriptive names for your rules, as this will allow them to be clearly identifiable in the [Log](../../insights/log.md) area.
{% endhint %}

Every Rule can have a **Name, Description** and is specified for a specific authentication type.\
Currently you can define a rule for **Wi-Fi**, **LAN** and **VPN**. Furthermore, you can **Enable** or **Disable** each rule.

<figure><img src="../../../../.gitbook/assets/image (389).png" alt=""><figcaption><p>Showing various rules</p></figcaption></figure>

## SSID, Access Point (AP) and Switch MAC Groups

{% hint style="info" %}
For **Wi-Fi** and **Wired/LAN** networks only!
{% endhint %}

To restrict access from specific infrastructure items only such as access points, SSIDs or network switches, you have two options:&#x20;

1. Add the respective **MAC** **address(es)** or **SSID(s)** directly in the Rule collection.
2. Create **Groups** that allow you to add multiple targets and manage them more efficiently. This way, items can be added or removed without the need to touch the Rule itself, as the Rule will only reference the Group.&#x20;

<figure><img src="../../../../.gitbook/assets/image (28).png" alt="" width="277"><figcaption><p>Showing how to add an SSID group</p></figcaption></figure>

## Trusted NAS Identifiers and IP Addresses

{% hint style="info" %}
For **VPN** networks only!
{% endhint %}

When using RADIUSaaS for authenticating a VPN, the authentication requests can be limited to certain Network Access Controllers (NAS) by allow-listing their identifier or IP address.&#x20;

1. Add the respective **NAS Identifier(s)** or **NAS IP Address(es)** directly in the Rule collection
2. Create Groups that allow you to add multiple targets and manage them more efficiently. This way, items can be added or removed without the need to touch the Rule itself, as the Rule will only reference the Group.&#x20;

<figure><img src="../../../../.gitbook/assets/image (30).png" alt="" width="278"><figcaption></figcaption></figure>

## Custom Certificate Extensions

If you have your own PKI and want to assign VLAN IDs based on the value of a custom certificate extension (OID), you can make that mapping information available to RADIUSaaS under **Custom Certificate Extensions.** Once you have specified such a custom extension, you can reference it in any rule and assign VLANs based on the raw or filtered extension value.

{% hint style="info" %}
Currently it is not supported to add custom certificate extensions to SCEP profiles in many MDM systems, including Microsoft Intune and Jamf Pro.

We therefore recommend using the Certificate Subject Name instead to [dynamically assign VLANs](./#vlan-assignment).
{% endhint %}

![Showing VLAN assignment by Certificate Extension](../../../../.gitbook/assets/2024-05-28\_10h41\_47.png)

<figure><img src="../../../../.gitbook/assets/image (21).png" alt=""><figcaption><p>Showing a Certificate Extension</p></figcaption></figure>
