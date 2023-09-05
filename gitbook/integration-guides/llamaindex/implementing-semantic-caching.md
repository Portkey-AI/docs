# 🧠 Implementing Semantic Caching

Semantic caching is a smart caching mechanism that understands the context of a request. Instead of caching based solely on exact input matches, semantic caching identifies similar requests and serves cached results, reducing redundant requests and improving response times as well as saving money.

Let's see how to implement semantic caching with Portkey:

First install,

<pre><code># Installing the Rubeus AI gateway developed by the Portkey team
<strong>$ pip install rubeus
</strong>$ pip install llama_index
</code></pre>

<pre><code># Importing necessary libraries and modules
from llama_index.llms import Portkey, ChatMessage
from rubeus import LLMBase

import os

os.environ["PORTKEY_API_KEY"] = ""
os.environ["OPENAI_API_KEY"] = ""
<strong>
</strong><strong>import time
</strong>
pk_llm = Portkey(mode="single", cache_status="semantic")
pk_llm.add_llms(llm1)

current_messages = [
    ChatMessage(role="system", content="You are a helpful assistant"),
    ChatMessage(role="user", content="What are the ingredients of a pizza?"),
]

print("Testing Portkey Semantic Cache:")

start = time.time()
response = pk_llm.chat(current_messages)
end = time.time() - start

print(response)
print("\n--------------------------------------\n")
print(f"Served in {end} seconds.")
print("\n--------------------------------------\n")

new_messages = [
    ChatMessage(role="system", content="You are a helpful assistant"),
    ChatMessage(role="user", content="Ingredients of pizza"),
]


print("Testing Portkey Semantic Cache:")

start = time.time()
response = pk_llm.chat(new_messages)
end = time.time() - start

print(response)
print("\n--------------------------------------\n")
print(f"Served in {end} seconds.")
print("\n--------------------------------------\n")
</code></pre>

Portkey's cache supports two more cache-critical functions - Force Refresh and Age.

`cache_force_refresh`: Force-send a request to your provider instead of serving it from a cache. `cache_age`: Decide the interval at which the cache store for this particular string should get automatically refreshed. The cache age is set in seconds.

Here's how you can use it:

```
pk_llm = Portkey(
    mode="single", cache_status="semantic", cache_age=1000, cache_force_refresh=True
)
```

### Next: [Observability](observability.md)
