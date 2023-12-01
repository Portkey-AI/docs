# Fallbacks

With an array of Language Model APIs available on the market, each with its own strengths and specialties, wouldn't it be great if you could seamlessly switch between them based on their performance or availability? Portkey's Fallback capability is designed to do exactly that.

The Fallback feature allows you to specify a list of Language Model APIs (LLMs) in a prioritized order. If the primary LLM fails to respond or encounters an error, Portkey will automatically fallback to the next LLM in the list, ensuring your application's robustness and reliability.

### Enabling Fallback on LLMs

To enable fallbacks, you can modify the [config object](../../api-reference/config-object.md) to include the `fallback` mode.

Here's a quick example of a config to **fallback** to Anthropic's `claude-v1` if OpenAI's `gpt-3.5-turbo` fails.

```json
{
  "version": "2.0",
  "mode": "fallback",
  "targets": [
    {
      "virtualKey": "openai-virtual-key",
    },
    {
      "virtualKey": "anthropic-virtual-key",
      "override_params": {
          "model": "claude-1"
      }
    }
  ]
}
```

In this scenario, if the OpenAI model encounters an error or fails to respond, Portkey will automatically retry the request with Anthropic.

[Using Configs in your Requests](configs.md#using-configs)

### Caveats and Considerations

While the Fallback on LLMs feature greatly enhances the reliability and resilience of your application, there are a few things to consider:

1. Ensure the LLMs in your fallback list are compatible with your use case. Not all LLMs offer the same capabilities.
2. Keep an eye on your usage with each LLM. Depending on your fallback list, a single request could result in multiple LLM invocations.
3. Understand that each LLM has its own latency and pricing. Falling back to a different LLM could have implications on the cost and response time.
