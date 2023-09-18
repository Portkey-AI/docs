# 🔁 Implementing Fallbacks and Retries

Fallbacks and retries are essential for building resilient AI applications. With Portkey, implementing these features is straightforward:

* **Fallbacks**: If a primary service or model fails, Portkey will automatically switch to a backup model.
* **Retries**: If a request fails, Portkey can be configured to retry the request multiple times.

Below, we demonstrate how to set up fallbacks and retries using Portkey:

```
portkey_client = Portkey(mode="fallback")
messages = [
    ChatMessage(role="system", content="You are a helpful assistant"),
    ChatMessage(role="user", content="What can you do?"),
]

llm1 = pk.LLMOptions(
    provider="openai",
    model="gpt-4",
    retry_settings={"on_status_codes": [429, 500], "attempts": 2},
    virtual_key=openai_virtual_key_a,
)

llm2 = pk.LLMOptions(
    provider="openai",
    model="gpt-3.5-turbo",
    virtual_key=openai_virtual_key_b,
)

portkey_client.add_llms(llm_params=[llm1, llm2])

print("Testing Fallback & Retry functionality:")
response = portkey_client.chat(messages)
print(response)
```

### Next: [⚖️ Implementing Load Balancing](implementing-load-balancing.md)
