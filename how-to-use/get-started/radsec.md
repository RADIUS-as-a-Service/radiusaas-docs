# RadSec

## Configuration Steps

### Step 1: RadSec Connection Certificate

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

To establish trust between your RadSec-capable network gear and RADIUSaaS, upload the RadSec server certificate as described [here](../../portal/settings/settings-server/certificates.md#radsec-connection-certificates).

Your network gear vendor should either provide this certificate or provide guidance on how to create one (CSR or via FQDN).&#x20;

### Step 2: Server Certificate

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

Since the endpoint device will establish a TLS tunnel to RADIUSaaS during network authentication, a trusted TLS certificate is required. This can be generated directly from the RADIUSaaS Admin Portal or imported if you already own a suitable certificate.

1. Create a custom CA or upload your own certificate as described [here](../../portal/settings/settings-server/certificates.md#server-certificates).&#x20;
2. Download the **active** server certificate as described [here](../../portal/settings/settings-server/certificates.md#download). You will need it later on for the Intune device profile.

{% hint style="danger" %}
Please ensure to **download** **the root CA certificate** (highlighted in green). This root certificate must later be deployed to your endpoint devices - not the server certificate itself. In case you are using SCEPman to create a server certificate, you probably already have the SCEPman root CA certificate deployed into the trust store of your endpoints.
{% endhint %}

### Step 3: Trusted Root CA

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

1. Tell your RADIUSaaS instance which certificates will be allowed to connect as described [here](../../portal/settings/settings-trusted-roots/trusted-roots.md#add) .

### Step 4: Network Gear Configuration

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

#### WiFi Access Points

For some popular vendors, we have prepared representative step-by-step guides [here](../access-point-setup/radsec-available/). While we are not able to provide documentation for every vendor, in general, the following steps apply:

1. Import your active RADIUS Server Certificate to your WiFi infrastructure.
2. Add the CA certificate from which your APs obtain their RadSec connection certificate to your **RadSec allowed Connection** list as described [here](../../portal/settings/settings-server/certificates.md#add-a-new-certificate).
3. Create a new RADIUS profile.
4. Set the IP address and the port of your server in your RADIUS profile. Therefore, use the [public RadSec IP address](../../portal/settings/settings-server/ports-and-ip-addresses.md#server-ip-address) and the standard RadSec port (2083).
5. Assign the created profile to your SSID(s).

#### Wired (LAN) Switches

Currently, we have not prepared sample guides for switch appliances yet. However, the configuration steps are similar to the ones for WiFi Access Points. In case you face difficulties, please [reach out to us](https://www.radius-as-a-service.com/help/).

### Step 5: Configure your MDM Deployment Profiles

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

{% hint style="success" %}
**For Jamf Pro**

We strongly recommend to configure all 802.1X-relevant payloads in a **single** Configuration Profile in Jamf - and one Configuration Profile per assignment type (Computers, Devices, Users).&#x20;
{% endhint %}

#### Server Certificate

To enable trust between the client and RADIUSaaS, configure a trusted certificate profile in your preferred MDM solution:

**Microsoft Intune**

{% content-ref url="../../azure/microsoft-intune/trusted-root.md" %}
[trusted-root.md](../../azure/microsoft-intune/trusted-root.md)
{% endcontent-ref %}

#### Jamf Pro

{% content-ref url="../../azure/jamf/trusted-root.md" %}
[trusted-root.md](../../azure/jamf/trusted-root.md)
{% endcontent-ref %}

#### WiFi Profile

To configure a WiFi profile in your preferred MDM solution, follow one of these guides:

**Microsoft Intune**

{% content-ref url="../../azure/microsoft-intune/wifi-profile/" %}
[wifi-profile](../../azure/microsoft-intune/wifi-profile/)
{% endcontent-ref %}

**Jamf Pro**

{% content-ref url="../../azure/jamf/wifi-profile.md" %}
[wifi-profile.md](../../azure/jamf/wifi-profile.md)
{% endcontent-ref %}

#### Wired (LAN) Profile

To configure a wired (LAN) profile for your stationary devices in your preferred MDM solution, follow one of these guides:

**Microsoft Intune**

{% content-ref url="../../azure/microsoft-intune/wired-profile/" %}
[wired-profile](../../azure/microsoft-intune/wired-profile/)
{% endcontent-ref %}

**Jamf Pro**

{% content-ref url="../../azure/jamf/wired-profile.md" %}
[wired-profile.md](../../azure/jamf/wired-profile.md)
{% endcontent-ref %}

### Step 6: Rules

{% hint style="info" %}
This is an **optional** step.
{% endhint %}

If you would like to configure additional rules, for example to assign VLAN IDs or limit authentication requests to certain trusted CA or WiFi access points, please check out the RADIUSaaS Rule Engine.

{% content-ref url="../../portal/settings/rules/" %}
[rules](../../portal/settings/rules/)
{% endcontent-ref %}
