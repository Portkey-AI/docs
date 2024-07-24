# Request Timeouts

{% hint style="success" %}
This feature is available on all Portkey [plans](https://portkey.ai/pricing).
{% endhint %}

Manage unpredictable LLM latencies effectively with Portkey's **Request Timeouts**. This feature allows automatic termination of requests that exceed a specified duration, letting you gracefully handle errors or make another, faster request.&#x20;

## Enabling Request Timeouts

You can enable request timeouts while **making your request** or you can **set them in Configs**.&#x20;

{% hint style="info" %}
Request timeouts are specified in **milliseconds** (`integer)`&#x20;
{% endhint %}

### While Making Request

Set request timeout while instantiating your Portkey client or if you're using the REST API, send the `x-portkey-request-timeout` header.

{% tabs %}
{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

# Construct a client with a virtual key
portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="VIRTUAL_KEY",
<strong>    request_timeout=3000
</strong>)

completion = portkey.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'gpt-4o-mini'
)
</code></pre>
{% endtab %}

{% tab title="Node" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai';

// Construct a client with a virtual key
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "VIRTUAL_KEY",
<strong>    requestTimeout: 3000
</strong>})

const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-4o-mini',
});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="REST" %}
<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: openai-virtual-key" \
<strong>  -H "x-portkey-request-timeout:5000" \
</strong>  -d '{
    "model": "gpt-4o-mini",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
</code></pre>
{% endtab %}
{% endtabs %}

### With Configs

In Configs, request timeouts are set at either (1) strategy level, or (2) target level.

For a 10-second timeout, it will be:

```json
"request_timeout": 10000
```

### Setting Request Timeout at Strategy Level

<pre class="language-json"><code class="lang-json">{
  "strategy": { "mode": "fallback" },
<strong>  "request_timeout": 10000,
</strong>  "targets": [
    { "virtual_key": "open-ai-xxx" },
    { "virtual_key": "azure-open-ai-xxx" }
  ]
}
</code></pre>

Here, the request timeout of 10 seconds will be applied to \***all**\* the targets in this Config.

### Setting Request Timeout at Target Level

<pre class="language-json"><code class="lang-json">{
  "strategy": { "mode": "fallback" },
  "targets": [
<strong>    { "virtual_key": "open-ai-xxx", "request_timeout": 10000, },
</strong><strong>    { "virtual_key": "azure-open-ai-xxx", "request_timeout": 2000,}
</strong>  ]
}
</code></pre>

Here, for the first target, a request timeout of 10s will be set, while for the second target, a request timeout of 2s will be set.

{% hint style="info" %}
Nested target objects inherit the top-level timeout, with the option to override it at any level for customized control.
{% endhint %}

### How timeouts work in nested Configs

<pre class="language-json" data-line-numbers><code class="lang-json">{
  "strategy": { "mode": "loadbalance" },
<strong>  "request_timeout": 2000,
</strong>  "targets": [
    {
      "strategy": { "mode":"fallback" },
<strong>      "request_timeout": 5000,
</strong>      "targets": [
        {
          "virtual_key":"open-ai-1-1"
        },
        {
          "virtual_key": "open-ai-1-2",
<strong>          "request_timeout": 10000
</strong>        }
      ],
      "weight": 1        
    },
    { 
      "virtual_key": "azure-open-ai-1", 
      "weight": 1
    }
  ]
}
</code></pre>

1. We've set a global timeout of **2s** at line #3
2. The first target has a nested fallback strategy, with a top level request timeout of **5s** at line #7
3. The first virtual key (at line #10), the **target-level** timeout of **5s** will be applied
4. For the second virtual key (i.e. `open-ai-1-2`), there is a timeout override, set at **10s**, which will be applied only to this target
5. For the last target (i.e. virtual key `azure-open-ai-1`), the top strategy-level timeout of **2s** will be applied

## Handling Request Timeouts

Portkey issues a standard **408 error** for timed-out requests. You can leverage this by setting up fallback or retry strategies through the **`on_status_codes`** parameter, ensuring robust handling of these scenarios.

### Triggering Fallbacks with Request Timeouts

<pre><code>{
  "strategy": {
<strong>    "mode": "fallback",
</strong><strong>    "on_status_codes": [408]
</strong>  },
  "targets": [
<strong>    { "virtual_key": "open-ai-xxx", "request_timeout": 2000, },
</strong>    { "virtual_key": "azure-open-ai-xxx"}
  ]
}
</code></pre>

Here, fallback from OpenAI to Azure OpenAI will only be triggered if the first request times out after 2 seconds, otherwise the request will fail with a 408 error code.

### Triggering Retries with Request Timeouts

<pre><code>{
<strong>    "request_timeout": 1000,
</strong><strong>    "retry": { "attempts": 3, "on_status_codes": [ 408 ] },
</strong>    "virtual_key": "open-ai-xxx"
}
</code></pre>

Here, retry is triggered upto 3 times whenever the request takes more than 1s to return a response. After 3 unsuccessful retries, it will fail with a 408 code.

[Here's a general guide on how to use Configs in your requests.](configs.md)

### Caveats and Considerations

While the request timeout is a powerful feature to help you gracefully handle unruly models & their latencies, there are a few things to consider:

1. Ensure that you are setting reasonable timeouts - for example, models like `gpt-4` often have sub-10-second response times
2. Ensure that you gracefully handle 408 errors for whenever a request does get timed out - you can inform the user to rerun their query and setup some neat interactions on your app
3. For streaming requests, the timeout will not be triggered if it gets **atleast a chunk** before the specified duration.
