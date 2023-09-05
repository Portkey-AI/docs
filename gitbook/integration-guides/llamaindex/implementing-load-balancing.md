# ⚖ Implementing Load Balancing

Load balancing ensures that incoming requests are efficiently distributed among multiple models. This not only enhances the performance but also provides redundancy in case one model fails.

With Portkey, implementing load balancing is simple. You need to:

* Define the `weight` parameter for each LLM. This weight determines how requests are distributed among the LLMs.
* Ensure that the sum of weights for all LLMs equals 1.

Here's an example of setting up load balancing with Portkey:

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

pk_llm = Portkey(mode="loadbalance")

llm1 = LLMBase(provider="openai", model="gpt-4", weight=0.2)
llm2 = LLMBase(provider="openai", model="gpt-3.5-turbo", weight=0.8)

pk_llm.add_llms(llm_params=[llm1, llm2])

print("Testing Loadbalance functionality:")
response = pk_llm.chat(messages)
print(response)
```

### Next: [🧠 Implementing Semantic Caching](implementing-semantic-caching.md)
