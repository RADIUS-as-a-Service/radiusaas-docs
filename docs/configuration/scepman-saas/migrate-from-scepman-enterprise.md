# Migrate from SCEPman Enterprise

If you are currently using SCEPman Enterprise in your own tenant and want to further reduce your infrastructure footprint you might want to consider moving to <code class="expression">space.vars.SCEPmanSAAS_ProductName</code>, a fully integrated CA solution built into RADIUSaaS.

### What will change?

<table data-full-width="false"><thead><tr><th>What</th><th>Changes</th><th>Stays the same</th></tr></thead><tbody><tr><td><strong>SCEPman hosting</strong></td><td>Managed by us, no longer in your Azure tenant.</td><td></td></tr><tr><td><strong>SCEPman URL</strong></td><td>New endpoint URLs.</td><td></td></tr><tr><td><strong>RADIUS server certificate</strong></td><td>If you currently use the <a href="../../admin-portal/settings/settings-server.md#scepman-issued-server-certificate">SCEPman connection</a> or a <a href="../../admin-portal/settings/settings-server.md#scepman-issued-server-certificate">SCEPman-issued server certificate</a> a new server certificate will be created.</td><td>No changes if you use the <a href="../../admin-portal/settings/settings-server.md#customer-ca">Customer CA</a>.</td></tr><tr><td><strong>Certificate functionality</strong></td><td></td><td>Certificates work the same way as before.</td></tr><tr><td><strong>MDM integration</strong></td><td>SCEP certificate profile(s) need to point to new URL and reference a new Root CA certificate.</td><td>Overall integration approach remains the same.</td></tr><tr><td><strong>End-user experience</strong></td><td></td><td>No impact, transparent to end-users.</td></tr><tr><td><strong>Existing certificates</strong></td><td>See migration impact section below.</td><td>Valid certificates continue to work.</td></tr></tbody></table>

### Pre-Migration Considerations

* **Features**: Some features of SCEPman Enterprise are not available in <code class="expression">space.vars.SCEPmanSAAS_ProductName</code>. Please have a look in our [SCEPman Edition Comparison](../../details.md#scepman-edition-comparison) to ensure, that you have all your use-cases covered, before you start the migration.&#x20;
* **MDM SCEP profiles**: You'll need to update your SCEP profiles to point to the new <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> URL and reference the new Root CA. Plan for how you want to roll this out (all at once vs. phased).
* **Network Environment**: If you are using RadSec, migrating to <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> can mean that the server certificate used for the connection might change.
* **DNS or firewall rules**: If you have any network rules tied to your current SCEPman endpoint, these will need to be updated.

## Migration Path

{% stepper %}
{% step %}
### Setup <code class="expression">space.vars.SCEPmanSAAS_ProductName</code>

Follow our dedicated guide on how to enroll:

{% content-ref url="./" %}
[.](./)
{% endcontent-ref %}

You should be in the following situation after this step:

* <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> is enrolled and your devices have received suitable certificates for client authentication to be used with RADIUSaaS (besides the ones issued by your current SCEPman Enterprise deployment).
* <code class="expression">space.vars.SCEPmanSAAS_ProductName</code>' CA certificate is trusted by your devices and added to the RADIUSaaS trust store.
{% endstep %}

{% step %}
### Verify Client Configuration

Clients already using RADIUSaaS will require the following adjustments if you want to migrate to <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> :

* Ensure they are trusting the <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> Root CA certificate
* Adjust the WiFi profile to include (additive to the current SCEPman Enterprise root) the <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> CA certificate for server validation

{% hint style="danger" %}
**Caution if you have an existing** [**SCEPman Connection**](https://docs.radiusaas.com/admin-portal/settings/settings-server#scepman-connection)

If you have already setup the SCEPman connection for automatic server certificate management, <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> will take over the connection after the enrollment. This means the server certificate will be from a different issuer **after the next certificate rotation**.

Ensure that your clients have the <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> CA certificate installed and trust the new CA for server validation in the WiFi profile.
{% endhint %}
{% endstep %}

{% step %}
### Verify Authenticator Configuration

If you are using RadSec, your RADIUS authenticators (access points, switches, and VPN gateways) will also have to trust the new Root CA certificate to establish a connection to RADIUSaaS.

Ensure that the <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> Root CA has been added here to prevent impact during the cutover of the server certificate.

{% hint style="danger" %}
**Please check if your network equipment handles multiple CAs**

If your authenticators are not able to trust multiple CAs for the RadSec server validation, changing the server certificate to one issued by <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> will need to happen at the same time as changing the CA trusted for the RadSec authentication as connections will be failing otherwise.
{% endhint %}
{% endstep %}

{% step %}
### Check for Rules matching Issuer(s)

If you are using rules to filter for specific certificate authorities, client authentications might be rejected when using certificates from <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> if the **Any authentication allowed** rule is disabled.

Add new rules for the added CA certificate or make sure to modify existing rules to include it:

<figure><img src="../../.gitbook/assets/image (513).png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

## Frequently Asked Questions

### Can I run SCEPman Enterprise and <code class="expression">space.vars.SCEPmanSAAS_ProductName</code> in parallel during the migration phase?

Yes. As long as you have trusted both Root CA certificates for client connection in RADIUSaaS, client certificates from both certification authorities can be used for authentication.

### Can I use my existing SCEPman Enterprise Root Certificate for <code class="expression">space.vars.SCEPmanSAAS_ProductName</code>?

While technically possible through [Bring your own Key Vault](../../admin-portal/settings/scepman.md#scepman-identity-and-keys), reusing your existing Root CA certificate is not recommended for the following reasons:

* Already issued certificates are only stored in the tenant of the existing SCEPman Enterprise deployment while newly created certificates will be stored within <code class="expression">space.vars.SCEPmanSAAS_ProductName</code>. This can lead to failing validations if components are not able to figure out which CA should be considered for validation
* Depending on the deployment location of your Key Vault and your RADIUSaaS instance, added latency is expected during cryptographic operations that require the Root CA certificate

### When can I decommission my old SCEPman instance?

Once all certificate use cases of the SCEPman Enterprise instance have been migrated to <code class="expression">space.vars.SCEPmanSAAS_ProductName</code>, you can safely stop the app service. These use cases include but are not limited to:

* Client Certificates
* Server Certificates
* DC Certificates
* Certificates for TLS inspection
* Code Signing Certificates
* S/MIME Certificates

Make sure to monitor the Log Analytics Workspace long enough to verify that no certificate validation or issuance is happening anymore.

{% embed url="https://docs.scepman.com/azure-configuration/log-configuration" %}
