# 🚪 AI Gateway

This is the most important functionality that Portkey provides, and also where Portkey solve the most impactful production challenges for your LLM app.

### Portkey's AI Gateway equips your app with the following functionalities:

[**Automatic Retries with Exponential Backoff**](automatic-retries.md)

When your requests fail for no reason or due to server overload, Portkey automatically retries your requests with exponential backoff, ensuring that the request gets served.

[**Simple & Semantic Caching**](request-caching.md)

When your app is running at scale, you are likely to encounter identical requests you have already served previously. By caching, you can save up on costs for such requests and serve them 20x times faster.&#x20;

[**Automatic Fallbacks**](fallbacks-on-llms.md)

In cases of primary model being down, automatically send your request to a fallback model, ensuring reliability.

[**Load Balancing**](load-balancing.md)

This unique feature helps you stay within your rate limit and queuing thresholds by distributing your requests across different model/providers **and** different accounts of the same providers&#x20;
