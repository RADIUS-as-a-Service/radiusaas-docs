# LAN

## Basics

1. To create a LAN rule, add an **item** under the **Rule collection** hive and select **LAN Rule.**&#x20;
2. Give the rule a **Name** that explains for what the rule is used for. Furthermore, a descriptive name will help you to identify authentication requests processed by this rule in your logs easily later on.
3. Do not forget to **Enable** the rule!

<figure><img src="../../../../.gitbook/assets/image (436).png" alt=""><figcaption></figcaption></figure>

## **Authentication**&#x20;

Under the **Authentication** hive, your first choice is whether you want to allow or decline **Certificate-based** or **Username/Password-based** authentication for this rule.

<figure><img src="../../../../.gitbook/assets/image (437).png" alt=""><figcaption><p>Showing LAN authentication</p></figcaption></figure>

### **Certificate-based authentication**

For certificate-based authentication you have the following choices to further constrain incoming authentication requests.

#### Allow only specific CAs (Trusted CAs)

This allows you to narrow down incoming authentication requests to specific trusted root or issuing CAs. Those CAs can be a subset of all Trusted Roots you have configured on the RADIUSaaS platform.

<figure><img src="../../../../.gitbook/assets/image (438).png" alt=""><figcaption><p>Showing Trusted Root CA filtering</p></figcaption></figure>

#### Filter for Intune IDs&#x20;

This is a historical setting. If your clients are authenticating with certificates that your clients received during the AAD-Join, you want to filter for your Intune Tenant ID.&#x20;

In case you have entered your Tenant IDs as described [here](../trusted-roots.md#intune-id), the default behaviour of RADIUSaaS is that only machines presenting a certificate with extension OID **1.2.840.113556.5.14** and a whitelisted value for the Tenand ID will get access to the network. With the rule engine, you now have the option to further restrict the access to specific Intune IDs for a specific rule or to ignore the certificate extension. This allows you to have a multi-deployment setup, where some clients come with certificates providing the respective OID and some do not.&#x20;

<figure><img src="../../../../.gitbook/assets/image (439).png" alt=""><figcaption><p>Showing Intune ID filtering</p></figcaption></figure>

### Username/Password-based authentication

After enabling **Username/Password-based** authentication, you can apply additional filtering by configuring a Regex on the **Username**. Default is all Usernames.

<figure><img src="../../../../.gitbook/assets/image (440).png" alt=""><figcaption><p>Showing Username / Password-based authentication</p></figcaption></figure>

## Configuration

Under the **Configuration** hive you are able to configure additional filter criteria based on the origin of authentication requests as well as assign VLAN IDs.

### Switch filter

{% hint style="info" %}
This MAC address filter allows you to permit specific **switches** to communicate with RADIUSaaS. **This is not a MAC address filter for endpoints!**
{% endhint %}

To set a **MAC-Address-based** switch filter, either select **Addresses** or **Groups**.&#x20;

* If you select **Addresses**, you can specify multiple switch MAC addresse&#x73;**.**&#x20;
* If you select **Groups,** you can reference one or more of your pre-defined **MAC Address Groups**.&#x20;

<figure><img src="../../../../.gitbook/assets/image (442).png" alt=""><figcaption><p>Showing MAC address filtering</p></figcaption></figure>

The following notations are supported for the MAC addresses:

* xx-xx-xx-xx-xx-xx
* xx:xx:xx:xx:xx:xx
* xxxxxxxxxxxx

### VLAN assignment

The RADIUSaaS rule engine provides several ways to assign Virtual-LAN IDs. The following options are available:

#### Static

* Statically specify the VLAN ID which should be assigned based on the related rule

#### By Certificate Extension

{% hint style="info" %}
Currently it is not supported to add custom certificate extensions to SCEP profiles in many MDM systems, including Microsoft Intune and JAMF.

We therefore recommend to use the [Certificate Subject Name](lan.md#by-certificate-subject-name) of the certificate instead to add a VLAN assignment.
{% endhint %}

* Select one of your created Certificate Extensions
* The filter is set to match the Value to your specified extension (OID)
* Wildcards will be translated to .\* Regex

#### By Certificate Subject Name Property

You can also assign VLAN IDs based on properties in the Subject Name of your certificate. For example, if you wanted to assign VLAN 15 in your Rules and you are using Intune to define and deploy your SCEP profile, you will need to configure the **Subject name format** in your SCEP profile such as `CN={{DeviceId}},`**`OU=vlan-15`**

<figure><img src="../../../.gitbook/assets/image (13) (1).png" alt=""><figcaption><p>Showing VLAN ID configuration in SCEP Device Certificate</p></figcaption></figure>

Once the profile is deployed, go back to **RADIUSaaS** > **Rules** and specify in which property the VLAN ID is stored and configure the string the VLAN ID is prefixed with. e.g. `vlan-`

{% hint style="info" %}
The VLAN ID is not required to have a prefix. However, it can be useful in case your Subject Name carries the same attribute more than once (e.g. several CN's are quite common).
{% endhint %}

As an example, the following rule will assign the VLAN ID 15 based on the `Subject Name` attribute `OU` prefixed with `vlan-`

<figure><img src="../../../../.gitbook/assets/image (443).png" alt=""><figcaption><p>Showing VLAN filtering</p></figcaption></figure>

![](<../../../../.gitbook/assets/image (317).png>)

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

We therefore recommend to use the [Certificate Subject Name](lan.md#by-certificate-subject) of the certificate instead to add a VLAN assignment.
{% endhint %}

* Select one of your created Certificate Extensions
* The filter is set to match the Value of the specified return attribute to your specified extension (OID)
* Wildcards will be translated to .\* Regex

#### By Certificate Subject Name Property

* You can also return additional RADIUS attributes based on properties in the Subject Name of your certificate
* Therefor, specify in which property the return attribute value is stored
* Then, configure which string the return attribute value is prefixed with
* The value provided in the Subject Name property is not required to have a prefix. However, it can be required to use a prefix in case your Subject Name carries the same attribute more than once (e.g. several CN's are quite common).
