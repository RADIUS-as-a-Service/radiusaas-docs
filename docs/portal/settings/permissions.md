# Permissions

## Overview

The permissions menu allows you to configure the permissions for the access to the RADIUSaaS admin portal.

{% hint style="success" %}
RADIUSaaS leverages Microsoft Entra ID (Azure AD) as an identity provider for the logon-authentication to the admin portal. It does not store or manage its own administrator identities. The authentication is delegated to the corresponding Microsoft Entra ID (Azure AD) tenant of the provided UPN.

Therefore administrators enjoy the comfort of working with their own Microsoft Entra ID (Azure AD) accounts and do not have to setup additional accounts.
{% endhint %}

![](<../../.gitbook/assets/image (71) (1).png>)

Please enter Microsoft Entra ID (Azure AD) **UPNs** in the provided fields and **separate them with comma and space**.&#x20;

Example:

```
john.doe@contoso.com, peter.pan@contoso.com, jane.doe@other-aad-tenant.com
```

The wildcard "\*" is also supported, and may be used to allow all users from a certain domain.

Example:

```
*@contoso.com
```

## Roles

### Administrators

Microsoft Entra ID (Azure AD) UPNs entered in this field have permission to access the RADIUSaaS admin portal. Permissions include:

* See the dashboard and logging
* See and change users
* See and change settings including permissions

### Viewers

Users can see everything but are not able to make any changes

### Users

Users can access the Users portal, where they are able to create Users for them or their guests
