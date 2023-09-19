# 🪝 Load Balancing

Portkey's Load Balancing feature is designed to efficiently distribute network traffic across multiple Language Model APIs (LLMs). This ensures high availability and optimal performance of your generative AI apps, preventing any single LLM from becoming a performance bottleneck.

The Load Balancing feature allows you to specify a list of Language Model APIs (LLMs) and a corresponding list of weights. Portkey will then distribute the requests among these LLMs based on the given weights.

{% hint style="danger" %}
**Please note:** As of now, the Load Balancing feature is in beta. Please reach out to Portkey support, if you face any issues.
{% endhint %}

### Enabling Load Balancing

To enable Load Balancing, you can modify the `config` object of your `complete` or `chatComplete` API request to include the `loadbalance` mode.

Here's a quick example to **load balance equally** between OpenAI's `gpt-3.5-turbo` and Anthropic's `claude-v1`

```powershell
# Load balance 50-50 between text-davinci-003 and claude-v1
curl --location 'https://api.portkey.ai/v1/chatComplete' \
--header 'Content-Type: application/json' \
--header 'x-portkey-api-key: <PORTKEY_API_KEY>' \
--data '{
    "config": {
        "mode": "loadbalance",
        "options": [{
            "provider": "openai",
            "apiKey": "<API_KEY>",
            "weight": 0.5,
            "override_params": { "model": "gpt-3.5-turbo" }
        }, {
            "provider": "anthropic",
            "apiKey": "<API_KEY>",
            "weight": 0.5,
            "override_params": { "model": "claude-v1" }
        }]
    },
    "params": {
        "messages": [{"role": "user","content":"What are the top 10 happiest countries in the world?"}],
        "max_tokens": 50,
        "user": "jbu3470"
    }
}'
```

Here's another example to load balance between **OpenAI and an Azure deployment**.

```powershell
# Load balance 70-30 between azure and openai
curl --location 'https://api.portkey.ai/v1/chatComplete' \
--header 'Content-Type: application/json' \
--header 'x-portkey-api-key: <PORTKEY_API_KEY>' \
--data '{
    "config": {
        "mode": "loadbalance",
        "options": [{
            "provider": "azure-openai",
	    "apiKey": "<AZURE_API_KEY>",
	    "resourceName": "<AZURE_RESOURCE_NAME>",
	    "deploymentId": "<AZURE_DEPLOYMENT_NAME>",
	    "apiVersion": "<AZURE_API_VERSION>",
	    "weight": 0.7
        }, {
            "provider": "openai",
            "apiKey": "<OPENAI_API_KEY>",
            "weight": 0.3
        }]
    },
    "params": {
        "messages": [{"role": "user","content":"What are the top 10 happiest countries in the world?"}],
        "max_tokens": 50,
        "user": "jbu3470",
        "model": "gpt-3.5-turbo"
    }
}'
```

### Caveats and Considerations

While the Load Balancing feature offers numerous benefits, there are a few things to consider:

1. Ensure the LLMs in your list are compatible with your use case. Not all LLMs offer the same capabilities or respond in the same format.
2. Be aware of your usage with each LLM. Depending on your weight distribution, your usage with each LLM could vary significantly.
3. Keep in mind that each LLM has its own latency and pricing. Diversifying your traffic could have implications on the cost and response time.
