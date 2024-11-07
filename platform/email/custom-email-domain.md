# Custom email addresses

By default, Mechanic sends mail from an address built from the Shopify store's domain. Mechanic also supports sending from a custom email address.

Configuration for a custom email address always involves adding DNS records to the email address domain. This means that Mechanic's custom email address feature can only be used on an email domain that the store owner controls.

{% hint style="info" %}
Remember, Mechanic only allows transactional mail – this means messages that do not require an unsubscribe link.
{% endhint %}

{% hint style="info" %}
Changing the sender _name_ doesn't require a custom outgoing email address. For that, use the `"from_display_name"` option of the [Email action](../../core/actions/email.md).
{% endhint %}

## Configuration

To change the email address used for outgoing email, open the Mechanic email settings for your store. Or, jump directly to [https://admin.shopify.com/apps/mechanic/settings/email](https://admin.shopify.com/apps/mechanic/settings/email).

<figure><img src="../../.gitbook/assets/Screenshot 2023-03-09 at 4.41.31 PM.png" alt=""><figcaption></figcaption></figure>

## Approval

Mechanic requires approval of custom email addresses before using them for outgoing mail.

Custom email addresses must be manually approved by Mechanic staff.&#x20;

Contact [team@usemechanic.com](mailto:team@usemechanic.com) to have your address approved.

## Verification

Mechanic requires verification of domain name ownership via DNS record. Once a custom email address is configured and approved, Mechanic will provide you with two DNS records to add to your domain. For help on adding these records, see Postmark's documentation: [Resources for adding DKIM and Return-Path records to DNS for common hosts and DNS providers](https://postmarkapp.com/support/article/1090-resources-for-adding-dkim-and-return-path-records-to-dns-for-common-hosts-and-dns-providers).

DMARC is also critical for email deliverability! It's an administrator responsibility for the email domain used for sending. Mechanic can't take care of this for you, but we do have documentation: [learn more about Mechanic and DMARC here](dmarc.md).
