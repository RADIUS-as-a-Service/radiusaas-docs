---
description: >-
  This section handles certificates which are allowed to authenticate. Every
  certificate derived from any of the CAs listed below will get access.
---

# Trusted Roots for Client Authentication

## Add

To **Add** a trusted root, click **Add,** select the import format/source and click **Save.**&#x20;

{% hint style="warning" %}
If you have a tiered PKI infrastructure (e.g. a **Microsoft PKI**), remember to **upload** the **entire chain**, i.e. the root CA certificate as well as all relevant issuing CA certificates (RADIUSaaS supports the upload of a combined file or of separate files for each CA)..
{% endhint %}

![](<../../../.gitbook/assets/add-root (1).gif>)

## Delete

To **Delete** one of your root CAs, expand the corresponding row, click **Delete** and confirm your choice.

![](<../../../.gitbook/assets/image (81) (1).png>)
