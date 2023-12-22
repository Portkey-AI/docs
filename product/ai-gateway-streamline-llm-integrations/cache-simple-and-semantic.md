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
