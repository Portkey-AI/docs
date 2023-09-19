# 🔬 Observability

Having insight into your application's behavior is paramount. Portkey's observability features allow you to monitor, debug, and optimize your AI applications with ease. You can track each request, understand its journey, and segment them based on custom tags. This level of detail can help in identifying bottlenecks, optimizing costs, and enhancing the overall user experience.

Here's how to set up observability with Portkey:

```
metadata = {
    "_environment": "production",
    "_prompt": "test",
    "_user": "user",
    "_organisation": "acme",
}

trace_id = "llamaindex_portkey"

portkey_client = Portkey(mode="single")

openai_llm = pk.LLMOptions(
    provider="openai",
    model="gpt-3.5-turbo",
    virtual_key=openai_virtual_key_a,
    metadata=metadata,
    trace_id=trace_id,
)

portkey_client.add_llms(openai_llm)

print("Testing Observability functionality:")
response = portkey_client.chat(messages)
print(response)
```
