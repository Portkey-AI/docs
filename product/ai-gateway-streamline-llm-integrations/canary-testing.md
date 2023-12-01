# Canary Testing

You can use Portkey's AI gateway to also canary test new models or prompts in different environments. This uses the same techniques as [load balancing](load-balancing.md) but to achieve a different outcome.

### Example: Test Llama2 on 5% of the traffic

Let's take an example where we want to introduce llama2 in our systems (through Anyscale) but we're not sure of the impact. We can create a config specifically for this use case to test llama2 in production.

The config object would look like this

```json
{
  "version": "2.0",
  "mode": "loadbalance",
  "targets": [
    {
      "virtualKey": "openai-virtual-key",
      "weight": 0.95
    },
    {
      "virtualKey": "anyscale-virtual-key",
      "weight": 0.05,
      "override_params": {
          "model": "meta-llama/Llama-2-70b-chat-hf"
    }
  ]
}
```

Here we are telling the gateway to send 5% of the traffic to anyscale's hosted llama2-70b model. Portkey handles all the request transforms to make sure you don't have to change your code.

You can now [use this config like this](configs.md#using-configs) in your requests.

Once data starts flowing in, we can use Portkey's [analytics dashboards](../observability-modern-monitoring-for-llms/analytics.md) to see the impact of the new model on cost, latency, errors and feedback.
