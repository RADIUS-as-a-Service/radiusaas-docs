# General Structure

## Rule Collection

{% hint style="info" %}
We recommend to provide descriptive names for your rules, as this will allow them to be clearly identifiable in the Log area.
{% endhint %}

Every Rule can have a **Name, Description** and is specified for a specific authentication type.\
Currently you can define a rule for **WiFi** or **LAN**. Furthermore, you can **Enable** or **Disable** your rule.

![](<../../../.gitbook/assets/image (65).png>)

## SSID, Access Points and Switch MAC Groups

To restrict the access to only allow authentication requests originating from specific infrastructure items such as access points, SSIDs or network switches, you have two options:&#x20;

1. Add the respective **MAC** **address(es)** or **SSID(s)** directly in the Rule collection
2. Create Groups that allow you to add multiple targets and manage them more efficiently. This way, items can be added or removed without the need to touch the Rule itself, as the Rule will only reference the Group.&#x20;

![](<../../../.gitbook/assets/image (83).png>)

## Custom Certificate Extensions

If you have your own PKI and want to assign VLAN IDs based on the value of a custom certificate extension (OID), you can make that mapping information available to RADIUSaaS under **Custom Certificate Extensions.** Once you have specified such a custom extension, you can reference it in any rule and assign VLANs based on the raw or filtered extension value.

{% hint style="info" %}
Currently it is not supported to add custom certificate extensions to SCEP profiles in many MDM systems, including Microsoft Intune and JAMF.

We therefore recommend to use the Certificate Subject Name of the certificate instead to add a VLAN assignment.
{% endhint %}

![](<../../../.gitbook/assets/image (86).png>)
