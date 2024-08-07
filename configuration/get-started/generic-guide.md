# Generic Guide

## Configuration Steps

### Step 1: PKI Setup

{% hint style="warning" %}
This is a **mandatory** step.&#x20;

May be omitted if you are using RADIUSaaS with [username/password-based authentication](../../admin-portal/users.md) only.
{% endhint %}

Set up your PKI so that the necessary client authentication certificates are automatically pushed to your endpoint devices.&#x20;

If you are using any of the below PKIs, please follow the relevant guides instead:

{% content-ref url="scenario-based-guides/microsoft-cloud-pki.md" %}
[microsoft-cloud-pki.md](scenario-based-guides/microsoft-cloud-pki.md)
{% endcontent-ref %}

### Step 2: Trusted CA(s) Setup

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

Tell your RADIUSaaS instance which client authentication certificates will be allowed to authenticate and how to check if those certificates are still valid:

{% content-ref url="../../admin-portal/settings/trusted-roots.md" %}
[trusted-roots.md](../../admin-portal/settings/trusted-roots.md)
{% endcontent-ref %}

### Step 3: RADIUS Server Certificate Configuration

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

Since endpoint devices will establish a TLS connection to RADIUSaaS during network authentication, RADIUSaaS must present a server certificate to the client (the **RADIUS Server Certificate**). This certificate can be generated directly from the **RADIUSaaS Admin Portal** or imported if you already own a suitable certificate (**BYO**).

In case you are happy to use the **built-in** [**Customer CA**](../../admin-portal/settings/settings-server.md#customer-ca), whose sole purpose it is to issue the **RADIUS Server Certificate**, no further action is required as part of this step.

In case you'd prefer to bring your own TLS server certificate, issued by your preferred CA, please follow [these steps](../../admin-portal/settings/settings-server.md#bring-your-own-certificate).

### Step 4: Network Equipment Configuration

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

#### RadSec

If your network equipment supports the **RadSec** protocol, follow below steps:

**WiFi Access Points**

For some popular vendors, we have prepared representative step-by-step guides on setting up the RadSec connection [here](../access-point-setup/radsec-available/). While we are not able to provide documentation for every vendor, in general, the following steps apply:

1. Import your active **RADIUS Server Certificate** to your WiFi infrastructure.
2. Add the CA certificate from which your APs have obtained their **RadSec Connection Certificate** to your **Trusted Ceertificates** list as described [here](../../admin-portal/settings/trusted-roots.md#add).
3. Create a new RADIUS profile.
4. Set the IP address and the port of your server in your RADIUS profile. Therefore, use the [public RadSec IP address](../../admin-portal/settings/settings-server.md#properties) and the standard RadSec port (2083).
5. Set the **Shared Secret** to "radsec", if applicable.
6. Assign the created profile to your SSID(s).

#### Wired (LAN) Switches

Currently, we have not prepared sample guides for networking switches yet. However, the configuration steps are similar to the ones for WiFi Access Points. In case you face difficulties, please [reach out to us](https://www.radius-as-a-service.com/help/).

#### RADIUS

If your network equipment **does not support RadSec**, you must first deploy proxies that handle the protocol conversion from [RADIUS](../../details.md#what-is-radius) to [RadSec](../../details.md#what-is-radsec) :

{% content-ref url="../../admin-portal/settings/settings-proxy.md" %}
[settings-proxy.md](../../admin-portal/settings/settings-proxy.md)
{% endcontent-ref %}

Next, move on to configuring your equipment:

**WiFi Access Points**

For some popular vendors, we have prepared representative step-by-step guides [here](../access-point-setup/proxy-needed/). While we are not able to provide documentation for every vendor, in general, the following steps apply:

1. Create a new RADIUS profile.
2. Configure an external RADIUS server:
   * As server IP address, configure the IP address of your [**proxy**](../../admin-portal/settings/settings-server.md#properties-1)**.**
   * Take the shared secret from your [**Server Settings**](../../admin-portal/settings/settings-server.md#radius-udp) page.
   * Configure the standard ports for RADIUS authentication (1812) and accounting (1813 - optional).
3. Assign the created profile to your SSID(s).

**Wired (LAN) Switches**

Currently, we have not prepared sample guides for switch appliances yet. However, the configuration steps are similar to the ones for WiFi Access Points. In case you face difficulties, please [reach out to us](https://www.radius-as-a-service.com/help/).

### Step 5: Configure your MDM Deployment Profiles

{% hint style="warning" %}
This is a **mandatory** step.
{% endhint %}

{% hint style="success" %}
**For Jamf Pro**

We strongly recommend to configure all 802.1X-relevant payloads in a **single** Configuration Profile in Jamf Pro - and one Configuration Profile per assignment type (Computers, Devices, Users).&#x20;
{% endhint %}

#### Server Certificate

To enable trust between your endpoint devices and the server certificate RADIUSaaS presents upon authentication, configure a trusted certificate profile in your preferred MDM solution. Therefore, first download the Root CA certificate that has issued your currently active **RADIUS Server Certificate** as described [here](../../admin-portal/settings/settings-server.md#download).

{% hint style="danger" %}
When downloading the relevant certificate, ensure to **only** **download** the **Root CA** certificate of your currently active RADIUS Server Certificate (highlighted in green) - not the entire chain or the RADIUS Server Certificate itself!
{% endhint %}

Move on to push out this certificate via MDM:

**Microsoft Intune**

{% content-ref url="../../profile-deployment/microsoft-intune/trusted-root.md" %}
[trusted-root.md](../../profile-deployment/microsoft-intune/trusted-root.md)
{% endcontent-ref %}

#### Jamf Pro

{% content-ref url="../../profile-deployment/jamf-pro/server-trust.md" %}
[server-trust.md](../../profile-deployment/jamf-pro/server-trust.md)
{% endcontent-ref %}

#### WiFi Profile

To configure a WiFi profile in your preferred MDM solution, follow one of these guides:

**Microsoft Intune**

{% content-ref url="../../profile-deployment/microsoft-intune/wifi-profile/" %}
[wifi-profile](../../profile-deployment/microsoft-intune/wifi-profile/)
{% endcontent-ref %}

**Jamf Pro**

{% content-ref url="../../profile-deployment/jamf-pro/wifi-profile.md" %}
[wifi-profile.md](../../profile-deployment/jamf-pro/wifi-profile.md)
{% endcontent-ref %}

#### Wired (LAN) Profile

To configure a wired (LAN) profile for your stationary devices in your preferred MDM solution, follow one of these guides:

**Microsoft Intune**

{% content-ref url="../../profile-deployment/microsoft-intune/wired-profile/" %}
[wired-profile](../../profile-deployment/microsoft-intune/wired-profile/)
{% endcontent-ref %}

**Jamf Pro**

{% content-ref url="../../profile-deployment/jamf-pro/wired-profile.md" %}
[wired-profile.md](../../profile-deployment/jamf-pro/wired-profile.md)
{% endcontent-ref %}

### Step 6: Rules

{% hint style="info" %}
This is an **optional** step.
{% endhint %}

If you would like to configure additional rules, for example to assign VLAN IDs or limit authentication requests to certain trusted CAs or WiFi access points, please check out the RADIUSaaS Rule Engine.

{% content-ref url="../../admin-portal/settings/rules/" %}
[rules](../../admin-portal/settings/rules/)
{% endcontent-ref %}
