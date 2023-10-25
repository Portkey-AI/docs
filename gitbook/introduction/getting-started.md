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

### 1. **Client SDKs or Rest APIs (🏎️ Fastest)**

Integrate Portkey's API within client SDKs of OpenAI, Anthropic, etc, or alternatively, invoke it through a direct cURL request. If you want to quickly experiment with Portkey and explore its value and capabilities, this is the method to get started!

{% tabs %}
{% tab title="OpenAI (Python)" %}
<pre class="language-python"><code class="lang-python">import openai

openai.api_base = "https://api.portkey.ai/v1/proxy"

r = openai.Completion.create(
  model="gpt-3.5-turbo-instruct",
  prompt="Once upon a time, Cinderella",
  headers=<a data-footnote-ref href="#user-content-fn-1">{</a>
    "x-portkey-api-key": "PORTKEY_API_KEY",
    "x-portkey-mode": "proxy openai"
  }
)

print(r.choices[0].text)
</code></pre>
{% endtab %}

{% tab title="Anthropic (Python)" %}
```python
from anthropic import Anthropic, HUMAN_PROMPT, AI_PROMPT

anthropic = Anthropic(
    api_key="ANTHROPIC_API_KEY",
    base_url="https://api.portkey.ai/v1/proxy",
    default_headers={
        "x-portkey-api-key": "PORTKEY_API_KEY",
        "x-portkey-mode": "proxy anthropic",
    }
)

r = anthropic.completions.create(
    model="claude-2",
    max_tokens_to_sample=300,
    prompt=f"{HUMAN_PROMPT} how does a court case get to the Supreme Court? {AI_PROMPT}",
)

print(r.completion)
```
{% endtab %}

{% tab title="cURL" %}
Portkey supports Mistral & Llama 2 models through Anyscale endpoints. Here's an example call:

```bash
curl 'https://api.portkey.ai/v1/chatComplete' \
     -H 'x-portkey-api-key: PORTKEY_API_KEY' \
     -H 'Content-Type: application/json' \
     -d '{ 
        "config": { 
            "provider": "anyscale",
            "api_key": "ANYSCALE_API_KEY"
        },
        "params": {
            "messages": [{"role": "user","content":"What are the ten tallest buildings in India?"}],
            "model": "mistralai/Mistral-7B-Instruct-v0.1"
        }
    }'
```

For OpenAI:

```bash
curl 'https://api.portkey.ai/v1/chatComplete' \
     -H 'x-portkey-api-key: PORTKEY_API_KEY' \
     -H 'Content-Type: application/json' \
     -d '{ 
        "config": { 
            "provider": "openai",
            "api_key": "OPENAI_API_KEY"
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

{% tabs %}
{% tab title="Python" %}
```python
# pip install -U portkey-ai

import portkey
from portkey import Config, LLMOptions

# Construct the Portkey Config
portkey.config = Config(
    api_key="PORTKEY_API_KEY",
    mode="single",
    llms=LLMOptions(provider="openai", api_key="YOUR_OPENAI_API_KEY")
)

r = portkey.ChatCompletions.create(
    model="gpt-4", 
    messages=[
        {"role": "user","content": "What is the meaning of life, universe and everything?"}
    ]
)
```
{% endtab %}

{% tab title="Node" %}
```typescript
// npm i portkey-ai

import { Portkey } from "portkey-ai";

const portkey = new Portkey({
    api_key: "PORTKEY_API_KEY",
    mode: "single",
    llms: [{ provider: "openai", virtual_key: "open-ai-xxx" }]
});

async function main() {
    const r = await portkey.chat.completions.create({
        model: 'gpt-4',
        messages: [{ role: 'user', content: 'Say this is a test' }]
    });
};

main();
```
{% endtab %}
{% endtabs %}

{% content-ref url="../sdk/" %}
[sdk](../sdk/)
{% endcontent-ref %}

### 3. Directly within Langhchain or Llamaindex

Portkey offers deep, user-friendly integrations with platforms like Langchain & Llamaindex, allowing you to utilize Portkey’s capabilities within these environments effortlessly.

{% content-ref url="../integrations/llamaindex/" %}
[llamaindex](../integrations/llamaindex/)
{% endcontent-ref %}

{% content-ref url="../integrations/langchain.md" %}
[langchain.md](../integrations/langchain.md)
{% endcontent-ref %}

[^1]: 
