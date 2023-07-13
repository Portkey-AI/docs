# 🪝 Load Balancing

Portkey's Load Balancing feature is designed to efficiently distribute network traffic across multiple Language Model APIs (LLMs). This ensures high availability and optimal performance of your generative AI apps, preventing any single LLM from becoming a performance bottleneck.

The Load Balancing feature allows you to specify a list of Language Model APIs (LLMs) and a corresponding list of weights. Portkey will then distribute the requests among these LLMs based on the given weights.

{% hint style="danger" %}
**Please note:** As of now, the Load Balancing feature is in beta and available by invite only. If you wish to have this feature enabled for your organization, please reach out to Portkey support.
{% endhint %}

### Enabling Load Balancing

To enable Load Balancing, you need to include two headers in your requests: `x-portkey-balance` and `x-portkey-balance-weights`.

The `x-portkey-balance` header should contain a string representing a comma-separated list of LLMs.

The `x-portkey-balance-weights` header should contain a string representing a comma-separated list of weights corresponding to each LLM. These weights determine the proportion of traffic that will be directed to each LLM.

For example, if you want to distribute requests between OpenAI and Azure's OpenAI with a 70:30 ratio, your request might look something like this:

```python
headers = {
    "x-portkey-api-key": "<YOUR_PORTKEY_API_KEY>",
    "x-portkey-balance": "openai,azure_openai",
    "x-portkey-balance-weights": "0.7,0.3"
}

response = requests.post('https://api.portkey.ai/v1/models', headers=headers, data=payload)
```

In this scenario, approximately 70% of the requests will be directed to OpenAI and 30% to Azure's OpenAI.

### Caveats and Considerations

While the Load Balancing feature offers numerous benefits, there are a few things to consider:

1. Ensure the LLMs in your list are compatible with your use case. Not all LLMs offer the same capabilities or respond in the same format.
2. Be aware of your usage with each LLM. Depending on your weight distribution, your usage with each LLM could vary significantly.
3. Keep in mind that each LLM has its own latency and pricing. Diversifying your traffic could have implications on the cost and response time.
