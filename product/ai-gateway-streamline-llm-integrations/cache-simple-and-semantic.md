# Cache (Simple & Semantic)

Speed up and save money on your LLM requests by storing past responses in the Portkey cache. There are 2 cache modes:

* **Simple:** Matches requests verbatim. Perfect for repeated, identical prompts.
* **Semantic:** Matches responses for requests that are semantically similar. Ideal for denoising requests with extra prepositions, pronouns, etc.

Portkey cache serves requests upto **20x times faster** and **cheaper**.

## **Enable Cache in the Config**

To enable Portkey cache, just add the `cache` params to your [config object](../../api-reference/config-object.md#cache-object-details).

### **Simple Cache**

```sh
"cache": { "mode": "simple" }
```

**How it works:** Simple cache performs an exact match on the input prompts. If the exact same request is received again, Portkey retrieves the response directly from the cache, bypassing the model execution.&#x20;

### **Semantic Cache**

```json
"cache": { "mode": "semantic" }
```

**How it works:** Semantic cache considers the contextual similarity between input requests. It uses cosine similarity to ascertain if the similarity between the input and a cached request exceeds a specific threshold. If the similarity threshold is met, Portkey retrieves the response from the cache, saving model execution time. Check out this [blog](https://portkey.ai/blog/reducing-llm-costs-and-latency-semantic-cache/) for more details on how we do this.

{% hint style="info" %}
Semantic cache is a "superset" of both caches. Setting cache mode to "semantic" will work for when there are simple cache hits as well.
{% endhint %}

{% hint style="warning" %}
To optimise for accurate cache hit rates, Semantic cache only works with requests with less than 8,191 input tokens, and with number of messages (human, assistant, system combined) less than or equal to 4.
{% endhint %}

[Read more how to set cache in Configs](cache-simple-and-semantic.md#how-cache-works-with-configs).

***

## Setting Cache Age

You can set the age (or "ttl") of your cached response with this setting. Cache age is also set in your Config object:

<pre><code>"cache": { 
    "mode": "semantic",
<strong>    "max_age": 60
</strong>}
</code></pre>

In this example, your cache will automatically expire after 60 seconds. Cache age is set in **seconds**.&#x20;

{% hint style="info" %}
* **Minimum** cache age is **60 seconds**
* **Maximum** cache age is **90 days** (i.e. **7776000** seconds)
* **Default** cache age is **7 days** (i.e. **604800** seconds)
{% endhint %}

***

## Force Refresh Cache

Ensure that a new response is fetched and stored in the cache even when there is an existing cached response for your request. Cache force refresh can only be done **at the time of making a request**, and it is **not a part of your Config**.

You can enable cache force refresh with this header:

```bash
"x-portkey-cache-force-refresh": "True"
```

{% tabs %}
{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash">curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: open-ai-xxx" \
<strong>  -H "x-portkey-config: cache-config-xxx" \
</strong><strong>  -H "x-portkey-cache-force-refresh: True" \
</strong>  -d '{
    "messages": [{"role": "user","content": "Hello!"}]
  }'
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="open-ai-xxx",
<strong>    config="pp-cache-xxx" 
</strong>)

response = portkey.with_options(
<strong>    cache_force_refresh = True
</strong>).chat.completions.create(
    messages = [{ "role": 'user', "content": 'Hello!' }],
    model = 'gpt-4'
)
</code></pre>
{% endtab %}

{% tab title="Node" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai';

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
<strong>    config: "pc-cache-xxx",
</strong>    virtualKey: "open-ai-xxx"
})

async function main(){
    const response = await portkey.chat.completions.create({
        messages: [{ role: 'user', content: 'Hello' }],
        model: 'gpt-4',
    }, {
<strong>        cacheForceRefresh: true
</strong>    });
}

main()
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
* Cache force refresh is only activated if a cache config is **also passed** along with your request. (setting `cacheForceRefresh` as `true` without passing the relevant cache config will not have any effect)
* For requests that have previous semantic hits, force refresh is performed on ALL the semantic matches of your request.
{% endhint %}

***

## **Cache in Analytics**

Portkey shows you powerful stats on cache usage on the Analytics page. Just head over to the Cache tab, and you will see:

* Your raw number of cache hits as well as daily cache hit rate&#x20;
* Your average latency for delivering results from cache and how much time it saves you
* How much money the cache saves you

## **Cache in Logs**

On the Logs page, the cache status  is updated on the Status column. You will see `Cache Disabled` when you are not using the cache, and any of `Cache Miss`, `Cache Refreshed`, `Cache Hit`, `Cache Semantic Hit` based on the cache hit status. Read more [here](../observability-modern-monitoring-for-llms/logs.md).

<div align="left">

<figure><img src="../../.gitbook/assets/image (22).png" alt="" width="199"><figcaption></figcaption></figure>

</div>

For each request we also calculate and show the cache response time and how much money you saved with each hit.

***

## How Cache works with Configs

You can set cache at two levels:

* **Top-level** that works across all the targets.
* **Target-level** that works when that specific target is triggered.

{% tabs %}
{% tab title="Setting Top-Level Cache" %}
<pre class="language-json"><code class="lang-json">{
<strong>  "cache": {"mode": "semantic", "max_age": 60},
</strong>  "strategy": {"mode": "fallback"},
  "targets": [
    {"virtual_key": "openai-key-1"},
    {"virtual_key": "openai-key-2"}
  ]
}
</code></pre>
{% endtab %}

{% tab title="Setting Target-Level Cache" %}
<pre class="language-json"><code class="lang-json">{
  "strategy": {"mode": "fallback"},
  "targets": [
    {
      "virtual_key": "openai-key-1",
<strong>      "cache": {"mode": "simple", "max_age": 200}
</strong>    },
    {
      "virtual_key": "openai-key-2",
<strong>      "cache": {"mode": "semantic", "max_age": 100}
</strong>    }
  ]
}
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
You can also set cache at **both levels (top & target).**&#x20;

In this case, the **target-level cache** setting will be **given preference** over the **top-level cache** setting. You should start getting cache hits from the second request onwards for that specific target.
{% endhint %}

{% hint style="warning" %}
If any of your targets have **`override_params`** then cache on that target will not work until that particular combination of params is also stored with the cache. \
\
If there are **no** **`override_params`** for that target, then **cache will be active** on that target even if it hasn't been triggered even once.
{% endhint %}

