# Certificate Extensions

<figure><img src="../../../.gitbook/assets/image (572).png" alt=""><figcaption></figcaption></figure>

Certificate extensions let you register a field carried by your certificates so a rule can read it directly, either a private custom OID issued by your own PKI, or a standard field such as the subject, SAN or an issuer attribute. Once registered, the extension appears as a source in any rule's Certificate Attribute filters or Dynamic assignments.

{% hint style="info" %}
Currently it is not supported to add custom certificate extensions to SCEP profiles in many MDM systems, including Microsoft Intune and Jamf Pro.

We therefore recommend using the Certificate Subject Name instead to [dynamically assign VLANs](https://docs.radiusaas.com/admin-portal/settings/rules#vlan-assignment).
{% endhint %}

