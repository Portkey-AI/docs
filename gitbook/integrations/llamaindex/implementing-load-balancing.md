# ⚖ Implementing Load Balancing

Load balancing ensures that incoming requests are efficiently distributed among multiple models. This not only enhances the performance but also provides redundancy in case one model fails.

With Portkey, implementing load balancing is simple. You need to:

* Define the `weight` parameter for each LLM. This weight determines how requests are distributed among the LLMs.
* Ensure that the sum of weights for all LLMs equals 1.

Here's an example of setting up load balancing with Portkey:

```
portkey_client = Portkey(mode="ab_test")

messages = [
    ChatMessage(role="system", content="You are a helpful assistant"),
    ChatMessage(role="user", content="What can you do?"),
]

llm1 = pk.LLMOptions(
    provider="openai",
    model="gpt-4",
    virtual_key=openai_virtual_key_a,
    weight=0.2,
)

llm2 = pk.LLMOptions(
    provider="openai",
    model="gpt-3.5-turbo",
    virtual_key=openai_virtual_key_a,
    weight=0.8,
)

portkey_client.add_llms(llm_params=[llm1, llm2])

print("Testing Loadbalance functionality:")
response = portkey_client.chat(messages)
print(response)
```

### Next: [🧠 Implementing Semantic Caching](implementing-semantic-caching.md)
