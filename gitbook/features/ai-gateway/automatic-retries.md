# 💫 Automatic Retries

Portkey has an in-built retry mechanism that makes sure your requests get fulfilled. We implement a retry mechanism with exponential backoff. This can be managed by the `x-portkey-retry-count` header.

```
{
    "x-portkey-retry-count": 4
}
```

{% hint style="info" %}
The max value of retry-count is 5
{% endhint %}

