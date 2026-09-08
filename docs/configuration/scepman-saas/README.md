# 🆕 SCEPman SaaS

<code class="expression">space.vars.SCEPmanSAAS_ProductName</code> is a fully managed, hosted and maintained SCEPman deployment. It is included with the RADIUSaaS & <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> Bundle, which provides both services under one subscription. Use it to issue certificates for certificate-based authentication with RADIUSaaS, without managing a SCEPman instance. This guide covers the initial configuration for an **Intune** deployment.

## Setup <code class="expression">space.vars.SCEPmanSAAS_ProductName</code>

{% hint style="info" %}
If you are already using an existing SCEPman Enterprise deployment in your tenant with RADIUSaaS, make sure to have a look at our migration guide: [migrate-from-scepman-enterprise.md](migrate-from-scepman-enterprise.md "mention")
{% endhint %}

{% stepper %}
{% step %}
### Enroll the <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> CA

With an enabled <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> license, you will see that the menu section in **SCEPman** > **Settings** contains options to enroll and configure your SCEPman CA.

At the top you have the ability to choose the **Common Name** as well as the **Organization** name for your CA.

<figure><img src="../../.gitbook/assets/image (620).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The Common Name and Organization will form the subject of the CA certificate during the enrollment.
{% endhint %}

By clicking **Enroll root CA**, the setup will start and the status is shown above. Once finished, the SCEPman section will contain additional pages.

{% tabs %}
{% tab title="Status" %}
The **Status** page shows the current state of the CA and its integrations as well as the endpoint URLs you need to request certificates.

<figure><img src="../../.gitbook/assets/image (614).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Manage Certificates" %}
Under **Manage Certificates** you can browse through issued certificates, check their validity, and also have the option to revoke them.

<figure><img src="../../.gitbook/assets/image (617).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Request Certificates" %}
Under **Request Certificates**, you can request different types of certificates for manual enrollment and installation or sign CSRs.

{% hint style="info" %}
Have a look at the dedicated [Certificate Master](https://docs.scepman.com/certificate-management/certificate-master) documentation for more information on the different types of certificates you can request here.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (618).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Tasks" %}
The **Tasks** section shows the current status of the Certificate Master and links to sections permitted by your role.

<figure><img src="../../.gitbook/assets/image (619).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

{% hint style="success" %}
SCEPman is now deployed and ready to issue certificates!
{% endhint %}
{% endstep %}

{% step %}
### Configure the default Certificate Profile

The default settings applied to certificates issued through all certificate endpoints. Each source under Certificate endpoints can override these two values with its own profile.

Revocation for every certificate this CA issues is configured here as well.

<details>

<summary>Default Certificate Profile Settings</summary>

**Default Extended Key Usage**

What the certificate may be used for. Server certificates need `ServerAuthentication`; device certificates for 802.1X need `ClientAuthentication`. This is only the fallback in case the request does not contain an EKU.

**Validity Period**

The maximum number of days that an issued certificate is valid. [Learn more](https://docs.scepman.com/scepman-configuration/application-settings/certificates#appconfig-validityperioddays).

**Certificate Revocation List (CRL)**

Publishes a signed list of revoked certificates for clients that don't support OCSP. The distribution point is embedded in every certificate issued from the moment you enable it. [Learn more](https://docs.scepman.com/certificate-management/manage-certificates/enabling-crl).

**OCSP Authorised Responder**

Answers revocation live over OCSP, signed by a dedicated responder certificate. Always enabled for SCEPman SaaS. [Learn more](https://docs.scepman.com/certificate-management/manage-certificates).

</details>

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Enable Certificate Endpoints

Enable the certificate endpoints relevant to your tenant. Switching the endpoints on will reveal additional settings to configure. For more information on each endpoint, see [here](../../admin-portal/scepman-saas/settings.md#certificate-endpoints).

{% hint style="warning" %}
Microsoft Intune and Static challenge + Entra device check endpoints require Step 4 to be done first.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Connect SCEPman to your Azure Tenant

{% hint style="info" %}
You will only need to connect SCEPman to your Azure tenant if you plan to deploy certificate through Intune or the Static-AAD endpoint.
{% endhint %}

In most scenarios you will want SCEPman to be able to issue certificates by using Intune SCEP profiles and also revoke certificates automatically if a device has been wiped for example.

For this to work as intended, SCEPman requires specific roles in your tenant. This can either happen by consenting to our multi-tenant enterprise application or by providing an app registration holding the required permissions yourself.

#### Confirm Tenant

{% hint style="info" %}
We recommend the Admin Consent / multi-tenant enterprise application approach to connect to your Azure tenant, since it does not require a client secret that must be monitored for expiration.
{% endhint %}

The first step of the **Admin Consent** flow is to enter your tenant ID and confirming it.

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Upon clicking **Confirm Tenant** you will be redirected to Microsoft's consent page for authentication and to approve this application initially:

<figure><img src="../../.gitbook/assets/image (13) (1).png" alt=""><figcaption></figcaption></figure>

Accepting this consent will add the <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> enterprise application to your tenant but does not yet add the required permissions.

#### Consent Admin

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

After you confirm the tenant, select **Consent Admin**. Microsoft's consent page opens again. Confirm that the application can receive the listed permissions. Learn more in the Security & Privacy Q\&As.

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
SCEPman is now able to connect to your Azure tenant and retrieve information for certificate binding!
{% endhint %}
{% endstep %}

{% step %}
### Enable the Intune Endpoint

In most scenarios, certificates will be deployed by leveraging Intune SCEP certificate profiles to trigger devices to request certificates from SCEPman. To enable this endpoint, navigate to **SCEPman** > **Settings** and enable the **Intune Validation** setting and save the configuration:

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

#### Compliance Check

As with SCEPman Enterprise, SCEPman can evaluate the validity of a certificate by checking the compliance state of a bound device. Please refer to the [SCEPman documentation](https://docs.scepman.com/scepman-configuration/application-settings/scep-endpoints/intune-validation#appconfig-intunevalidation-compliancecheck) for more information.

#### Device Directory

Selecting the device directory depends on the specific binding you choose in the SCEP profile:

* `{{DeviceId}}` will be looked up in **Intune**
* `{{AAD_DeviceID}}` will be looked up in **AAD** (Entra ID)
* `{{UserPrincipalName}}` (UPN) will be looked up in **AAD** (Entra ID)

Please refer to the [SCEPman documentation](https://docs.scepman.com/scepman-configuration/application-settings/scep-endpoints/intune-validation#appconfig-intunevalidation-devicedirectory) for more details on the device directories.

{% hint style="success" %}
The Intune validation is now enabled and its certificate endpoint is available!
{% endhint %}
{% endstep %}

{% step %}
### Deploy Certificates

With the Intune validation enabled, you will find that the SCEPman status page now shows an endpoint URL for the Intune MDM:

<figure><img src="../../.gitbook/assets/image (615).png" alt=""><figcaption></figcaption></figure>

This URL will be used in the Intune SCEP certificate profile for the **SCEP Server URL**.

#### Root Certificate

Make sure to create a **Trusted Certificate** profile in Intune before continuing to the **SCEP certificate** profile and deploy the CA certificate of <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> to your clients.

[SCEPman documentation: Root Certificate](https://docs.scepman.com/certificate-management/microsoft-intune/windows-10#root-certificate)

<figure><img src="../../.gitbook/assets/image (510).png" alt=""><figcaption></figcaption></figure>

#### SCEP Certificate

The process of creating the SCEP certificate profile is identical to SCEPman Enterprise.

[SCEPman documentation: SCEP - Intune - Windows](https://docs.scepman.com/certificate-management/microsoft-intune/windows-10)

<figure><img src="../../.gitbook/assets/image (509).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Your clients should now receive certificates issued by SCEPman!
{% endhint %}
{% endstep %}
{% endstepper %}

## Establish Trust

To allow devices to authenticate using certificates from your <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> CA and enabling your access points to establish RadSec connections to your RADIUSaaS instance, you will need to trust its CA certificate. To do this, first download your CA certificate from the **SCEPman** > **Status** page.

<figure><img src="../../.gitbook/assets/image (501).png" alt=""><figcaption></figcaption></figure>

Having the CA certificate in place, navigate to **Settings** > **Trusted Certificates** and add a new certificate.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Upload your downloaded certificate file and save:

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
RADIUSaaS will now accept accept incoming connections that use certificates issued by SCEPman!
{% endhint %}

## Enable Management of the RADIUSaaS Server Certificate

After you have enrolled <code class="expression">space.vars.SCEPmanSAAS_ProductName</code>, you will notice that the SCEPman Connection section under **Connectivity** > **SCEPman** allows you to pregenerate a certificate and setup a connection.

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

A connection to your SCEPman instance has already been established at this point and RADIUSaaS can request server certificates. The correct way of going further now depends on if you already use RADIUSaaS to authenticate clients at this point or if this is a fresh setup.

#### Pregenerate Certificate

RADIUSaaS will request a server certificate and add it to the list of certificates but **will not activate it or enable the automatic management.**

In case you currently have clients authenticating to RADIUSaaS, this allows you to verify that your Wifi profile has the correct names for server validation as well as the correct root certificate for server validation.

<figure><img src="../../.gitbook/assets/image (506).png" alt=""><figcaption></figcaption></figure>

#### Setup Connection

If this is a fresh setup or after you have verified that your clients use the correct information for validating the server certificate, you can enable the automatic management of the server certificate by clicking **Setup Connection**.

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
RADIUSaaS will now request a server certificate from SCEPman, activate it, and renew it in time before it expires!
{% endhint %}

## Other Certificate Endpoints

Setting up other certificate endpoints is similar to the way they are set up with SCEPman Enterprise. Make sure to take a look at the relevant documentation:

#### Jamf

{% embed url="https://docs.scepman.com/certificate-management/jamf/general" %}

#### REST Enrollment API

{% hint style="info" %}
The SCEPman SaaS variant of the REST Enrollment API uses RADIUSaaS access tokens instead of Entra access tokens.
{% endhint %}

{% embed url="https://docs.scepman.com/certificate-management/api-certificates" %}

#### Active Directory Validation

{% embed url="https://docs.scepman.com/certificate-management/active-directory" %}

#### Static Validation

{% embed url="https://docs.scepman.com/scepman-configuration/application-settings/scep-endpoints/static-validation" %}

#### Static-AAD Validation

{% embed url="https://docs.scepman.com/scepman-configuration/application-settings/scep-endpoints/staticaad-validation" %}

#### DC

{% embed url="https://docs.scepman.com/scepman-configuration/application-settings/scep-endpoints/dc-validation" %}

## Logs

You can find all application logs that you would expect in SCEPman Enterprise in the **Logs** section. These include:

* Service Health Messages
* Issued Certificates
* OCSP Responses
* Warnings and Errors during Validation and Issuance

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>
