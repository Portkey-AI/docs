# Load Balancing

Load Balancing feature efficiently distributes network traffic across multiple LLMs. This ensures high availability and optimal performance of your generative AI apps, preventing any single LLM from becoming a performance bottleneck.&#x20;

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
      "weight": 0.75
    },
    {
      "virtual_key": "azure-virtual-key",
      "weight": 0.25
    }
  ]
}
```

#### You can [create](configs.md#creating-configs) and then [use](configs.md#using-configs) the config in your requests.

### How Load Balancing Works

1. **Defining the Loadbalance Targets & their Weights**: You provide a list of `virtual keys` (or `provider` + `api_key` pairs), and assign a **`weight`** value to each target. The weights represent the relative share of requests that should be routed to each target.
2.  **Weight Normalization**: Portkey first sums up all the weights you provided for the targets. It then divides each target's weight by the total sum to calculate the normalized weight for that target. This ensures the weights add up to 1 (or 100%), allowing Portkey to distribute the load proportionally.

    \
    For example, let's say you have three targets with weights **5**, **3**, and **1**. The total sum of weights is **9** (5 + 3 + 1). Portkey will then normalize the weights as follows:

    * **Target 1**: 5 / 9 = 0.55 (55% of the traffic)
    * **Target 2**: 3 / 9 = 0.33 (33% of the traffic)
    * **Target 3**: 1 / 9 = 0.011 (11% of the traffic)
3. **Request Distribution**: When a request comes in, Portkey routes it to a target LLM based on the normalized weight probabilities. This ensures the traffic is distributed across the LLMs according to the specified weights.

{% hint style="info" %}
* Default **`weight`** value is **`1`**
* Minimum **`weight`** value is **`0`**
* If **`weight`** is not set for a target, the default **`weight`** value (i.e. **`1`**) is applied.
* You can set **`"weight":0`** for a specific target to stop routing traffic to it without removing it from your Config
{% endhint %}

### Caveats and Considerations

While the Load Balancing feature offers numerous benefits, there are a few things to consider:

1. Ensure the LLMs in your list are compatible with your use case. Not all LLMs offer the same capabilities or respond in the same format.
2. Be aware of your usage with each LLM. Depending on your weight distribution, your usage with each LLM could vary significantly.
3. Keep in mind that each LLM has its own latency and pricing. Diversifying your traffic could have implications on the cost and response time.
