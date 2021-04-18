# Custom email addresses

By default, Mechanic sends mail from an address built from the Shopify store's domain. \(A store located at fantastic.myshopify.com, for example, will have mail sent from [fantastic@mail.usemechanic.com](mailto:fantastic@mail.usemechanic.com).\) Mechanic also supports sending from a custom email address.

Configuration for a custom email address always involves adding DNS records to the email address domain. This means that Mechanic's custom email address feature can only be used on an email domain that the store owner controls.

{% hint style="info" %}
Remember, Mechanic only allows transactional mail – that means messages that do not require an unsubscribe link.
{% endhint %}

## Approval

Mechanic validates custom email addresses, before allowing them to be used for outgoing mail.

The address will be auto-approved if one of the following is true:

* If the custom address matches the store owner's email address
* If the custom address has the same domain name as the store's primary domain name
* If the custom address is on a subdomain of the store's primary domain name

In all other cases, custom email addresses must be manually approved by Mechanic staff. Contact [team@usemechanic.com](mailto:team@usemechanic.com) to have your address approved.

## Configuration

To change your Mechanic account's "From" address, open the Mechanic settings area. Look for the "Email" section, and add the email address that you'd like to use for your outgoing mail.

[![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e3c2f1a04286364bc94f42e/file-DqoBFym1FO.png)](https://docs.usemechanic.com/article/348-what-kind-of-email-can-i-send-with-mechanic)

Only email addresses that share a domain name with your Shopify store are pre-approved. \(For example, if your store has the domain name "example.com", you're pre-approved to use email addresses like "support@example.com", or "ops@example.com", etc.\) For all other domains, your email address will need to be approved by a Mechanic admin. Use the support button in the corner to request approval.

Once the address is approved, Mechanic will show you information for two new DNS records.

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e3c30b22c7d3a7e9ae75674/file-H0hDxz239h.png)

Add these to your domain, verify them, and Mechanic will start sending mail from your selected address. For more help with this, see Postmark's documentation: [Resources for adding DKIM and Return-Path records to DNS for common hosts and DNS providers](https://postmarkapp.com/support/article/1090-resources-for-adding-dkim-and-return-path-records-to-dns-for-common-hosts-and-dns-providers).

P.S. Changing the sender _name_ doesn't require any of these steps – for that, you can use the `"from_display_name"` option of the [Email action](../../core/actions/types/email.md). The steps above are only required if you want to change the sender's email address.

