# Automatic Retries

LLM APIs often have inexplicable failures. With Portkey, you can rescue a substantial number of your requests with our in-built automatic retries feature.&#x20;

* Automatic retries are triggered **up to 5 times**
* Retries can also be triggered only on **specific error codes**
* And each subsequent retry attempt follows **exponential backoff strategy** to prevent network overload

## Enabling Retries

To enable retry, just add the `retry` param to your [config object](../../api-reference/config-object.md).

### Retry with 5 attempts

```json
{
    "retry": { "attempts": 5 },
    "virtual_key": "virtual-key-xxx"
}
```

### Retry only on specific error codes

```json
{
  "retry": {
    "attempts": 3,
    "on_status_codes": [
      408, 429, 401
    ]
  },
  "virtual_key": "virtual-key-xxx"
}
```

{% hint style="info" %}
The **`on_status_codes`** parameter is optional. You can use it to specify which errors trigger retries. Without it, retries will be triggered for every error.
{% endhint %}

### Exponential backoff strategy

Here's how Portkey triggers retries following exponential backoff:

<table><thead><tr><th width="249">Attempt</th><th>Time delay between requests</th></tr></thead><tbody><tr><td>Initial Call</td><td>Immediately</td></tr><tr><td>Retry 1st attempt</td><td>1 second</td></tr><tr><td>Retry 2nd attempt</td><td>2 seconds</td></tr><tr><td>Retry 3rd attempt</td><td>4 seconds</td></tr><tr><td>Retry 4th attempt</td><td>8 seconds</td></tr><tr><td>Retry 5th attempt</td><td>16 seconds</td></tr></tbody></table>
