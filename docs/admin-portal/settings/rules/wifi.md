# WiFi

## Basics

1. To create a WiFi rule, add an **item** under the **Rule collection** hive and select **WiFi Rule.**&#x20;
2. Give the rule a **Name** that explains for what the rule is used for. Furthermore, a descriptive name will help you to identify authentication requests processed by this rule in your logs easily later on.
3. Do not forget to **Enable** the rule!

<figure><img src="../../../.gitbook/assets/image (36).png" alt="" width="401"><figcaption><p>Showing how to add a Wi-Fi rule</p></figcaption></figure>

## **Authentication**&#x20;

Under the **Authentication** hive, your first choice is whether you want to allow or decline **Certificate-based** or **Username/Password-based** authentication for this rule.

<figure><img src="../../../.gitbook/assets/image (37).png" alt=""><figcaption><p>Showing authentication</p></figcaption></figure>

### **Certificate-based authentication**

For certificate-based authentication you have the following choices to further constrain incoming authentication requests.

#### Allow only specific CAs (Trusted CAs)

This allows you to narrow down incoming authentication requests to specific trusted root or issuing CAs. Those CAs can be a subset of all Trusted Roots you have configured on the RADIUSaaS platform.

<figure><img src="../../../.gitbook/assets/image (40).png" alt=""><figcaption><p>Showing certificate filtering</p></figcaption></figure>

#### Filter for Intune IDs&#x20;

This is a historical setting. If your clients are authenticating with certificates that your clients received during the AAD-Join, you want to filter for your Intune Tenant ID.&#x20;

In case you have entered your Tenant IDs as described [here](../trusted-roots.md#intune-id), the default behaviour of RADIUSaaS is that only machines presenting a certificate with extension OID **1.2.840.113556.5.14** and a whitelisted value for the Tenand ID will get access to the network. With the rule engine, you now have the option to further restrict the access to specific Intune IDs for a specific rule or to ignore the certificate extension. This allows you to have a multi-deployment setup, where some clients come with certificates providing the respective OID and some do not.&#x20;

<figure><img src="../../../.gitbook/assets/image (34).png" alt=""><figcaption><p>Showing filtering by Intune ID</p></figcaption></figure>

### Username/Password-based authentication

After enabling **Username/Password-based** authentication, you can apply additional filtering by configuring a Regex on the **Username**. Default is all Usernames

<figure><img src="../../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

## Configuration

Under the **Configuration** hive you are able to configure additional filter criteria based on the origin of authentication requests as well as assign VLAN IDs.

### SSID filter

To set an SSID filter, either select **Names** or **Groups**.&#x20;

* If you select **Names**, you can specify multiple **SSIDs.**
* If you select **Groups,** you can reference one or more of your pre-defined **SSID Groups**.&#x20;

<figure><img src="../../../.gitbook/assets/image (32).png" alt=""><figcaption><p>Showing filter by SSID name or group</p></figcaption></figure>

### Access Point filter

{% hint style="info" %}
This MAC address filter allows you to permit specific **access points** to communicate with RADIUSaaS. **This is not a MAC address filter for endpoints!**
{% endhint %}

To set a **MAC-Address-based** access point filter, either select **Addresses** or **Groups**.&#x20;

* If you select **Addresses**, you can specify multiple Access Point MAC addresse&#x73;**.**&#x20;
* If you select **Groups,** you can reference one or more of your pre-defined **MAC Address Groups**.&#x20;

<figure><img src="../../../.gitbook/assets/image (45).png" alt="" width="402"><figcaption><p>Showing Access Point selection by MAC addresses or group</p></figcaption></figure>

The following notations are supported for the MAC addresses:

* xx-xx-xx-xx-xx-xx
* xx:xx:xx:xx:xx:xx
* xxxxxxxxxxxx

{% hint style="info" %}
Your access point (AP) will most probably not use the LAN MAC address, that is printed on the AP housing and showed in the AP admin portal as primary MAC address. Many vendors generate individual/random MAC addresses (BSSIDs) for every single SSID and frequency.

If you can not find the used MAC addresses in your AP admin interface, you can also consult the "Rule Engine" log (in the "Insights" section) in your RADIUSaaS admin web console. You find the MAC address that your AP is sending in the field "`AP-MAC"`.
{% endhint %}

### VLAN assignment

The RADIUSaaS rule engine provides several ways to assign Virtual-LAN IDs. The following options are available:

#### Static

* Statically specify the VLAN ID which should be assigned based on the related rule

#### By Certificate Extension

{% hint style="info" %}
Currently it is not supported to add custom certificate extensions to SCEP profiles in many MDM systems, including Microsoft Intune and JAMF.

We therefore recommend to use the [Certificate Subject Name](wifi.md#by-certificate-subject) of the certificate instead to add a VLAN assignment.
{% endhint %}

* Select one of your created Certificate Extensions
* The filter is set to match the Value to your specified extension (OID)
* Wildcards will be translated to. \* Regex

<figure><img src="../../../.gitbook/assets/image (28).png" alt="" width="402"><figcaption></figcaption></figure>

#### By Certificate Subject Name Property

* You can also assign VLAN IDs based on properties in the Subject Name of your certificate
* Therefore, specify in which property the VLAN ID is stored
* Then, configure which string the VLAN ID is prefixed with
* The VLAN ID is not required to have a prefix. However, it can be required to use a prefix in case your Subject Name carries the same attribute more than once (e.g. several CN's are quite common).

As an example, the following rule will assign the VLAN ID 15 based on the `Subject Name` attribute `OU` prefixed with `vlan-`

<figure><img src="../../../.gitbook/assets/image (30).png" alt="" width="403"><figcaption><p>Showing VLAN filtering</p></figcaption></figure>

![](<../../../.gitbook/assets/image (335).png>)

### Additional RADIUS attributes

{% hint style="info" %}
In case you require return attributes that are not available by default, please [contact our support](https://www.radius-as-a-service.com/help/).
{% endhint %}

The RADIUSaaS rule engine provides several ways to return additional RADIUS attributes (besides the VLAN ID). The following options are available:

#### Static

Statically specify the return attribute(s) and their value(s) which should be assigned based on the related rule.

#### By Certificate Extension

{% hint style="info" %}
Currently it is not supported to add custom certificate extensions to SCEP profiles in many MDM systems, including Microsoft Intune and JAMF.

We therefore recommend to use the [Certificate Subject Name](wifi.md#by-certificate-subject) of the certificate instead to add a VLAN assignment.
{% endhint %}

* Select one of your created Certificate Extensions
* The filter is set to match the Value of the specified return attribute to your specified extension (OID)
* Wildcards will be translated to .\* Regex

#### By Certificate Subject Name Property

* You can also return additional RADIUS attributes based on properties in the Subject Name of your certificate
* Therefor, specify in which property the return attribute value is stored
* Then, configure which string the return attribute value is prefixed with
* The value provided in the Subject Name property is not required to have a prefix. However, it can be required to use a prefix in case your Subject Name carries the same attribute more than once (e.g. several CN's are quite common).
