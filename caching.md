# Caching in Portkey

Portkey offers two types of caching to enhance performance and optimize response retrieval: fixed string matching cache(simple) and semantic cache.

## Fixed String Matching Cache
The fixed string matching cache is the traditional caching mechanism where an exact match is performed on the input prompts. If the exact same request is received again, Portkey can directly return the response from the cache without executing the model.

### Enabling Fixed String Matching Cache
To enable the fixed string matching cache, include the following headers in your requests:

```sh
"x-portkey-cache": "simple"
"Cache-Control": "max-age:1000"
```
The x-portkey-cache header enables or disables the cache storage and retrieval. The Cache-Control header accepts the max-age parameter in seconds, which specifies the maximum age of the cached response. If the Cache-Control header is not provided, Portkey will automatically cache requests for 7*24*60*60 seconds (7 days) when x-portkey-cache is set to true.

### Invalidating Fixed String Matching Cache
You can force refresh the fixed string matching cache by using the x-portkey-cache-force-refresh header. Setting it to true ensures that the cache is invalidated, and a new value is stored in the cache.

```sh
"x-portkey-cache-force-refresh": true
```

## Semantic Cache
The semantic cache in Portkey goes beyond exact string matching and takes into account the contextual similarity between input prompts. It uses cosine similarity to determine if the similarity between the input and a cached request exceeds a certain threshold. If the similarity threshold is met, Portkey retrieves the response from the cache.

### Enabling Semantic Cache
To enable the semantic cache feature, use the following header in your requests:

```sh
"x-portkey-cache": "semantic"
```

Setting the x-portkey-cache header to "semantic" enables the semantic cache functionality.

### Implementation Details
When utilizing the semantic cache, it's important to note that the Cache-Control header is still applicable to control the maximum age of the cached response.

If you wish to force refresh the semantic cache and invalidate existing entries, you can use the x-portkey-cache-force-refresh header as described earlier.

By leveraging the semantic cache, you can optimize the caching process by considering the contextual similarity of input prompts, leading to more efficient response retrieval.

Choose the appropriate caching mechanism based on your use case to improve performance and minimize unnecessary model executions in Portkey.
