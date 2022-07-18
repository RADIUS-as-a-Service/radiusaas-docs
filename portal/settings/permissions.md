# Permissions

The permissions menu allows you to configure the permissions for the access to the RADIUSaaS admin portal.

{% hint style="success" %}
RADIUSaaS leverages Azure AD as identity provider for the logon-authentication to the admin portal. It does not store or manage own administrator identities. The authentication is delegated to the corresponding Azure AD tenant of the provided UPN.

Therefore administrators enjoy the comfort of working with their own Azure AD accounts and do not have to setup additional accounts.
{% endhint %}

![](<../../.gitbook/assets/image (71) (1).png>)

Please enter **Azure AD UPNs** in the provided fields and **separate them with comma and space**.&#x20;

Example:

```
john.doe@contoso.com, peter.pan@contoso.com, jane.doe@other-aad-tenant.com
```

The wildcard "\*" is also supported, and may be used to allow all users from a certain domain.

Example:

```
*@contoso.com
```

### Administrators

Azure AD UPNs entered in this field have the permission to access the RADIUSaaS admin portal. Permissions include:

* See dashboard and logging
* See and change users
* See and change settings including permissions

### Viewers

Users can see everything but are not able to make any changes

### Users

Users can access the Users portal, where they are able to create Users for them or their guests
