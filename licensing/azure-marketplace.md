# Azure Marketplace

## Pricing Model

* RADIUSaaS is offered as a **monthly or** **annual subscription plan** with different [User Segments](azure-marketplace.md#user-segments). The correct **user segment** is automatically selected by our platform based on the amount of desired users.
* All subscription plans consist of a **base fee** which includes a certain amount of users per subscription cycle - depending on the **user segment**. For example, the **base fee** for the user segment _RADIUSaaS (M) 50_ includes 50 users per month.
* If more than the included amount of users is required, **additional users** can be added to the  plan. For each additional user, we charge an additional per-user fee.



## Invoicing

* During the first subscription interval, your subscription fees are not immediately due after completing the subscription enrolment. Instead we will start billing once your cancellation grace period has expired.&#x20;
* Upon every renewal date, you will be billed immediately.
* The related items should appear on your Microsoft Azure invoice (Pay-As-You-Go) the month after we have reported your fees to Microsoft.
* In the PDF invoice you will receive from Microsoft, all RADIUSaaS fees are lumped into an item called "SaaS". The related Publisher is "glueckkanja-gab".![](<../.gitbook/assets/image (64).png>)

{% hint style="info" %}
For a more detailed cost breakdown of your base and additional user fees, please refer to the invoice in your Azure portal.
{% endhint %}

## Plan Overview

Subscriptions for RADIUSaaS are available based on a monthly or annual renewal interval.

{% hint style="info" %}
The annual plan is discounted by 10% in comparison to the monthly plan (calculated over the period of 12 months).
{% endhint %}

| **Plan**      | **Renewal Interval** |
| ------------- | -------------------- |
| RADIUSaaS (M) | Monthly              |
| RADIUSaaS (Y) | Annually             |

### User Segments

The following user segments are available for both, monthly and annual plans.&#x20;

| **User Segment**      | **Included Users in Base Fee** | **Maximum Total Users** |
| --------------------- | ------------------------------ | ----------------------- |
| RADIUSaaS (M/Y) 50    | 50                             | 249                     |
| RADIUSaaS (M/Y) 250   | 250                            | 999                     |
| RADIUSaaS (M/Y) 1000  | 1,000                          | 4,999                   |
| RADIUSaaS (M/Y) 5000  | 5,000                          | 9,999                   |
| RADIUSaaS (M/Y) 10000 | 10,000                         | unlimited               |

For prices in Euro (EUR), please check out our <mark style="color:green;"></mark> [website](https://www.radius-as-a-service.com/pricing/). For prices in _your_ currency, please directly refer to the **Marketplace** in the [Azure Portal](https://portal.azure.com).

## User Up- and Downgrades

### Upgrades

* If you would like to upgrade your user count, you can do that any time during the current subscription cycle by navigating to your **RADIUSaaS subscription** in the [Azure SaaS portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.SaaS%2Fresources) <mark style="color:green;"></mark> and by clicking "Open SaaS Account on publisher's site" (see screenshot below). This will re-direct you to our platform where the amount of users can be upgraded.

![](<../.gitbook/assets/Screenshot 2022-02-18 at 12.32.27 2.png>)

* Our platform will inform you about the new fees you to expect for a **complete** subscription cycle.
* For the current cycle, we will bill the additional users for remaining days only.
* After confirming your choice and once we have updated the license in our backend, you will receive a confirmation email from us.

### Downgrades

* Downgrading the amount of users is currently not possible without cancelling the subscription.
* If you want to perform a downgrade, please cancel your current subscription from the <mark style="color:green;"></mark> [Azure SaaS portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.SaaS%2Fresources) towards the end of the current cycle by clicking "Cancel subscription" (see screenshot below) and re-subscribe with the desired user amount once the cancellation becomes effective.

![](<../.gitbook/assets/Screenshot 2022-02-18 at 12.32.27 3.png>)

## **Trials**

In case you would like to test RADIUSaaS, please [get in contact with us](https://www.radius-as-a-service.com/start-now/) or send us an email to [sales@radiusaas.com](mailto:sales@radiusaas.com).

## How to purchase RADIUSaaS?

To get started with your RADIUSaaS subscription,

* Locate RADIUSaaS on the **Marketplace** in your [**Azure Portal**](https://portal.azure.com/#create/glueckkanja-gabag.radiusaas-transactable-prod/preview).&#x20;
* Select the RADIUSaaS **Plan** based on your preferred renewal interval and click "Subscribe"

![](<../.gitbook/assets/image (65).png>)

* Create or select the **Resource group** you would like to deploy the subscription to
* Assign a **Name** to later identify your RADIUSaaS subscription
* We recommend to keep **Recurring billing** on **** so that you do not have to worry about a manual renewal of your subscription.
* Click "Review + subscribe" and in the next blade "Subscribe" to deploy the subscription to your Azure SaaS portal.

![](<../.gitbook/assets/image (77).png>)

{% hint style="info" %}
The random order of **Base Fees** und **Additional Users** under the **Price** information is attributed to limitations of the Azure Marketplace. Later during the the enrolment process, we will provide you with transparent information on the expected licensing costs.
{% endhint %}

* Once the deployment is complete, please navigate to our platform by clicking "Configure account now"

![](<../.gitbook/assets/image (80).png>)

* After authenticating on our platform using your Microsoft credentials, you will be prompted for additional information, such as the desired **total user amount** and a **technical admin contact**.

![](../.gitbook/assets/Screenshot\_2022-02-18\_at\_12\_23\_47.png)

* Based on the amount of users provided on our platform, we will charge the relevant base fee for your user segment as well as additional users, in case you require more than the included amount in your base fee.
* The platform will show you the licensing fees you have to expect under **Cost Projection**.

{% hint style="warning" %}
For CSP deals we cannot display the Cost Projection as CSP margins might be in place.
{% endhint %}

* If you are happy with it, please complete the enrolment by clicking "Submit", which triggers us to set up your RADIUSaaS instance. We will inform you via email with all relevant information on the next steps once the instance is available for you. This won't take any longer than one business day.

{% hint style="info" %}
You will only be charged by Microsoft, once you have completed the enrolment on our landing page and received our welcome email.
{% endhint %}