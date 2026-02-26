# Upcoming Change: RADIUS Proxy IP Address Migration

As part of our continuous efforts to improve the reliability and scalability of **RADIUSaaS**, we are updating the IP addresses of certain **RADIUS proxy systems**. This change will be rolled out using a **soft migration approach** to ensure a smooth transition for all affected customers.

## Migration Timeline

* **Start of Migration Phase:** After the [July 2025 Release](./#july-2025-release) (18 July 2025)
* **End of Migration Phase:** End of April 2026

During this period, affected proxy servers will have two IP addresses in parallel:&#x20;

* **The old/legacy IP address**
* **a new IP address**&#x20;

Both IP addresses will be **fully functional during** the migration phase. After the migration phase ends, the **old**/**legacy IP addresses will stop working**.

## Proxy Update

As part of the [**March 2026 Release**](./#march-2026-release), we are finalizing the migration of our managed **RADIUSaaS proxies**. Administrators using RADIUSaaS proxies will see a pop-up window titled **“Proxy Infrastructure Update”** with three options: _**Postpone**_, _**Go To Proxies**_, and _**Update**_.

<figure><img src="../../.gitbook/assets/image (500).png" alt=""><figcaption></figcaption></figure>

Before you start the update, **adapt your network equipment configuration** to the **new proxy IP addresses** (see [How to check if you are affected](upcoming-change-radius-proxy-ip-address-migration.md#how-to-check-if-you-are-affected) for details).&#x20;

When you click _**Update**_, your tenant’s proxies will be updated. **Once the update completes, the legacy proxy IP addresses will no longer work**.&#x20;

If you need time to adapt your network equipment configuration, choose _**Postpone**_**.**

_**Go To Proxies**_ open the **Proxy Settings** page to review your configuration.

## What You Need to Do

If your configuration uses one of the affected RADIUS proxies, you must:

1. **Update your network equipment** to use the new RADIUS proxy IP address.
2. **Run the** [**Proxy Update**](upcoming-change-radius-proxy-ip-address-migration.md#proxy-update) (latest by the end of April 2026).

## How to check if you are affected

{% hint style="info" %}
In general only customers using the **RADIUS protocol** may be affected. If you are using **RadSec only**, no action is required, since you are not using the RADIUSaaS Proxy services.

Note that not every proxy server requires an IP address change.
{% endhint %}

You can verify whether your proxy configuration is impacted by logging into the **RADIUSaaS Admin Web Portal**:

* Navigate to your **proxy configuration**.
* If your proxy is affected, you will see:
  * A **new IP address** is listed.
  * A **legacy IP address** is shown in parentheses, labeled with the word **"legacy"**.

In the following example, the proxy with the region "Europe (London)" is affected, while the proxy with the region "North America (Dallas)" is not affected. The customer is required to change the IP address for the Europe (London) proxy in their network equipment, while 165.227.229.49 is the old IP address and  139.59.200.190 is the new address:

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

## Need Help?

If you have any questions or need assistance updating your configuration, please contact our support team via the [RADIUSaaS Support Portal](https://support.radiusaas.com/support/tickets/new?ticket_form=technical_support_request_%28radiusaas%29).
