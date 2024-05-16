---
description: Budget Limits lets you set cost limits on virtual keys
---

# Budget Limits

**Budget Limits on Virtual Keys** provide a simple way to manage your spending on AI providers (and LLMs) - giving you confidence and control over your application's costs.

{% hint style="success" %}
Budget Limit is currently only available to **Portkey Enterprise Plan** customers. Email us at `support@portkey.ai` if you would like to enable it for your org.
{% endhint %}

### Setting Budget Limits on Virtual Keys

When creating a new virtual key on Portkey, you can set a budget limit in USD. Once the limit is reached, the key automatically expires, preventing further usage and overspending.

<figure><img src="../../../.gitbook/assets/CleanShot 2024-05-16 at 08.26.15@2x.png" alt=""><figcaption></figcaption></figure>

### Key Considerations

{% hint style="warning" %}
* Budget limits are set in **USD** and can include decimal values
* The budget limit **applies only** to requests made after the limit is set; it **does not retroactively** apply to previous requests
* Once set, budget limits **cannot be edited** by any organization member
* The minimum budget limit you can set is **$1**
* Budget limits work for **all providers** available on Portkey and apply to **all organization members** who use the virtual key
{% endhint %}

### Editing Budget Limits

If you need to change or update a budget limit, you can **duplicate** the existing virtual key and create a new one with the desired limit.

### Monitoring Your Spending

You can track your spending for any specific virtual key by navigating to the Analytics tab and filtering by the **desired key** and **timeframe**.&#x20;

### Pricing Support and Limitations

Budget limits currently apply to all providers and models for which Portkey has pricing support. If a specific request log shows **`0 cents`** in the COST column, it means that Portkey does not currently track pricing for that model, and it will not count towards the virtual key's budget limit.

It's important to note that budget limits cannot be applied retrospectively. The spend counter starts from zero only after you've set a budget limit for a key.

### Availability

Budget Limits is currently available **exclusively to Portkey Enterprise** customers. If you're interested in enabling this feature for your account, please reach out us at [support@portkey.ai](mailto:support@portkey.ai) or join the [Portkey Discord](https://portkey.ai/community) community.

### Enterprise Plan

To discuss Portkey Enterprise plan details and pricing, [you can schedule a quick call here](https://calendly.com/rohit-portkey/noam).
