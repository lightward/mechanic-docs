# Custom email addresses

By default, Mechanic sends your mail from an address built from your Shopify account name. If your store is located at fantastic.myshopify.com, for example, Mechanic will send mail from [fantastic@mail.usemechanic.com](mailto:fantastic@mail.usemechanic.com). However, we do support sending from a custom email address, using your store's domain.

_Remember, Mechanic only allows transactional mail – that means messages that do not require an unsubscribe link. This is a restriction carried over from Postmark, our email provider._ [_Read more here._](https://docs.usemechanic.com/article/348-what-kind-of-email-can-i-send-with-mechanic)

To change your Mechanic account's "From" address, head to [the "Settings" area](https://docs.usemechanic.com/article/430-accessing-your-mechanic-account-settings). Look for the "Email" section, and add the email address that you'd like to use for your outgoing mail.

[![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e3c2f1a04286364bc94f42e/file-DqoBFym1FO.png)](https://docs.usemechanic.com/article/348-what-kind-of-email-can-i-send-with-mechanic)

Only email addresses that share a domain name with your Shopify store are pre-approved. \(For example, if your store has the domain name "example.com", you're pre-approved to use email addresses like "support@example.com", or "ops@example.com", etc.\) For all other domains, your email address will need to be approved by a Mechanic admin. Use the support button in the corner to request approval.

Once the address is approved, Mechanic will show you information for two new DNS records.

![](https://d33v4339jhl8k0.cloudfront.net/docs/assets/5ddd799f2c7d3a7e9ae472fc/images/5e3c30b22c7d3a7e9ae75674/file-H0hDxz239h.png)

Add these to your domain, verify them, and Mechanic will start sending mail from your selected address. For more help with this, see Postmark's documentation: [Resources for adding DKIM and Return-Path records to DNS for common hosts and DNS providers](https://postmarkapp.com/support/article/1090-resources-for-adding-dkim-and-return-path-records-to-dns-for-common-hosts-and-dns-providers).

P.S. Changing the sender _name_ doesn't require any of these steps – for that, you can use the `"from_display_name"` option of the "[email" action](../core-concepts/actions/action-types/email.md). The steps above are only required if you want to change the sender's email address. :\)

