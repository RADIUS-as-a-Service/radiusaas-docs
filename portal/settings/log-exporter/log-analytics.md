# Log Analytics

### Required Values

* Customer ID
* Shared Key
* Event Name

### Data

Some data which you might send to your Log Analytics workspace will include new line characters. \
To get a valid json for every entry, the template engine has a global "tojson" parser which will apply for all variables you access.&#x20;

Therefore don't quote any jinja variable.

## Correct

<figure><img src="../../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

## Wrong

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>
