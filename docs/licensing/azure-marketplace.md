# Azure Marketplace

## Prerequisites

In order to purchase solutions from independent software vendors (ISV) such as RADIUSaaS or the RADIUSaaS & SCEPman Bundle, you must fulfil the following requirements:

1. You have an active Azure subscription.
2. The account you want to purchase our solution with must have the **Owner** or **Contributor** role assigned on the Azure subscription you are going to pay with.
3. The billing account linked to your Azure subscription is properly set up. Depending on your billing account type (Microsoft Customer Agreement or Enterprise Agreement), you might need to enable marketplace purchases in the Azure portal first.

## Pricing model

* RADIUSaaS is offered as a **monthly or** **annual subscription plan** with different [User Segments](azure-marketplace.md#user-segments). The correct **user segment** is automatically selected by our platform based on the amount of desired users.
* All subscription plans consist of a **base fee** which includes a certain amount of users per subscription cycle - depending on the **user segment**. For example, the **base fee** for the user segment _RADIUSaaS (M) 50_ includes 50 users per month.
* If more than the included amount of users is required, **additional users** can be added to the  plan. For each additional user, we charge an additional per-user fee.

## Invoicing

* During the first subscription interval, your subscription fees are not immediately due after completing the subscription enrolment. Instead we will start billing once your cancellation grace period has expired.&#x20;
* Upon every renewal date, you will be billed immediately.
* You will always be billed for the entire subscription cycle in advance.
* The related items should appear on your Microsoft Azure invoice (Pay-As-You-Go or Enterprise Agreement) the month after we have reported your fees to Microsoft.
*   In the PDF invoice you will receive from Microsoft, all RADIUSaaS fees are lumped into an item called "SaaS". The related Publisher is "glueckkanja".\


    <figure><img src="../.gitbook/assets/image (15) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
For a more detailed cost breakdown of your base and additional user fees, please refer to the invoice in your Azure portal.
{% endhint %}

## Plan overview

Subscriptions for RADIUSaaS are available based on a **monthly** or **annual** renewal interval.

{% hint style="info" %}
The annual plan is discounted by 10% in comparison to the monthly plan (calculated over the period of 12 months).
{% endhint %}

| **Plan**      | **Renewal Interval** |
| ------------- | -------------------- |
| RADIUSaaS (M) | Monthly              |
| RADIUSaaS (Y) | Annually             |

### User segments

The following user segments are available for both, monthly and annual plans:&#x20;

<table data-header-hidden><thead><tr><th width="240.02162801098973">Plan</th><th width="244.07580174927114">Included Users</th><th></th></tr></thead><tbody><tr><td><strong>User Segment</strong></td><td><strong>Included Users in Base Fee</strong></td><td><strong>Maximum Total Users</strong></td></tr><tr><td>RADIUSaaS (M/Y) 50</td><td>50</td><td>249</td></tr><tr><td>RADIUSaaS (M/Y) 250</td><td>250</td><td>999</td></tr><tr><td>RADIUSaaS (M/Y) 1000</td><td>1,000</td><td>4,999</td></tr><tr><td>RADIUSaaS (M/Y) 5000</td><td>5,000</td><td>9,999</td></tr><tr><td>RADIUSaaS (M/Y) 10000</td><td>10,000</td><td>unlimited</td></tr></tbody></table>

For prices in Euro (EUR), please check out our [website](https://www.radius-as-a-service.com/pricing/). For prices in _your_ currency, please directly refer to the **Marketplace** in the [Azure Portal](https://portal.azure.com/).

## RADIUSaaS & SCEPman Bundle

We offer RADIUSaaS as well as our cloud-CA solution [SCEPman](https://www.scepman.com/) in a subscription bundle that is discounted by 25% in comparison to the individual solutions. The bundle plans are available with monthly or annual renewal as well as the same [User Segments](azure-marketplace.md#user-segments).&#x20;

Furthermore, the bundle plans allow the **one-time** purchase of the [SCEPman Setup Support](https://docs.scepman.com/licensing#optional-scepman-setup-support).

The information provided throughout this article is analogously applicable for RADIUSaaS & SCEPman Bundle subscriptions.

## Subscription Management

### User upgrades

* If you would like to upgrade your user count, you can do that any time during the current subscription cycle by navigating to your **RADIUSaaS subscription** in the [Azure SaaS portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.SaaS%2Fresources) and by clicking "Open SaaS Account on publisher's site" (see screenshot below). This will re-direct you to our platform where the amount of users can be upgraded.

![](<../../.gitbook/assets/Screenshot 2022-02-18 at 12.32.27 2.png>)

* Our platform will inform you about the new fees you to expect for a **complete** subscription cycle.
* For the current cycle, we will bill the additional users for remaining days only.
* After confirming your choice and once we have updated the license in our backend, you will receive a confirmation email from us.

### User downgrades

* Downgrading the amount of users is currently not possible without cancelling the subscription.
* If you want to perform a downgrade, please [cancel](azure-marketplace.md#cancellation) your current subscription and re-subscribe with the desired amount of users once the cancellation becomes effective.

### **Change plan**

{% hint style="warning" %}
This is currently **not supported**. Please [contact us](https://www.radius-as-a-service.com/help/) in case you have questions around this topic.
{% endhint %}

### **Recurring billing**

If you decide to disable **Recurring billing**, your subscription will not renew automatically. Instead, Microsoft will (irreversibly) cancel the subscription towards the end of the current subscription cycle. This means, the service will be terminated automatically on that date as well. While the subscription has not expired yet, you can opt to enable **Recurring billing** at any time.

### Cancellation

* If you would like to (irreversibly) cancel your subscription, navigate to your **RADIUSaaS subscription** in the [Azure SaaS portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.SaaS%2Fresources) and click **Cancel subscription**.

<figure><img src="../../.gitbook/assets/Screenshot 2022-02-18 at 12.29.47 copy.png" alt=""><figcaption></figcaption></figure>

* If you cancel within the grace period, the service will be stopped immediately.
* If you cancel after the grace period, the service will remain active until the end of the current subscription cycle.

## **Trials**

In case you would like to test RADIUSaaS, please [get in contact with us](https://www.radius-as-a-service.com/start-now/) or send us an email to [sales@radiusaas.com](mailto:sales@radiusaas.com).

## FAQs

### How to purchase RADIUSaaS (RADIUSaaS & SCEPman Bundle)?

{% hint style="info" %}
Deploying a RADIUSaaS (RADIUSaaS & SCEPman Bundle) subscription via Azure Marketplace **will not result** **in a re-deployment of RADIUSaaS (or SCEPman) if you already have an active trial or production deployment**. Instead, we will assign the license obtained as part of this subscription to your existing deployments.

For **new customers**, we will provision a new instance of RADIUSaaS once below steps are completed. Please allow up to 1 business day for us to complete the provisioning.
{% endhint %}

To get started with your RADIUSaaS or RADIUSaaS & SCEPman Bundle subscription, follow below steps:

* Locate [RADIUSaaS](https://portal.azure.com/#view/Microsoft_Azure_Marketplace/GalleryItemDetailsBladeNopdl/id/glueckkanja-gabag.radiusaas-transactable-prod) or the [RADIUSaaS & SCEPman Bundle](https://portal.azure.com/#view/Microsoft_Azure_Marketplace/GalleryItemDetailsBladeNopdl/id/glueckkanja-gabag.radiusaas-scepman-bundle-prod) on the **Marketplace** in your **Azure Portal**. In case we have extended a **Private Offer** to you or your MSP/distribution has extended a **Multiparty Offer (MPO)** to you, navigate to **Marketplace** in your **Azure Portal** and then to **Private Offer Management** to locate the Private Offer.
  * More details on Private Offers and MPOs can be found in Microsoft's documentation.
    * [Private Offer](https://learn.microsoft.com/en-us/marketplace/private-offers-purchase)
    * [Multiparty Offer](https://www.youtube.com/watch?v=TANUlgLuVqI)
* Select the **Plan** based on your preferred renewal interval and click **Subscribe**.

![](<../../.gitbook/assets/image (333).png>)

* Create or select the **Resource group** you would like to deploy the subscription to.
* Assign a descriptive **Name** to later identify your subscription.
* We recommend to keep **Recurring billing** **On** so that you do not have to worry about an automatic termination of your subscription.
* Click **Review + subscribe** and then **Subscribe** to deploy the **SaaS** resource to your **Resource group**.

![](<../../.gitbook/assets/image (217).png>)

{% hint style="info" %}
The random order of **Base Fees** und **Additional Users** under the **Price** information is attributed to limitations of the Azure Marketplace. Later during the the enrolment process, we will provide you with transparent information on the expected licensing fees.
{% endhint %}

* Once the deployment is complete, please navigate to our platform to complete the checkout. Therefore click **Configure account now**.

![](<../../.gitbook/assets/image (221).png>)

* After authenticating on our platform using your Microsoft credentials, you will be prompted for additional information, such as the desired total **User** amount and a **Technical contact**.

{% hint style="info" %}
The **Technical contact** must have a mailbox connected to it, so we are able to notify you in case there are relevant issues with RADIUSaaS. By default, we also configure the **Technical contact** as initial administrator on your RADIUSaaS instance. In case you'd like to change that, please [let us know](https://www.radius-as-a-service.com/help/).
{% endhint %}

{% hint style="success" %}
If the plan contains chargeable add-ons, you can select them under **Extras**. For example, all RADIUSaaS & SCEPman Bundle plans allow you to purchase the optional [SCEPman Setup Support](https://docs.scepman.com/support#scepman-setup-support) package.
{% endhint %}

![](../../.gitbook/assets/Screenshot_2022-02-18_at_12_23_47.png)

* Based on the amount of users provided, we will charge the relevant base fee for your user segment as well as additional users, in case you require more than the included amount in your base fee. **The platform automatically selects the best price / tier**.
* The platform will show you the licensing fees you have to expect under **Cost Projection**.
* If you are happy with it, please click **Review & Submit** for a final review and a fee summary.
* Complete the checkout by confirming your choice and clicking **Submit**.
* This triggers us to deploy your RADIUSaaS instance (and issue a SCEPman Enterprise Edition license key if the RADIUSaaS and SCEPman Bundle is purchased). We will inform you via email with all relevant information on the next steps once the instance (and license key) is available for you. This won't take any longer than one business day.

{% hint style="info" %}
You will only be charged by Microsoft, once you have completed the enrolment on our platform and received our welcome email.
{% endhint %}

### Why is my "Plan no longer available for purchase"?

In case you see hint in your subscription as shown in below screenshot, **there is no need to worry**!

<figure><img src="../../.gitbook/assets/azure-saas-plan-no-longer-available-for-purchase copy.png" alt=""><figcaption></figcaption></figure>

The reasons for this hint is that - from time to time - we might have to deprecate plans for technical reasons.&#x20;

**Important:**

* As an existing customer, you may continue to use the deprecated plan indefinitely (until the subscription is cancelled).
* In case a newer plan gives you better pricing or other advantages, we will inform you about this.

### Why is my Azure Marketplace purchase not working?

You may encounter problems when purchasing through Azure Marketplace. Here is a list of reasons, why buying through Azure Marketplace may fail:

1. You do not have permissions in your Azure tenant to purchase through Azure Marketplace. You must be assigned the role of Owner or Contributor in the Azure subscription you want to pay with.
2. The subscription belongs to an Enterprise Agreement (EA) and the EA admin disabled Azure Marketplace purchases. Or the EA admin has enabled purchases only for free offers and the offer is a paid offer. Please see [here](https://learn.microsoft.com/en-us/marketplace/purchase-control-options) for details.
3.  The subscription you're using belongs to a billing account in a region where the offer isn't available.\
    Our Marketplace offers are available in the following countries/regions:

    Armenia, Australia, Austria, Bulgaria, Belgium, Canada, Chile, Colombia, Croatia, Cyprus, Czechia, Denmark, Estonia, Finland, France, Germany, Greece, Hungary, Iceland, India, Indonesia, Ireland, Italy, Kenya, Latvia, Liechtenstein, Lithuania, Luxembourg, Malaysia, Malta, Monaco, Netherlands, New Zealand, Nigeria, Norway, Poland, Portugal, Puerto Rico, Romania, Saudi Arabia, Serbia, Singapore, Slovakia, Slovenia, South Africa, South Korea, Spain, Sweden, Switzerland, Taiwan, Thailand, Türkiye, United Arab Emirates, United Kingdom, United States, Vietnam
4. The subscription/billing account isn't associated with a valid payment instrument (such as a valid credit card).
5. Private Marketplace is enabled for the subscription and the offer isn't in the list of allowed offers.
6. Purchases are not permitted for subscriptions with a spending cap, including Free subscriptions, Sponsorships, and similar types.

### Does RADIUSaaS (RADIUSaaS & SCEPman Bundle) count towards my Microsoft Azure Consumption Commitment (MACC)?

Currently (March 2025), this is the case. Since Microsoft may change their policy on what counts towards MACC in the future, please always [confirm](https://learn.microsoft.com/en-us/marketplace/azure-consumption-commitment-benefit#find-and-purchase-azure-benefit-eligible-offers-in-azure-marketplace) eligibility first.
