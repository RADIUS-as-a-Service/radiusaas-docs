# Changelog

## Versions

### June 2024 Release

* Updated UI (reactive design, improved [Rule Engine](../admin-portal/settings/rules/) structure, separation of [RADIUS server certificates](../admin-portal/settings/settings-server.md#server-certificates) from [Trusted Certificates](../admin-portal/settings/trusted-roots.md) for client authentication and RadSec)
* Certificate verification can now be configured individually for each trusted CA (client authentication, RadSec) using OCSP, **CRL** or OCSP auto-detect (based on the certificate's AIA extension)
* OCSP Soft-Fail / Hard-Fail is now configurable on a per trusted CA basis.
* Introduction of [RADIUSaaS REST API documentation](rest-api.md) and [API access tokens](../admin-portal/settings/permissions.md#access-tokens).
* Improved logging:&#x20;
  * Added Accounting and [RadSec connection logs](../admin-portal/insights/log.md#log-types).
