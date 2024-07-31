---
description: Easily access your Portkey logs data for further analysis and reporting
---

# Logs Export

{% hint style="success" %}
Logs export feature is only available for [**Pro**](https://portkey.ai/pricing)[**duction**](https://portkey.ai/pricing) and [**Enterpris**](https://portkey.ai/docs/product/enterprise-offering)[**e**](https://portkey.ai/docs/product/enterprise-offering) users.
{% endhint %}

At Portkey, we understand the importance of data analysis and reporting for businesses and teams. That's why we provide a comprehensive logs export feature for our paid users. With this feature, you can easily request and obtain your Portkey logs data in a **structured JSON** format, allowing you to gain valuable insights into your LLM usage, performance, costs, and more.

## Requesting Logs Export

To submit a data export request, simply follow these steps:

1. Ensure you are an admin of your organization on Portkey.
2. Send an email to [support@portkey.ai](mailto:support@portkey.ai) with the subject line `Logs Export - [Your_Organization_Name]`.
3. In the email body,
   * Specify the **time frame** for which you require the logs data. **Pro plan** supports logs export of **last 30** days.
   * Share names of the **specific columns** you require (see the "[Exported Data](logs-export.md#exported-data)" section below for a complete list of available columns).
4. Our team will process your request and provide you with the exported logs data in JSON format.

{% hint style="warning" %}
Note: Porktye only supports data exports in the <mark style="color:blue;">**`JSONL`**</mark> format, and can not process exports in any other formats at the moment.
{% endhint %}

## Exported Data

The exported logs data will include the following columns:

<table><thead><tr><th width="214">Column Name</th><th>Column Description / Property</th></tr></thead><tbody><tr><td>created_at</td><td>Timestamp of the request</td></tr><tr><td>request.body</td><td>Request JSON payload (as seen in the Portkey logs)</td></tr><tr><td>response.body</td><td>Response JSON payload (as seen in the Portkey logs)</td></tr><tr><td>is_success</td><td>Request success status (1 = success, 0 = failure)</td></tr><tr><td>ai_org</td><td>AI provider name</td></tr><tr><td>ai_model</td><td>AI model name</td></tr><tr><td>req_units</td><td>Number of tokens in the request</td></tr><tr><td>res_units</td><td>Number of tokens in the response</td></tr><tr><td>total_units</td><td>Total number of tokens (request + response)</td></tr><tr><td>cost</td><td>Cost of the request in cents (USD)</td></tr><tr><td>cost_currency</td><td>Currency of the cost (USD)</td></tr><tr><td>request_url</td><td>Final provider API URL</td></tr><tr><td>request_method</td><td>HTTP request method</td></tr><tr><td>response_status_code</td><td>HTTP response status code</td></tr><tr><td>response_time</td><td>Response time in milliseconds</td></tr><tr><td>cache_status</td><td>Cache status (SEMANTIC HIT, HIT, MISS, DISABLED)</td></tr><tr><td>cache_type</td><td>Cache type (SIMPLE, SEMANTIC)</td></tr><tr><td>stream_mode</td><td>Stream mode status (TRUE, FALSE)</td></tr><tr><td>retry_success_count</td><td>Number of retries after which request was successful</td></tr><tr><td>trace_id</td><td>Trace ID for the request</td></tr><tr><td>mode</td><td>Config top level strategy (SINGLE, FALLBACK, LOADBALANCE)</td></tr><tr><td>virtual_key</td><td>Virtual key used for the request</td></tr><tr><td>runtime</td><td>Runtime environment</td></tr><tr><td>runtime_version</td><td>Runtime environment version</td></tr><tr><td>sdk_version</td><td>Portkey SDK version</td></tr><tr><td>config</td><td>Config ID used for the request</td></tr><tr><td>prompt_slug</td><td>Prompt ID used for the request</td></tr><tr><td>prompt_version_id</td><td>Version number of the prompt template slug</td></tr><tr><td>metadata.key</td><td>Custom metadata key</td></tr><tr><td>metadata.value</td><td>Custom metadata value</td></tr></tbody></table>

With this comprehensive data, you can analyze your API usage patterns, monitor performance, optimize costs, and make data-driven decisions for your business or team.

## Support

If you have any questions or need assistance with the logs export feature, reach out to the Portkey team at support@portkey.ai or hop on to our [Discord server](https://portkey.ai/community).
