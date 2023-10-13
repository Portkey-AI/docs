---
description: Discover the ease and simplicity of integrating Portkey.
---

# 🚀 Getting Started

## First, obtain **your Portkey API Key:**

Log into [Portkey](https://app.portkey.ai/), then click on the profile icon on top left and “Copy API Key”.

<div align="left">

<figure><img src="../.gitbook/assets/portkey_api.gif" alt="" width="366"><figcaption></figcaption></figure>

</div>

## 3 Ways to Integrate Portkey:

### 1. **Rest API (🏎️ Fastest)**

Integrate Portkey's API within client SDKs of OpenAI, Anthropic, etc, or alternatively, invoke it through a direct cURL request. If you want to quickly experiment with Portkey and explore its value and capabilities, this is the method to get started!

{% tabs %}
{% tab title="OpenAI" %}
<pre><code>import openai

# Set Portkey as the base path
openai.api_base = "https://api.portkey.ai/v1/proxy"

response = openai.Completion.create(
  model="text-davinci-003",
  prompt="Translate the following English text to French: '{}'",
  headers=<a data-footnote-ref href="#user-content-fn-1">{</a>
    "x-portkey-api-key": "&#x3C;PORTKEY_API_KEY>",
    "x-portkey-mode": "proxy openai"
  }
)

print(response.choices[0].text.strip())
</code></pre>
{% endtab %}

{% tab title="Anthropic" %}
```
from anthropic import Anthropic, HUMAN_PROMPT, AI_PROMPT

headers = {
    "x-portkey-api-key": <PORTKEY_API_KEY>,
    "x-portkey-mode": "proxy anthropic",
}

anthropic = Anthropic(
    api_key=<ANTHROPIC_API_KEY>,
    default_headers=headers,
    base_url="https://api.portkey.ai/v1/proxy",
)

completion = anthropic.completions.create(
    model="claude-2",
    max_tokens_to_sample=300,
    prompt=f"{HUMAN_PROMPT} how does a court case get to the Supreme Court? {AI_PROMPT}",
)

print(completion.completion)
```
{% endtab %}

{% tab title="cURL" %}
```
curl --location 'https://api.portkey.ai/v1/proxy' \ # Send your request to Portkey proxy
    --header 'x-portkey-api-key: PORTKEY_API_KEY' \ # Add Portkey API key
    --header 'Content-Type: application/json' \
    --data '{ 
        "config": { 
            provider: "openai",
            apiKey: "OPENAI_API_KEY",
        },
        "params": {
            "messages": [{"role": "user","content":"What are the ten tallest buildings in India?"}],
            "model": "gpt-4"
        }
    }'
```
{% endtab %}
{% endtabs %}

{% content-ref url="../integrations/llm-providers/" %}
[llm-providers](../integrations/llm-providers/)
{% endcontent-ref %}

### 2. Official **SDK (⭐️ Recommended)**

The best way to interact with Portkey and bring your LLMs to production. Use the same params you use for your LLM calls with OpenAI/Anthropic etc, and make them interoperable while adding production features like fallbacks, load balancing, a/b tests, caching, and more.

{% code fullWidth="true" %}
```python
# Install the SDK:
# pip install -U portkey-ai

# Doing a simple GPT-4 call:

import os
import portkey
from portkey import Config, LLMOptions

os.environ["PORTKEY_API_KEY"] = "YOUR_PORTKEY_API_KEY"

# Construct the Portkey Config
llm = LLMOptions(provider="openai", api_key="YOUR_OPENAI_API_KEY")
portkey.config = Config(mode="single", llms=[llm])

# Your Prompt
messages = [{
    "role": "user", 
    "content": "What is the meaning of life, universe and everything?"
}]

# Making a chat completion call
response = portkey.ChatCompletions.create(model="gpt-4", messages=messages)

print(response)
```
{% endcode %}

{% content-ref url="../sdk/" %}
[sdk](../sdk/)
{% endcontent-ref %}

### 3. Directly within Langhchain or Llamaindex

Portkey offers deep, user-friendly integrations with platforms like Langchain & Llamaindex, allowing you to utilize Portkey’s capabilities within these environments effortlessly.

{% content-ref url="../integrations/llamaindex/" %}
[llamaindex](../integrations/llamaindex/)
{% endcontent-ref %}

{% content-ref url="../integrations/langchain-coming-soon.md" %}
[langchain-coming-soon.md](../integrations/langchain-coming-soon.md)
{% endcontent-ref %}

[^1]: 
