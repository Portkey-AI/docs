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

pk_llm = Portkey(
    mode="single",
    trace_id="portkey_llamaindex_test",
    metadata=metadata,
)
pk_llm.add_llms(llm1)

print("Testing Observability functionality:")
response = pk_llm.chat(messages)
print(response)
```
