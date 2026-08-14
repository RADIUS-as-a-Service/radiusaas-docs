---
description: Rules allow further restrictions
---

# General Structure

<figure><img src="../../../.gitbook/assets/image (593).png" alt=""><figcaption></figcaption></figure>

## Rule Collection

{% hint style="info" %}
We recommend providing descriptive names for your rules, as this will allow them to be clearly identifiable in the Insight [Logs](../../insights/log.md).
{% endhint %}

Every Rule can have a **Name, Description** and is specified for a specific authentication type.\
Currently you can define a rule for **Wi-Fi**, **LAN** and **VPN**. Furthermore, you can **Enable** or **Disable** each rule.

The Rules tab lists every configured rule, its position, its type (Wi-Fi, LAN, VPN or Generic Allow), its **name** and **description**, the **authentication method** it accepts, the **filters** it applies to, what it **grants**, and whether it is enabled.

## Creating a Rule

Clicking **Add rule** lets you pick a medium: Generic Allow, Wi-Fi, LAN, or VPN.

Choosing **Wi-Fi**, **LAN** or **VPN** opens a guided four-step wizard. A live "in plain words" summary at the bottom of the dialog rephrases the rule in plain language as it is built, for example:

> **IN PLAIN WORDS**
>
> A Wi-Fi request authenticating with certificate only from CN=SCEPman-SaaS-CA,O=Contoso matching Cert Subject (DN) matches OU=Printers is granted VLAN 16 (static).

The following steps show the rule editor with a simple example of the above description:

{% stepper %}
{% step %}
### Identity

Just a **Name**, an optional **Description**, and an enable/disable toggle. The medium and authentication method are configured in later steps, so the name only needs to describe intent, e.g. "Corporate Wi-Fi Access" rather than encoding the SSID or VLAN into the name.

<figure><img src="../../../.gitbook/assets/image (557).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Who may authenticate (Authentication Methods)

Turn on Certificate-based and/or Username/Password-based authentication; at least one must be enabled. Enabling certificates reveals an optional **Restrict root certificates** toggle, which narrows the rule down to specific Trusted Roots instead of accepting any certificate trusted by the platform.

<figure><img src="../../../.gitbook/assets/image (558).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Where it applies (Filters)

Filters narrow the rule down and are combined with AND, meaning a request must satisfy every filter listed; leaving this step empty means the rule applies everywhere. The filters on offer depend on the medium, and each filter can take individual values or reference a reusable Group. See the Wi-Fi, LAN and VPN sections below for the specific filters each medium offers and a worked example.

<figure><img src="../../../.gitbook/assets/image (559).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### What it grants (Assignments)

Defines the access returned when the rule matches: a VLAN and/or additional RADIUS attributes, each either **Static** (a fixed value) or **Dynamic**. Dynamic assignment reads a value out of the certificate, from its Issuer, SAN, DN, or a custom Extension, applies a regex pattern to it, and maps the result to the value that gets granted. This same regex mechanism is also used for Certificate Attribute filters in Step 3. You can also define what happens if no pattern matches: reject the request, or fall back to a default value.

<figure><img src="../../../.gitbook/assets/image (560).png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

***

## Authentication Methods

The authentication methods you can define in "Who may authenticate" restricts the technical medium an authentication needs to use to be accepted.

#### Certificate-based authentication

You can either enable CBA alone to match all such authentications or further limit authentications for certificates signed by specific CAs.

<figure><img src="../../../.gitbook/assets/image (578).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
You can further restrict CBA using filters to match on the issuer, SAN, DN or extensions of the certificate used.
{% endhint %}

#### Username/Password-based authentication

Enable this method to allow authentications that do not use certificate-based authentication.

<figure><img src="../../../.gitbook/assets/image (577).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
You can further restrict username/password-based authentication using filters to match on usernames or owners.
{% endhint %}

***

## Filters

Filters can be used to build granular rules that match only under certain circumstances. There are multiple available filters that can be used in different situations:

#### Available for all Rules

* Client IPs
* Certificate Attributes
* Username/Password
* Intune IDs

#### Available for specific Rules

* SSIDs (Wi-Fi Rules)
* Switch MACs (LAN Rules)
* NAS Identitfiers (VPN Rules)
* NAS IPs (VPN Rules)

### Client IPs

Use the Client IPs filter to match a rule on the WAN-IP address of the authentications. You can use single addresses, CIDRs or a defined group.

<figure><img src="../../../.gitbook/assets/image (579).png" alt=""><figcaption></figcaption></figure>

### Certificate Attributes

Use this filter type to match a rule only if a presented client certificate contains the required information. You can filter for the following attributes:

* Issuer
* SAN (Subject Alternative Name)
* DN (Distinguished Name)
* Extension

<figure><img src="../../../.gitbook/assets/image (580).png" alt=""><figcaption></figcaption></figure>

#### Examples:

* Use a SAN filter `.*-ext@contoso\.com$` to match `john.smith-ext@contoso.com` → contractor identity cert, VLAN 10.
* Use a DN `OU=Printers` to match the subject `CN=PRINTER07,OU=Printers,O=Contoso` → assign VLAN 16 (printers).
* Use a DN `OU=Finance` to match the subject `CN=jdoe,OU=Finance,O=Contoso` → VLAN 30 (Finance).

### Username/Password

<figure><img src="../../../.gitbook/assets/image (582).png" alt=""><figcaption></figcaption></figure>

#### Examples:

* Username `^ext-.*` matches `ext-jsmith` → VLAN 50 (contractor)
* Owner `IT-AssetPool` matches shared/kiosk device tag → Filter-Id `kiosk-acl`

### SSIDs

{% hint style="info" %}
This filter type is only available on Wi-Fi rules
{% endhint %}

To filter for SSIDs, you can choose to match single SSIDs per entry or add a defined group of SSIDs.

<figure><img src="../../../.gitbook/assets/image (585).png" alt=""><figcaption></figcaption></figure>

### Switch MACs

{% hint style="info" %}
This filter type is only available on LAN rules
{% endhint %}

To filter for Switch MACs, you can choose to match single address per entry or add a defined group of Switch MACs.

<figure><img src="../../../.gitbook/assets/image (587).png" alt=""><figcaption></figcaption></figure>

### NAS-Identifiers

{% hint style="info" %}
This filter type is only available on VPN rules
{% endhint %}

Similar to other filters, you can choose to match single NAS-Identifiers per entry or add a defined group of NAS-Identifiers.

### NAS IPs

{% hint style="info" %}
This filter type is only available on VPN rules
{% endhint %}

Similar to other filters, you can choose to match single NAS-IPs per entry or add a defined group of NAS-IPs.

### Intune IDs

This is a historical filter. If your clients are authenticating with certificates that your clients received during the AAD-Join, you want to filter for your Intune Tenant ID.

In case you have entered your Tenant IDs as described [here](https://docs.radiusaas.com/admin-portal/settings/trusted-roots#intune-id), the default behaviour of RADIUSaaS is that only machines presenting a certificate with extension OID **1.2.840.113556.5.14** and a whitelisted value for the Tenant ID will get access to the network. With the rule engine, you now have the option to further restrict the access to specific Intune IDs for a specific rule or to ignore the certificate extension. This allows you to have a multi-deployment setup, where some clients come with certificates providing the respective OID and some do not.

<figure><img src="../../../.gitbook/assets/image (584).png" alt=""><figcaption></figcaption></figure>

***

## Assignments

If an authentication is valid and matches a rule, the returned Access-Accept packet can contain specific information to give further instructions to the authenticator.

### VLAN

A VLAN ID can be assigned statically, meaning that all matching authentications will receive this VLAN ID, or dynamically by extracting the desired value from either a certificate attribute or the username/owner.

#### By Certificate Subject (DN)

* You can also assign VLAN IDs based on properties in the Subject Name of your certificate
* Therefore, specify in which property the VLAN ID is stored
* Then, configure which string the VLAN ID is prefixed with
* The VLAN ID is not required to have a prefix. However, it can be required to use a prefix in case your Subject Name carries the same attribute more than once (e.g. several CN's are quite common).

As an example, the following assignment will match the DN attribute of the client certificate using the regex pattern `OU=vlan-(\d+)` and use the first matching group as resulting value.

For a certificate subject (DN) of `CN=CLIENT01,OU=vlan-15` this result in a VLAN assignment of **15**.

<figure><img src="../../../.gitbook/assets/image (594).png" alt=""><figcaption></figcaption></figure>

#### By Certificate Extension

{% hint style="info" %}
Currently it is not supported to add custom certificate extensions to SCEP profiles in many MDM systems, including Microsoft Intune and JAMF.

We therefore recommend to use the subject of the certificate instead to add a VLAN assignment.
{% endhint %}

* Select one of your created Certificate Extensions
* Choose a regex pattern to get the desired value

<figure><img src="../../../.gitbook/assets/image (588).png" alt=""><figcaption></figcaption></figure>

### Attributes

RADIUS return attributes allow network administrators to define specific settings for individual users or groups.

For example,

* For User Profile Configuration, an attribute can specify the maximum session duration, allowed services (such as VPN or Wi-Fi), and IP address assignment method.
* For Dynamic IP Address Assignment, an attribute might specify that the user should receive a static IP address or use DHCP for dynamic assignment.
* For Access Control and Authorization, an attribute determines the user’s access level (e.g., guest, employee, administrator) and any restrictions (e.g., time limits).
* For Session Management, an attribute can specify session timeout (how long the user can stay connected), idle timeout (disconnect after inactivity), and maximum simultaneous logins.
* For Quality of Service (QoS), an attribute might prioritize voice traffic over data traffic for a specific user.

Vendors can create their own custom attributes (vendor-specific attributes or VSAs). These allow for additional functionality beyond the standard IETF attributes. VSAs are encapsulated within the standard attribute 26.

In the same manner of VLAN assignments, attributes can be returned either statically or dynamically.

You can choose from different return attributes and extend them in the attribute catalogue:

{% content-ref url="attribute-catalogue.md" %}
[attribute-catalogue.md](attribute-catalogue.md)
{% endcontent-ref %}

#### Example:

Dynamically assign the value of the certificates RDN L to the the return attribute Filter-Id

<figure><img src="../../../.gitbook/assets/image (591).png" alt=""><figcaption></figcaption></figure>
