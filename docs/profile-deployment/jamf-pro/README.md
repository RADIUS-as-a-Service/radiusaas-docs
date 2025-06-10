# Jamf Pro

## General

{% hint style="success" %}
We strongly recommend to configure all 802.1X-relevant payloads in a **single** Configuration Profile in Jamf - and one Configuration Profile per assignment type (Computers, Devices, Users).&#x20;
{% endhint %}

To fully configure 802.1X, this typically means you require the following payloads:

* Network payload(s) (WiFi and/or LAN)
* Certificate payload(s)
  * Root CA certificate of the SCEP-issuing CA
  * Root CA certificate of the RADIUS server certificate issuing CA (can be the same as the SCEP issuing CA)
* SCEP payload (client authentication certificate)

<figure><img src="../../../.gitbook/assets/image (306).png" alt=""><figcaption></figcaption></figure>

## Payloads

{% content-ref url="server-trust.md" %}
[server-trust.md](server-trust.md)
{% endcontent-ref %}

{% content-ref url="wifi-profile.md" %}
[wifi-profile.md](wifi-profile.md)
{% endcontent-ref %}

{% content-ref url="wired-profile.md" %}
[wired-profile.md](wired-profile.md)
{% endcontent-ref %}
