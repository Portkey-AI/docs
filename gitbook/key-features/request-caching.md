# 🚀 Request Caching

In the quest for enhancing performance and optimizing response retrieval, Portkey offers two unique types of caching - Fixed String Matching Cache (Simple Cache) and Semantic Cache.

### Fixed String Matching Cache

Fixed String Matching Cache, also known as a simple cache, performs an exact match on the input prompts. If the exact same request is received again, Portkey retrieves the response directly from the cache, bypassing the model execution. This approach is straightforward and effective for repeated identical requests.

#### Enabling Fixed String Matching Cache

To enable this cache, include the following headers in your requests:

```json
{
    "x-portkey-cache": "simple"
}
```

### Semantic Cache

Portkey's Semantic Cache extends beyond exact string matching. This advanced caching mechanism considers the contextual similarity between input prompts. It uses cosine similarity to ascertain if the similarity between the input and a cached request exceeds a specific threshold. If the similarity threshold is met, Portkey retrieves the response from the cache, saving model execution time.

#### Enabling Semantic Cache

To enable the Semantic Cache, use the `x-portkey-cache` header in your requests, and set it to `semantic`.

```json
{
    "x-portkey-cache": "semantic"
}
```



**Cache TTL**

The `Cache-Control` header specifies the maximum age of the cached response in seconds. If the `Cache-Control` header is not provided, Portkey automatically caches requests for 604800 seconds (7 days). Minimum max-age allowed is 60 seconds.

```json
{
    "Cache-Control": "max-age=1000"
}
```



#### Invalidating Fixed String Matching Cache

If you need to force a cache refresh, use the `x-portkey-cache-force-refresh` header. Setting this to `true` ensures the cache is invalidated, and a new value is stored in the cache.

```json
{
    "x-portkey-cache-force-refresh": true
}
```

By using Semantic Cache, you can optimize the caching process by considering the contextual similarity of input prompts, leading to more efficient response retrieval.

Choosing the right caching mechanism based on your specific use case can greatly enhance performance and minimize unnecessary model executions, making Portkey an effective tool for managing your Generative AI applications.

### **🖥️ Portkey Dashboard Guide**

Simple cache hits will show up as **"HIT"** & semantic cache hits will show up as **"SEMANTIC HIT"** on your Portkey logs. We also calculate and show the response time and how much money you saved with each hit.

<figure><img src="../.gitbook/assets/Cache 1 (3).png" alt=""><figcaption><p>See cache status directly on your Portkey logs</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Cache 2.png" alt=""><figcaption><p>Each log also shows the response time for cache hit</p></figcaption></figure>

