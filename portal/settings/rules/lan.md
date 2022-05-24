# LAN

## Basics

1. To create a LAN rule, add an **item** under the **Rule collection** hive **** and select **LAN Rule.**&#x20;
2. Give the rule a **Name** that explains for what the rule is used for. Furthermore, a descriptive name will help you to identify authentication requests processed by this rule in your logs easily later on.
3. Do not forget to **Enable** the rule!

![](<../../../.gitbook/assets/image (65) (1) (1).png>)

## **Authentication**&#x20;

Under the **Authentication** hive, your first choice is whether you want to allow or decline **Certificate-based** or **Username/Password-based** authentication for this rule.

![](<../../../.gitbook/assets/image (70).png>)

### **Certificate-based Authentication**

For certificate-based authentication you have the following choices to further constrain incoming authentication requests.

#### Allow only specific CAs (Trusted Roots)

This allows you to narrow down incoming authentication requests to specific trusted root CAs. Those CAs can be a subset of all Trusted Roots you have configured on the RADIUSaaS platform.

![](<../../../.gitbook/assets/image (62) (1) (1).png>)

#### Filter for Intune IDs&#x20;

This is a historical setting. If your clients are authenticating with certificates that your clients received during the AAD-Join, you want to filter for your Intune Tenant ID.&#x20;

In case you have entered your Tenant IDs as described [here](../settings-trusted-roots/intune-cert.md#configure-intune-ids), the default behaviour of RADIUSaaS is that only machines presenting a certificate with extension OID **1.2.840.113556.5.14** and a whitelisted value for the Tenand ID will get access to the network. With the rule engine, you now have the option to further restrict the access to specific Intune IDs for a specific rule or to ignore the certificate extension. This allows you to have a multi-deployment setup, where some clients come with certificates providing the respective OID and some do not.&#x20;

![](<../../../.gitbook/assets/image (77) (1) (1) (1).png>)

### Username/Password-based Authentication

After enabling **Username/Password-based** authentication, you can apply additional filtering by configuring a Regex on the **Username**. Default is all Usernames

The **Owner** field is currently not used.

&#x20;

![](<../../../.gitbook/assets/image (74) (1).png>)

## Configuration

Under the **Configuration** hive you are able to configure additional filter criteria based on the origin of authentication requests as well as assign VLAN IDs.

### Switch Filter

To set a **MAC-Address-based** switch filter, either select **Addresses** or **Groups**.&#x20;

* If you select **Addresses**, you can specify multiple switch MAC addresses**.**&#x20;
* If you select **Groups,** you can reference one or more of your pre-defined **MAC-Address-Groups**.&#x20;

![](<../../../.gitbook/assets/image (72).png>)

### VLAN assignment

The RADIUSaaS rule engine provides several ways to assign Virtual-LAN IDs. The following options are available:

*   **Static**

    * Statically specify the VLAN ID which should be assigned based on the related rule


*   **By Certificate Extension**

    * Select one of your created **Certificate Extensions**
    * The filter is set to match the **Value** to your specified extension (OID).&#x20;
    * Wildcards will be translated to .\* Regex


* **By Certificate Subject Name**
  * You can also assign VLAN IDs based on attributes in the **Subject Name** of your certificate
  * Therefor, specify in which attribute the VLAN ID is stored
  * Then, configure which type the VLAN ID is prefixed with
  * The VLAN ID is not required to have a prefix. However, it can be required to use a prefix in case your Subject Name carries the same attribute more than once (e.g. several CN's are quite common).

As an example, the following rule will assign the VLAN ID 15 based on the Subject Name attribute **OU** prefixed with **vlan-**.

![](<../../../.gitbook/assets/image (78) (1) (1) (1).png>)

![](<../../../.gitbook/assets/image (67) (1) (1).png>)
