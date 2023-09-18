# 🧠 Implementing Semantic Caching

Semantic caching is a smart caching mechanism that understands the context of a request. Instead of caching based solely on exact input matches, semantic caching identifies similar requests and serves cached results, reducing redundant requests and improving response times as well as saving money.

Let's see how to implement semantic caching with Portkey:

```
import time

portkey_client = Portkey(mode="single")

openai_llm = pk.LLMOptions(
    provider="openai",
    model="gpt-3.5-turbo",
    virtual_key=openai_virtual_key_a,
    cache_status="semantic",
)

portkey_client.add_llms(openai_llm)

current_messages = [
    ChatMessage(role="system", content="You are a helpful assistant"),
    ChatMessage(role="user", content="What are the ingredients of a pizza?"),
]

print("Testing Portkey Semantic Cache:")

start = time.time()
response = portkey_client.chat(current_messages)
end = time.time() - start

print(response)
print(f"{'-'*50}\nServed in {end} seconds.\n{'-'*50}")

new_messages = [
    ChatMessage(role="system", content="You are a helpful assistant"),
    ChatMessage(role="user", content="Ingredients of pizza"),
]

print("Testing Portkey Semantic Cache:")

start = time.time()
response = portkey_client.chat(new_messages)
end = time.time() - start

print(response)
print(f"{'-'*50}\nServed in {end} seconds.\n{'-'*50}")
```

Portkey's cache supports two more cache-critical functions - Force Refresh and Age.

`cache_force_refresh`: Force-send a request to your provider instead of serving it from a cache. `cache_age`: Decide the interval at which the cache store for this particular string should get automatically refreshed. The cache age is set in seconds.

Here's how you can use it:

```
# Setting the cache status as `semantic` and cache_age as 60s.
openai_llm = pk.LLMOptions(
    provider="openai",
    model="gpt-3.5-turbo",
    virtual_key=openai_virtual_key_a,
    cache_force_refresh=True,
    cache_age=60,
)
```

### Next: [Observability](observability.md)
