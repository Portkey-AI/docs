---
description: Implement fallback from GPT-4 to Llama2-70B in your Llamaindex app
---

# ⚙ Fallback to Llama2-70B in Llamaindex

Portkey's deep integrations with both Llamaindex and Anyscale endpoints make it very easy to fully use Portkey's production functionalities in your Llamaindex app.

{% tabs %}
{% tab title="Llamaindex" %}
First, install Portkey SDK & Export Portkey API Key

```bash
$ pip install -U portkey-ai
$ export PORTKEY_API_KEY=PORTKEY_API_KEY
```

Construct Llama2-70B & GPT-4 LLMs with Porktey SDK

```python
import portkey

llama = portkey.LLMOptions(
    provider="anyscale", 
    model="meta-llama/Llama-2-70b-chat-hf", 
    virtual_key="anyscale-xxx"
)

gpt = portkey.LLMOptions(
    provider="openai",
    model="gpt-4", 
    virtual_key="open-ai-xxx"
)
```

Create the Portkey client, set `mode` as `fallback`, and add the LLMs

```python
from llama_index.llms import Portkey

portkey_client = Portkey(mode="fallback")
portkey_client.add_llms(llm_params=[llama,gpt])
```

Let's test it!

```python
from llama_index.llms import ChatMessage

messages = [
    ChatMessage(role="system", content="You are a helpful assistant"),
    ChatMessage(role="user", content="What can you do?"),
]

response = portkey_client.chat(messages)

print(response
```
{% endtab %}
{% endtabs %}
