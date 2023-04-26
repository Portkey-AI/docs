# Caching

Portkey supports caching across text & chat completions. When the exact same request comes in to Portkey, we can return the response from our cache.

This could be useful if you have fixed input prompts or are testing the app with the same inputs.

### Enabling cache

To enable caching, pass the following header in your requests.

```
    "x-portkey-cache": true
    "Cache-Control": "max-age:1000"
```

The `x-portkey-cache` enables or disables cache storage and retrieval. The `Cache-Control` header takes a `max-age` in seconds. The minimum value for `Cache-Control` is 30. If you don't provide this header, we will automatically cache requests for `7*24*60*60 seconds` (7 days).

### Invalidating Cache

You can choose to force refresh cache by using the `x-portkey-cache-force-refresh` header. Setting it to `true` ensures that the cache is invalidated, and a new value is stored in the cache.

```
    "x-portkey-cache-force-refresh": true
```
