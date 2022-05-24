# FAQ

## Is the UDP-based RADIUS protocol secure?

We are recommending to use the modern RadSec protocol to authentication against RADIUSaaS. However, there are many network infrastructure components still out there, which do not support RadSec.

The following diagram shows the RADIUS authentication flow:

![](../.gitbook/assets/radius-authentication-sequence.png)

In the first authentication sequence, the communication is secured by an MD5 based hashing algorithm (partially encrypted with the shared secret). No secrets are transported in this phase.

In the second sequence, a TLS-based EAP (e.g. EAP-TLS) encrypts the traffic. The EAP-TLS traffic it tunneled in the UDP traffic. If you use certificate based authentication, no secrets are transported in this phase.

{% hint style="success" %}
Conclusion: UDP-based RADIUS authentication with RADIUSaaS is secure, since&#x20;

* the relevant traffic is encrypted\
  and
* additionally there are no secrets transported, if certificate-based authentication is used.
{% endhint %}

## Why can't I log in to the RADIUSaaS web portal?

In order to log in to the RADIUSaaS web portal ("RADIUSaas Admin Portal"), the following requirements have to be met:

* The UPN/email address you provided as technical admin has to be authenticatable against **any** Azure AD.
* The UPN/email address you provided as technical admin must have been registered on your RADIUSaaS instance as described [here](../portal/settings/permissions.md). In case it is the initial user, please [contact us](https://www.radius-as-a-service.com/help/) if you believe we registered the wrong user.
* The Azure AD user object behind the UPN/email address has to be entitled to grant the RADIUSaaS Enterprise Application the following permissions (see screenshot below):
  * **Read** the Basic User Profile
  * **Maintain** access to data you have given it access to (allow request of refresh token)\
    ![](../.gitbook/assets/Screenshot\_2022-04-11\_at\_09\_31\_26.png)
* In case your Azure AD user has no rights to grant the required permissions, no corresponding **Enterprise Application** will be auto-created in your Azure AD. To circumvent this, either ask you IT department to grant your user the needed permissions or alternatively, they may manually create the required Enterprise Application and assign your user to it.
* To manually create the Enterprise Application, please follow these steps:
  * **Create** a new Enterprise Application
  * Give it a **name** such as "RADIUSaaS Admin Center"
  * Enable **users sign-in**
  * Set the **Homepage URL** to [`https://radius-as-a-service.com`](https://radius-as-a-service.com)``
  * Optionally, apply your **Conditional Access** policies
  * Configure the following permissions (either an Admin or User consent level):\
    <img src="../.gitbook/assets/image (78) (1).png" alt="" data-size="original">
  * Under **Users and groups** assign every relevant RADIUSaaS admin that shall be able to access the platform.
