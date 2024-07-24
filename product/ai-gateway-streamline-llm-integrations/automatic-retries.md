# Automatic Retries

{% hint style="success" %}
This feature is available on all Portkey [plans](https://portkey.ai/pricing).
{% endhint %}

LLM APIs often have inexplicable failures. With Portkey, you can rescue a substantial number of your requests with our in-built automatic retries feature.&#x20;

* Automatic retries are triggered **up to 5 times**
* Retries can also be triggered only on **specific error codes**
* And each subsequent retry attempt follows **exponential backoff strategy** to prevent network overload

## Enabling Retries

To enable retry, just add the `retry` param to your [config object](../../api-reference/config-object.md).

### Retry with 5 attempts

<pre class="language-json"><code class="lang-json">{
    "retry": {
<strong>        "attempts": 5
</strong>    },
    "virtual_key": "virtual-key-xxx"
}
</code></pre>

### Retry only on specific error codes

By default, Portkey triggers retries on the following error codes: **\[429, 500, 502, 503, 504]**

You can change this behaviour by setting the optional **`on_status_codes`** param in your retry config and manually inputting the error codes on which rety will be triggered.

<pre class="language-json"><code class="lang-json">{
  "retry": {
    "attempts": 3,
<strong>    "on_status_codes": [ 408, 429, 401 ]
</strong>  },
  "virtual_key": "virtual-key-xxx"
}
</code></pre>

{% hint style="info" %}
If the `on_status_codes` param is present, retries will be triggered **only** on the error codes specified in that Config and not on Portkey's default error codes for retries (i.e. \[429, 500, 502, 503, 504])
{% endhint %}

### Exponential backoff strategy

Here's how Portkey triggers retries following exponential backoff:

<table><thead><tr><th width="249">Attempt</th><th>Time out between requests</th></tr></thead><tbody><tr><td>Initial Call</td><td>Immediately</td></tr><tr><td>Retry 1st attempt</td><td>1 second</td></tr><tr><td>Retry 2nd attempt</td><td>2 seconds</td></tr><tr><td>Retry 3rd attempt</td><td>4 seconds</td></tr><tr><td>Retry 4th attempt</td><td>8 seconds</td></tr><tr><td>Retry 5th attempt</td><td>16 seconds</td></tr></tbody></table>
