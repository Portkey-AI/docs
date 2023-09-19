# 🤖 Fallbacks on LLMs

With an array of Language Model APIs available on the market, each with its own strengths and specialties, wouldn't it be great if you could seamlessly switch between them based on their performance or availability? Portkey's Fallback capability is designed to do exactly that.

The Fallback feature allows you to specify a list of Language Model APIs (LLMs) in a prioritized order. If the primary LLM fails to respond or encounters an error, Portkey will automatically fallback to the next LLM in the list, ensuring your application's robustness and reliability.

{% hint style="danger" %}
**Please note:** As of now, the Fallback on LLMs feature is in beta, please reach out to [Portkey support](mailto:support@portkey.ai) if you face issues or have questions.
{% endhint %}

### Enabling Fallback on LLMs

To enable Load Balancing, you can modify the `config` object of your `complete` or `chatComplete` API request to include the `fallback` mode.

Here's a quick example to **fallback** to Anthropic's `claude-v1` if OpenAI's `gpt-3.5-turbo` fails.

```powershell
# Fallback to claude-v1 if gpt-3.5-turbo fails
curl --location 'https://api.portkey.ai/v1/chatComplete' \
--header 'Content-Type: application/json' \
--header 'x-portkey-api-key: <PORTKEY_API_KEY>' \
--data '{
    "config": {
        "mode": "fallback",
        "options": [{
            "provider": "openai",
            "apiKey": "<API_KEY>",
            "override_params": { "model": "gpt-3.5-turbo" }
        }, {
            "provider": "anthropic",
            "apiKey": "<API_KEY>",
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

In this scenario, if the OpenAI model encounters an error or fails to respond, Portkey will automatically retry the request with Anthropic.

### Caveats and Considerations

While the Fallback on LLMs feature greatly enhances the reliability and resilience of your application, there are a few things to consider:

1. Ensure the LLMs in your fallback list are compatible with your use case. Not all LLMs offer the same capabilities.
2. Keep an eye on your usage with each LLM. Depending on your fallback list, a single request could result in multiple LLM invocations.
3. Understand that each LLM has its own latency and pricing. Falling back to a different LLM could have implications on the cost and response time.

