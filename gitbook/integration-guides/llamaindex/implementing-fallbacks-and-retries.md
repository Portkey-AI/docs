# 🔁 Implementing Fallbacks and Retries

Fallbacks and retries are essential for building resilient AI applications. With Portkey, implementing these features is straightforward:

* **Fallbacks**: If a primary service or model fails, Portkey will automatically switch to a backup model.
* **Retries**: If a request fails, Portkey can be configured to retry the request multiple times.

Below, we demonstrate how to set up fallbacks and retries using Portkey:

First install,

<pre><code># Installing the Rubeus AI gateway developed by the Portkey team
<strong>$ pip install rubeus
</strong>$ pip install llama_index
</code></pre>

```
# Importing necessary libraries and modules
from llama_index.llms import Portkey, ChatMessage
from rubeus import LLMBase

import os

os.environ["PORTKEY_API_KEY"] = ""
os.environ["OPENAI_API_KEY"] = ""

pk_llm = Portkey(mode="fallback", retry=5)

llm1 = LLMBase(provider="openai", model="gpt-4")
llm2 = LLMBase(provider="openai", model="gpt-3.5-turbo")

pk_llm.add_llms(llm_params=[llm1, llm2])

print("Testing Fallback & Retry functionality:")
response = pk_llm.chat(messages)
print(response)
```

### Next: [⚖️ Implementing Load Balancing](implementing-load-balancing.md)
