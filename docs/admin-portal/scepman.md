# SCEPman

{% hint style="info" %}
To use SCEPman-as-a-Service, please ensure to configure it first under Settings > [SCEPman](settings/scepman.md).
{% endhint %}

The **SCEPman** menu hive gives you access to your SCEPman-as-a-Service instance built right into RADIUSaaS (requires SCEPman Enterprise Edition).

The submenus allow you to perform manual certificate tasks leveraging the Certificate Master component. More information on how to **Manage Certificates**, **Request Certificates**, and **Tasks**, can be found in the corresponding SCEPman documentation:

{% embed url="https://docs.scepman.com/certificate-management/certificate-master" %}

## Status

The status page of your SCEPman-as-a-Service instance provides you with the most relevant information that you require when configuring the service for your environment. It allows you to

* review the connectivity status to dependencies, such as the KeyVault, Graph API (Intune), or Jamf Pro,
* download the public portion of the CA certificate, or
* access the SCEP enrolment endpoint(s) that are required for configuration in your MDM system(s).

<figure><img src="../.gitbook/assets/image (498).png" alt=""><figcaption></figcaption></figure>
