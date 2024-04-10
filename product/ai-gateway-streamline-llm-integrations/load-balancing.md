# Load Balancing

Portkey's Load Balancing feature is designed to efficiently distribute network traffic across multiple Language Model APIs (LLMs). This ensures high availability and optimal performance of your generative AI apps, preventing any single LLM from becoming a performance bottleneck.

The Load Balancing feature allows you to specify a list of Language Model APIs (LLMs) and a corresponding list of weights. Portkey will then distribute the requests among these LLMs based on the given weights.

### Enable Load Balancing

To enable Load Balancing, you can modify the `config` object to include a `strategy` with `loadbalance` mode.

Here's a quick example to **load balance 75-25** between an OpenAI and an Azure OpenAI account

```json
{
  "strategy": {
      "mode": "loadbalance",
  },
  "targets": [
    {
      "virtual_key": "openai-virtual-key",
      "weight": 0.7
    },
    {
      "virtual_key": "azure-virtual-key",
      "weight": 0.3
    }
  ]
}
```

You can [create](configs.md#creating-configs) and then [use](configs.md#using-configs) the config in your requests.

{% hint style="info" %}
* `weight` is a necessary param for each target of a loadbalancer config
* `weight` can have any value >=0
* you can set `weight=0` for a specific target to stop routing traffic to it without removing it from your Config
{% endhint %}

### Caveats and Considerations

While the Load Balancing feature offers numerous benefits, there are a few things to consider:

1. Ensure the LLMs in your list are compatible with your use case. Not all LLMs offer the same capabilities or respond in the same format.
2. Be aware of your usage with each LLM. Depending on your weight distribution, your usage with each LLM could vary significantly.
3. Keep in mind that each LLM has its own latency and pricing. Diversifying your traffic could have implications on the cost and response time.
