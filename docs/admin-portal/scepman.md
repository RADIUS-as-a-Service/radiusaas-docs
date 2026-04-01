# SCEPman

{% hint style="info" %}
To use <code class="expression">space.vars.SCEPmanSAAS_ProductName</code>, please ensure to configure it first under **Settings** > [**SCEPman**](settings/scepman.md).
{% endhint %}

The **SCEPman** menu hive gives you access to your SCEPman SaaS instance built right into RADIUSaaS (requires a separate license).

The submenus allow you to perform manual certificate tasks leveraging the Certificate Master component. More information on how to **Manage Certificates**, **Request Certificates**, and **Tasks**, can be found in the corresponding SCEPman documentation:

{% embed url="https://docs.scepman.com/certificate-management/certificate-master" %}

## Status

The status page of your <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> instance provides you with the most relevant information that you require when configuring the service for your environment. It allows you to

* review the connectivity status to dependencies, such as the Key Vault, Graph API (Intune), or Jamf Pro,
* download the public portion of the CA certificate, or
* access the SCEP enrolment endpoint(s) that are required for configuration in your MDM system(s).

<figure><img src="../.gitbook/assets/image (498) (1).png" alt=""><figcaption></figcaption></figure>
