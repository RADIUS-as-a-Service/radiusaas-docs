# VPN

## Basics

1. To create a VPN rule, add an **item** under the **Rule collection** hive and select **VPN Rule.**&#x20;
2. Give the rule a **Name** that explains for what the rule is used for. Furthermore, a descriptive name will help you to identify authentication requests processed by this rule in your logs easily later on.
3. Do not forget to **Enable** the rule!

<figure><img src="../../../../.gitbook/assets/image (5).png" alt=""><figcaption><p>Showing VPN rule</p></figcaption></figure>

## **Authentication**&#x20;

Under the **Authentication** hive, your first choice is whether you want to allow or decline **Certificate-based** or **Username/Password-based** authentication for this rule.

<figure><img src="../../../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Showing VPN authntication</p></figcaption></figure>



### **Certificate-based authentication**

For certificate-based authentication you have the following choices to further constrain incoming authentication requests.

#### Allow only specific CAs (Trusted CAs)

This allows you to narrow down incoming authentication requests to specific trusted root or issuing CAs. Those CAs can be a subset of all Trusted Roots you have configured on the RADIUSaaS platform.

<figure><img src="../../../../.gitbook/assets/image (7).png" alt=""><figcaption><p>Showing certificate filtering</p></figcaption></figure>

#### Filter for Intune IDs&#x20;

This is a historical setting. If your clients are authenticating with certificates that your clients received during the AAD-Join, you want to filter for your Intune Tenant ID.&#x20;

In case you have entered your Tenant IDs as described [here](../trusted-roots.md#intune-id), the default behaviour of RADIUSaaS is that only machines presenting a certificate with extension OID **1.2.840.113556.5.14** and a whitelisted value for the Tenand ID will get access to the network. With the rule engine, you now have the option to further restrict the access to specific Intune IDs for a specific rule or to ignore the certificate extension. This allows you to have a multi-deployment setup, where some clients come with certificates providing the respective OID and some do not.&#x20;

<figure><img src="../../../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Showing Intune ID filtering</p></figcaption></figure>

### Username/Password-based authentication

After enabling **Username/Password-based** authentication, you can apply additional filtering by configuring a Regex on the **Username**. Default is all Usernames.

<figure><img src="../../../../.gitbook/assets/image (9).png" alt=""><figcaption><p>Showing username / password-based authentication</p></figcaption></figure>

## Configuration

Under the **Configuration** hive you are able to configure additional filter criteria based on the origin of authentication requests as well as return additional RADIUS attributes, e.g. Filter-Id.

### NAS Identifier filter

{% hint style="info" %}
In case you are unsure what identifier your NAS uses, you can check the logs (Logtype = detail). RADIUSaaS looks for a property called _NAS-Identifier_ to match against the trusted NAS Identifiers.&#x20;
{% endhint %}

To set a NAS Identifier filter, either select **Identifiers** or **Groups**.&#x20;

* If you select **Identifiers**, you can specify multiple **Identifiers.**
* If you select **Groups,** you can reference one or more of your pre-defined **NAS Identifier Groups**.&#x20;

<figure><img src="../../../../.gitbook/assets/image (2).png" alt=""><figcaption><p>Showing NAS Identifier</p></figcaption></figure>

### NAS IP Address filter

To set a NAS IP address filter, either select **IPs** or **Groups**.&#x20;

* If you select **IPs**, you can specify multiple **IPs.**
* If you select **Groups,** you can reference one or more of your pre-defined **NAS IP Address Groups**.

<figure><img src="../../../../.gitbook/assets/image (3).png" alt="" width="401"><figcaption><p>Showing NAS IP Address Filter</p></figcaption></figure>

### RADIUS attribute return

{% hint style="info" %}
In case you require return attributes that are not available by default, please [contact our support](https://www.radius-as-a-service.com/help/).
{% endhint %}

The RADIUSaaS rule engine provides several ways to return additional RADIUS attributes. The following options are available:

#### Static

Statically specify the return attribute(s) and their value(s) which should be assigned based on the related rule.

#### By Certificate Extension

{% hint style="info" %}
Currently it is not supported to add custom certificate extensions to SCEP profiles in many MDM systems, including Microsoft Intune and JAMF.

We therefore recommend to use the [Certificate Subject Name](vpn.md#by-certificate-subject) of the certificate instead to add a VLAN assignment.
{% endhint %}

* Select one of your created Certificate Extensions
* The filter is set to match the Value of the specified return attribute to your specified extension (OID)
* Wildcards will be translated to .\* Regex

#### By Certificate Subject Name Property

* You can also return additional RADIUS attributes based on properties in the Subject Name of your certificate
* Therefor, specify in which property the return attribute value is stored
* Then, configure which string the return attribute value is prefixed with
* The value provided in the Subject Name property is not required to have a prefix. However, it can be required to use a prefix in case your Subject Name carries the same attribute more than once (e.g. several CN's are quite common).
