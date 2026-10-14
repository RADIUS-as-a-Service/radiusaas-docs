# Meraki

{% hint style="info" %}
To use the RadSec feature on your Meraki APs, firmware version **MR 30.X** or later is required.
{% endhint %}

### Critical Notice to all Meraki Customers

{% hint style="warning" %}
Meraki is moving RadSec device certificates from the Keeper Organization CA to its new [PureCA service](https://documentation.meraki.com/Wireless/Design_and_Configure/Architecture_and_Best_Practices/Cisco_Keeper_to_PureCA_Migration_Strategy). Once an access point presents a PureCA-issued certificate, RADIUSaaS rejects its RadSec connection unless the PureCA chain is trusted, and authentication fails.

Meraki lists MR 33.1.2 as the minimum firmware for PureCA. However, we have seen access points on earlier firmware versions presenting PureCA certificates, so the firmware version is not a reliable indicator of whether you are affected.

To prevent or fix this, upload the complete PureCA chain to your RADIUSaaS Trusted Certificates with **Use for** set to **RadSec**, as described in [RadSec PureCA certificates](https://docs.radiusaas.com/configuration/access-point-setup/radsec-available/meraki#radsec-pureca-certificates). Keep your existing Keeper CA trusted as well. Trusting both in parallel is fully supported and lets access points connect regardless of which CA issued their certificate.

For questions about which CA or firmware your access points use, or about the migration itself, please contact Meraki support.

\
_&#x4C;ast updated: 14 October 2026_
{% endhint %}

## Prepare certificates

{% hint style="warning" %}
The Meraki platform does not allow you to generate RadSec client certificates from a CA of your choice. Instead, you must use Meraki's built-in **Organization CA** that is unique to your Meraki Organization.
{% endhint %}

Download the root certificate of the CA that has issued your active **RADIUS Server Certificate** as described [here](../../../admin-portal/settings/settings-server.md#download). You will need to upload it to your Meraki console later on.

## Meraki configuration

{% hint style="info" %}
Below settings are the necessary settings to establish a functional RadSec connection with our service. Configure any other settings at your discretion.
{% endhint %}

{% hint style="danger" %}
Ensure to **disable the OCSP revocation check of the RadSec client certificate** as described in step 7 of this guide.
{% endhint %}

{% stepper %}
{% step %}
### Navigate to your Meraki Dashboard

In your Dashboard, select **Wireless > Access Control** and ensure that you have switched to the **new UI version** of the Access control site:

<figure><img src="../../../.gitbook/assets/image (57) (1).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Configure the RADIUS server in your SSID

Select the **SSID** you wish to configure RADIUS authentication for (or navigate to **Wireless > Configure > SSIDs** to create a new SSID first).

In the **Security** section of your SSID, select **Enterprise with** and in the dropdown **my RADIUS server:**

<figure><img src="../../../.gitbook/assets/image (534).png" alt=""><figcaption></figcaption></figure>

Under **RADIUS**, click **Add server**. Configure the **Host IP or FQDN** to match the IP address or the DNS name of your [RadSec service endpoint](../../../admin-portal/settings/settings-server.md#properties), set the **Port** to 2083 and  set the **Secret** value to "radsec" and activate the **RadSec** checkbox.

<figure><img src="../../../.gitbook/assets/image (537).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Configure EAP Timeouts

Configure **EAP parameters and timeouts** according to [this](../../../other/faqs/general.md#timers-and-timeouts) reference guide by going to **Wireless** > **Radius** > **Advanced RADIUS settings.** Once configured, it should look similar to the screenshot below. Click **Save** to apply the settings.

<figure><img src="../../../.gitbook/assets/image (538).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Add RadSec Server Certificate

To upload and generate the required certificates that make the RadSec connection functional, navigate to **Organization > Certificates > RADSEC**.

<figure><img src="../../../.gitbook/assets/image (70) (1).png" alt=""><figcaption></figcaption></figure>

In the top table, click **Upload certificate** and provide the root certificate of the CA that has signed your [**RADIUS Server Certificate**](../../../admin-portal/settings/settings-server.md#server-certificates), which you should have already downloaded in this [step](meraki.md#prepare-certificates). Your Meraki APs now trust your RADIUS server.

{% tabs %}
{% tab title="Using RADIUSaaS Customer CA" %}
<figure><img src="../../../.gitbook/assets/image (518).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="Using SCEPman or other CA" %}
<figure><img src="../../../.gitbook/assets/image (519).png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
### Create RadSec Client Certificate

{% tabs %}
{% tab title="Keeper" %}
Under **RadSec device certificates**, first create an **Organization CA** by clicking **Generate CA**. This CA is unique to your Meraki Organization.\
The Meraki platform will now automatically generate RadSec client certificates for all your APs signed by this CA. The lifetime of the certificate is very long (> 50 years), i.e. you do not have to worry about renewing them.

Download the root CA certificate of your **Organization CA** of your Meraki system by clicking **Download CA**.

<figure><img src="../../../.gitbook/assets/image (625).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="RadSec PureCA certificates" %}
{% hint style="info" %}
PureCA is currently in beta.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (626).png" alt=""><figcaption></figcaption></figure>

After downlading the certificate chain, you will have to split it up and upload every certificate in the chain separately to RADIUSaaS > Trusted Certificates, starting with the root.&#x20;

You can use PowerShell to split the chain into 5 certificates. For example, the following script will split the downloaded chain and saves each file separately using the certificate's commonName as the file name.&#x20;

{% code overflow="wrap" %}
```powershell
$c=[regex]::Matches((Get-Content .\chain.pem -Raw),'(?s)-----BEGIN CERTIFICATE-----.*?-----END CERTIFICATE-----').Value | % { [Security.Cryptography.X509Certificates.X509Certificate2]::new([Text.Encoding]::ASCII.GetBytes($_)) }; $ord=[Collections.ArrayList]@(); $seen=@{}; $cur=$c | ? { $_.Subject -eq $_.Issuer } | Select -First 1; while($cur){ [void]$ord.Add($cur); $seen[$cur.Thumbprint]=1; $cur=$c | ? { $_.Issuer -eq $ord[-1].Subject -and -not $seen[$_.Thumbprint] } | Select -First 1 }; $c | ? { -not $seen[$_.Thumbprint] } | % { [void]$ord.Add($_) }; $i=0; $ord | % { $cn=($_.GetNameInfo('SimpleName',$false) -replace '[\\/:*?"<>|]','_'); $f=".\{0:d2}-{1}.pem" -f $i++,$cn; Set-Content $f ("-----BEGIN CERTIFICATE-----`n"+[Convert]::ToBase64String($_.RawData,'InsertLineBreaks')+"`n-----END CERTIFICATE-----") -Encoding ascii; "$f   subject=$($_.Subject)   issuer=$($_.Issuer)" }

```
{% endcode %}

The above script will produce the following certificate structure.

```
00-Cisco Meraki Dashboard Root CA.pem
  └─ 01-Cisco Meraki Dashboard Cluster SubCA.pem
    └─ 02-Meraki.com Cluster CA.pem
      └─ 03-1608009.pem                   (your org CA)
        └─ 04-radsec-1608009.pem          (issues the RadSec client certs)
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
You will not be able to upload RadSec client certificates of another CA with Meraki. Instead the Organization CA, which is unique to your organization, will provide all your access points with suitable certificates.
{% endhint %}
{% endstep %}

{% step %}
### Trust Organization CA in RADIUSaaS

Now, upload the downloaded CA certificate to your [Trusted Certificates in your RADIUSaaS web console](../../../admin-portal/settings/trusted-roots.md#add) and select **RadSec** under **Use for**.

<figure><img src="../../../.gitbook/assets/image (68) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
In case your PKI has a multi-tier hierarchy (Root CA, Intermediate CA, Issuing CA), make sure to upload all of them to the RADIUSaaS Trusted Certificates store.
{% endhint %}
{% endstep %}

{% step %}
### Disable Revocation Check for RadSec Certificates

Finally, disable the [revocation check for the RadSec client certificates](../../../admin-portal/settings/settings-server.md#verification-check-for-radsec-certificates) on your RADIUSaaS instance (this does not adversely affect security as the Meraki Organization CA does not allow to revoke RadSec client certificates). Therefore, navigating to your RADIUSaaS instance and then **Settings > Server Settings** and disable the checkbox **Verification check for RadSec certificates**.

<figure><img src="../../../.gitbook/assets/image (66) (1).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Test Configuration

To test that the configuration works, you can add a user in your [Portal](../../../admin-portal/users/users.md#add-a-new-user) and use the Meraki test function.

<figure><img src="../../../.gitbook/assets/image (67) (1).png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}

### References

Link to Meraki's documentation for the RadSec configuration: [https://documentation.meraki.com/MR/Encryption\_and\_Authentication/MR\_RADSec](https://documentation.meraki.com/MR/Encryption_and_Authentication/MR_RADSec)

{% code overflow="wrap" %}
```
ERROR: (0) SSL says error 20 : unable to get local issuer certificate
ERROR: (0) TLS Alert write:fatal:unknown CA
```
{% endcode %}
