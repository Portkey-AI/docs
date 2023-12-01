# Cache (Simple & Semantic)

Respond to previously served queries from Portkey cache instead of sending them again to your AI provider.&#x20;

**Match exact strings** OR **Semantically similar strings** and serve requests upto **20x times faster** and **cheaper**.

## **Enable Cache in the Config**

To enable Portkey cache, just add the `cache` params to your [config object](../../api-reference/config-object.md#cache-object-details).

### **Semantic Cache**

```json
"cache": {
    "mode": "semantic",
    "maxAge": 10000
}
```

The semantic cache considers the contextual similarity between input prompts. It uses cosine similarity to ascertain if the similarity between the input and a cached request exceeds a specific threshold. If the similarity threshold is met, Portkey retrieves the response from the cache, saving model execution time. Check out this [blog](https://portkey.ai/blog/reducing-llm-costs-and-latency-semantic-cache/) for more details.

### **Simple Cache**

```sh
"cache": {
    "mode": "simple",
    "maxAge": 10000
}
```

Simple cache performs an exact match on the input prompts. If the exact same request is received again, Portkey retrieves the response directly from the cache, bypassing the model execution. This approach is straightforward and effective for repeated identical requests.

## **Cache Dashboard & Logs**

Simple cache hits will show up as **"HIT"** & semantic cache hits will show up as **"SEMANTIC HIT"** on your Portkey logs. We also calculate and show the response time and how much money you saved with each hit.

You can also view the hit rate and cost savings from cache on the [Analytics dashboard](../observability-modern-monitoring-for-llms/analytics.md#cache).
