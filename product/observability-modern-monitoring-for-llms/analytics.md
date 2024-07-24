# Analytics

{% hint style="success" %}
This feature is available for all plans:-

* [Developer](https://app.portkey.ai/): 30 days retention
* [Production](https://app.portkey.ai/): 365 days retention
* [Enterprise](https://portkey.ai/docs/product/enterprise-offering): Unlimited
{% endhint %}

As soon as you integrate Portkey, you can start to view detailed & real-time analytics on cost, latency and accuracy across all your LLM requests.

The analytics dashboard provides an interactive interface to understand your LLM application  Here, you can see various graphs and metrics related to requests to different LLMs, costs, latencies, tokens, user activity, feedback, cache hits, errors, and much more.

The metrics in the Analytics section can help you understand the overall efficiency of your application, discover patterns, identify areas of optimization, and much more.

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption><p>Analytics Overview Dashboard</p></figcaption></figure>

## Charts

The dashboard provides insights into your [users](analytics.md#users), [errors](analytics.md#errors), [cache](analytics.md#cache), [feedback](analytics.md#feedback) and also summarizes information by [metadata](analytics.md#metadata-summary).

### Overview

The overview tab is a 70,000ft view of your application's performance. This highlights the cost, tokens used, mean latency, requests and information on your users and top models.

This is a good starting point to then dive deeper.

### Users

The users tab provides an overview of the user information associated with your Portkey requests. This data is derived from the `user` parameter in OpenAI SDK requests or the special `_user` key in the Portkey [metadata header](metadata.md).

{% hint style="info" %}
Portkey currently does not provide analytics on usage patterns for individual team members in your Portkey organization. The users tab is designed to track end-user behavior in your application, not internal team usage.
{% endhint %}

<figure><img src="../../.gitbook/assets/CleanShot 2024-05-20 at 19.03.50@2x (1).png" alt=""><figcaption></figcaption></figure>

### Errors

Portkey captures errors automatically for API and Accuracy errors. The charts give you a quick sense of error rates allowing you to debug further when needed.

The dashboard also shows you the number of requests rescued by Portkey through the various AI gateway strategies.

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Error Analytics Dashboard</p></figcaption></figure>

### Cache

When you enable cache through the AI gateway, you can view data on the latency improvements and cost savings due to cache.

### Feedback

Portkey allows you to collect feedback on LLM requests through the logs dashboard or via API. You can view analytics on this feedback collected on this dashboard.

### Metadata Summary

Group your request data by metadata parameters to unlock insights on usage. Select the metadata property to use in the dropdown and view the request data grouped by values of that metadata parameter.

This lets you answer questions like:

1. Which users are we spending the most on?
2. Which organisations have the highest latency?

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption><p>Metadata Analytics</p></figcaption></figure>
