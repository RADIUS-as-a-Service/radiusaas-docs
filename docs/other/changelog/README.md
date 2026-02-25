# Changelog

{% hint style="success" %}
If you'd like to **stay up to date on the latest changes and news in the RADIUSaaS changelog**, you can subscribe to our email update service. Subscribers will receive **email notifications** when there are new updates to the changelog.

[**Please click here to sign up for email notifications.**](https://feedback.radiusaas.com)
{% endhint %}

## Versions

### March 2026 Release

#### Schedule

* Roll-out start: 2026-02-26
* Roll-out end: 2026-03-05

#### Fixes

* Fixed VLAN-related attributes not being populated in an RFC-compliant way
* Fixed incorrect expiration date in access token table
* Several bug fixes in configuring the [SCEPman Connection](../../admin-portal/settings/settings-server.md#scepman-connection)

#### New Features

* [VLAN](../../admin-portal/settings/rules/general-structure.md#vlan-attributes) and [additional return attributes (VSAs)](../../admin-portal/settings/rules/general-structure.md#radius-attributes) can now be managed directly via the RADIUSaaS Admin Portal
* Removed RADIUS server certificate slot limitation
* Popup Notifications:
  * Added certificate expiry and other important alerts
  * Added configuration improvement recommendations

### January 2026 Release

#### Schedule

* Roll-out start: 2026-01-21
* Roll-out end: 2026-01-28

#### Fixes

* RADIUS Server: Remove all attributes from Access-Reject to ensure RFC compliance
* RADIUS Server: Set Framed-MTU to value of last request
* Allow CSV import of AP and switch groups
* Fix issue when launching RaaS from MyApps

#### New Features

* SCEPman-as-a-Service (Public beta). Please [contact us](https://support.scepman.com/support/tickets/new?ticket_form=drop_a_question_%28scepman%29&_gl=1*1ufiojk*_ga*MTEzNjMwODM1MC4xNzY5MDUyMzAy*_ga_BRDGXLVK3H*czE3NjkwNTIzMDEkbzEkZzAkdDE3NjkwNTIzMDEkajYwJGwwJGgw) to get access.
* Automatic server certificate management using [SCEPman](../../admin-portal/settings/settings-server.md#scepman-connection)
* Allow adding multiple instances of the same return attribute

### July 2025 Release

#### Schedule

* Roll-out start: 2025-07-14
* Roll-out end: 2025-07-17

#### New Features

* More [IDP options](../../admin-portal/settings/permissions.md#supported-idps) for administrative SSO
* [Revoked certificate cache](../faqs/log-and-common-errors.md#certificate-status-was-revoked-previously)
* Directly [create tickets or report incidents](../../admin-portal/home.md#help) from the RADIUSaaS Admin Portal
* Improved [notification system](../../admin-portal/home.md#notifications)

#### Upcoming Service Updates

* Preparation for [upcoming IP address change of RADIUS Proxies](upcoming-change-radius-proxy-ip-address-migration.md)

### February 2025 Release

#### Schedule

* Roll-out start: 2025-02-27
* Roll-out end: 2025-03-06

#### Fixes

* RADIUS Server: Incoming traffic is not processed in some rare scenarios
* RADIUS Proxy: Version update / increased stability

#### New Features

* Status endpoint that enables the usage of external monitoring systems (certificates, proxies and RadSec servers)
* Optimized handling of invalid OCSP responses
* Support for [Authorized Responders](https://docs.scepman.com/advanced-configuration/application-settings/ocsp#appconfig-ocsp-useauthorizedresponder) in OCSP
* Display CRL download issues
* WPA2/WPA3 selector for XML generator

### December 2024 Release

* Custom logo experience for invited users in web UI
* Technical contacts can be configured per RADIUSaaS tenant. This is a preparation for a notification feature in a future release of RADIUSaaS.
* Support for FreshestCRL (DeltaCRL) in Trusted Certificates
* Support for parentheses in CRL URL
* Updated default templates for LogExporter
* Fixed: On rare occasions, the RADIUSaaS instances may be affected by a performance bottleneck for authentications.

### June 2024 Release

* Updated UI (reactive design, improved [Rule Engine](../../admin-portal/settings/rules/) structure, separation of [RADIUS server certificates](../../admin-portal/settings/settings-server.md#server-certificates) from [Trusted Certificates](../../admin-portal/settings/trusted-roots.md) for client authentication and RadSec)
* Certificate verification can now be configured individually for each trusted CA (client authentication, RadSec) using OCSP, **CRL** or OCSP auto-detect (based on the certificate's AIA extension)
* OCSP Soft-Fail / Hard-Fail is now configurable on a per trusted CA basis.
* Introduction of [RADIUSaaS REST API documentation](../rest-api/) and [API access tokens](../../admin-portal/settings/permissions.md#access-tokens).
* Improved logging:&#x20;
  * Added Accounting and [RadSec connection logs](../../admin-portal/insights/log.md#log-types).
