# Groups

The Groups tab centralizes reusable named sets of values, so a rule references the group instead of listing raw values directly. RADIUSaaS offers six kinds:

* SSID groups
* Access-point MAC groups
* Switch groups
* NAS identifier groups
* NAS IP address groups
* Client IP groups

Members can be added or removed from a group later without touching any rule that references it.

<figure><img src="../../../.gitbook/assets/image (592).png" alt=""><figcaption></figcaption></figure>

### Creating a Group

Creating a group asks for a name and an optional description, plus the members themselves, which can be added one at a time or pasted in bulk.

<figure><img src="../../../.gitbook/assets/image (563).png" alt=""><figcaption></figcaption></figure>

### Paste a List

To enter group members in bulk, you use the "Paste a list..." option to then paste your desired entries. The entries can be either one per line or comma separated.

<figure><img src="../../../.gitbook/assets/image (565).png" alt=""><figcaption></figcaption></figure>

Clicking Add to list will then deduplicate the entries and add them to the list of members.

### Prefixes

Some of the groups support prefixing to wild-card match multiple values. These are SSIDs, NAS-Identifiers and all MAC-based groups.

Such prefixed members will be shown with the "PREFIX" tag:

<figure><img src="../../../.gitbook/assets/image (566).png" alt=""><figcaption></figcaption></figure>

### MAC Addresses

For MAC address-based groups, different formats are supported and will be normalised on save.

Any separator, or none at all, will work as well as prefixing the addresses.

<figure><img src="../../../.gitbook/assets/image (567).png" alt=""><figcaption></figcaption></figure>

### IP Addresses

For IP address-based groups, a single address (10.20.0.11) or a CIDR range (10.30.0.0/16). IPv4 and IPv6 both work.

<figure><img src="../../../.gitbook/assets/image (569).png" alt=""><figcaption></figcaption></figure>
