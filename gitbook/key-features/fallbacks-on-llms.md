# 🤖 Fallbacks on LLMs

With an array of Language Model APIs available on the market, each with its own strengths and specialties, wouldn't it be great if you could seamlessly switch between them based on their performance or availability? Portkey's Fallback on LLMs feature is designed to do exactly that.

The Fallback on LLMs feature allows you to specify a list of Language Model APIs (LLMs) in a prioritized order. If the primary LLM fails to respond or encounters an error, Portkey will automatically fallback to the next LLM in the list, ensuring your application's robustness and reliability.

{% hint style="danger" %}
**Please note:** As of now, the Fallback on LLMs feature is in beta and available by invite only. If you wish to have this feature enabled for your organization, please reach out to Portkey support.
{% endhint %}

### Enabling Fallback on LLMs

To enable the Fallback on LLMs feature, you need to include the `x-portkey-fallback` header in your requests. The value of this header should be a string representing a comma-separated list of LLMs, ordered by priority.

For example, if you want to prioritize OpenAI, but fallback to Azure's OpenAI if the former fails, your request might look something like this:

```python
headers = {
    "x-portkey-api-key": "<YOUR_PORTKEY_API_KEY>",
    "x-portkey-fallback": "openai,azure_openai"
}

response = requests.post('https://api.portkey.ai/v1/models', headers=headers, data=payload)
```

In this scenario, if the OpenAI model encounters an error or fails to respond, Portkey will automatically retry the request with Azure's OpenAI model.

### Caveats and Considerations

While the Fallback on LLMs feature greatly enhances the reliability and resilience of your application, there are a few things to consider:

1. Ensure the LLMs in your fallback list are compatible with your use case. Not all LLMs offer the same capabilities.
2. Keep an eye on your usage with each LLM. Depending on your fallback list, a single request could result in multiple LLM invocations.
3. Understand that each LLM has its own latency and pricing. Falling back to a different LLM could have implications on the cost and response time.

