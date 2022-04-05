# RADIUS

## Configuration Steps

### Step 1: RADIUS Proxies

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

In order to bridge the RADIUS protocol from your network appliances to our internal RADIUS server's RadSec ports, please add RADIUS proxies to your instance as described here:

{% content-ref url="../../portal/settings/settings-proxy.md" %}
[settings-proxy.md](../../portal/settings/settings-proxy.md)
{% endcontent-ref %}

### Step 2: Server Certificate

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

Since the client will establish a TLS tunnel directly to RADIUSaaS during network authentication, a trusted TLS certificate is required. This can be generated directly from the RADIUSaaS Admin Portal or imported if you already own a suitable certificate.

{% hint style="danger" %}
If you are planning to use RADIUSaaS along with **Android** devices, you must create a custom CA or upload your own certificate that was signed by a CA. The default self-signed server certificate will not allow Android devices to connect to RADIUSaaS.
{% endhint %}

1. Create a custom CA, upload your own certificate or use the default self-signed server certificate as described [here](../../portal/settings/settings-server/certificates.md#server-certificates).&#x20;
2. Download the **active** server certificate as described [here](../../portal/settings/settings-server/certificates.md#download). You will need it later on for the Intune device profile.

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

For some popular vendors, we have prepared representative step-by-step guides [here](../access-point-setup/proxy-needed/). While we are not able to provide documentation for every vendor, in general, the following steps apply:

1. Create a new RADIUS profile.
2. Configure an external RADIUS server
   1. As server IP address, configure the IP address of your [**proxy**](../../portal/settings/settings-server/ports-and-ip-addresses.md#server-ip-address-and-location)**.**
   2. Take the shared secret from your [**Server Settings**](../../portal/settings/settings-server/ports-and-ip-addresses.md#shared-secret) page
   3. Configure the standard ports for RADIUS authentication (1812) and accounting (1813 - optional)
3. Assign the profile to the relevant SSID(s).

#### Wired (LAN) Switches

Currently, we have not prepared sample guides for switch appliances yet. However, the configuration steps are similar to the ones for WiFi Access Points. In case you face difficulties, please [reach out to us](https://www.radius-as-a-service.com/help/).

### Step 5: Intune Profile Configuration

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

#### Server Certificate

To enable trust between the client and RADIUSaaS, configure a trusted certificate profile as described here:

{% content-ref url="../../azure/trusted-root.md" %}
[trusted-root.md](../../azure/trusted-root.md)
{% endcontent-ref %}

#### WiFi Profile

To configure a WiFi profile in Intune, follow this guide:

{% content-ref url="../../azure/wifi-profile/" %}
[wifi-profile](../../azure/wifi-profile/)
{% endcontent-ref %}

#### Wired (LAN) Profile

To configure a wired (LAN) profile for your stationary devices in Intune, follow this guide:

{% content-ref url="../../azure/wired-profile/" %}
[wired-profile](../../azure/wired-profile/)
{% endcontent-ref %}

{% hint style="info" %}
Other MDM solutions, e.g. Jamf Pro, allow the creation of trusted certificate and network profiles in a very similar fashion. Please [reach out to us](https://www.radius-as-a-service.com/help/) in case you require support with your non-Intune MDM solution.
{% endhint %}

### Step 6: Rules

{% hint style="info" %}
This is an **optional** step.
{% endhint %}

If you would like to configure additional rules, for example to assign VLAN IDs or limit authentication requests to certain trusted CA or WiFi access points, please check out the RADIUSaaS Rule Engine.

{% content-ref url="../../portal/settings/rules/" %}
[rules](../../portal/settings/rules/)
{% endcontent-ref %}
