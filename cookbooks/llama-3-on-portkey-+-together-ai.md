---
description: Try out the new Llama 3 model directly using the OpenAI SDK
---

# Llama 3 on Portkey + Together AI

<div align="left">

<figure><img src="https://cdn.mos.cms.futurecdn.net/RNrVwVfRiyoKkrr8djHvf9-1200-80.jpg" alt="" width="563"><figcaption></figcaption></figure>

</div>

### You will need Portkey and Together AI API keys to get started

<table data-header-hidden><thead><tr><th width="255"></th><th></th></tr></thead><tbody><tr><td>Grab <a href="https://app.portkey.ai/">Portkey API Key</a></td><td>Grab <a href="https://api.together.xyz/settings/api-keys">Together AI API Key</a></td></tr></tbody></table>

```sh
pip install -qU portkey-ai openai
```

## With OpenAI Client

<pre class="language-py"><code class="lang-py">from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

openai = OpenAI(
<strong>    api_key= 'TOGETHER_API_KEY', ## Grab from https://api.together.xyz/
</strong><strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        provider="together-ai",
</strong><strong>        api_key= 'PORTKEY_API_KEY' ## Grab from https://app.portkey.ai/
</strong><strong>    )
</strong>)

response = openai.chat.completions.create(
<strong>    model="meta-llama/Llama-3-8b-chat-hf",
</strong>    messages=[{"role": "user", "content": "What's a fractal?"}],
    max_tokens=500
)

print(response.choices[0].message.content)
</code></pre>

## With Portkey Client

You can safely store your Together API key in [Portkey](https://app.portkey.ai/) and access models using Portkey's Virtual Key

<pre class="language-py"><code class="lang-py">from portkey_ai import Portkey

portkey = Portkey(
    api_key = 'PORTKEY_API_KEY',   ## Grab from https://app.portkey.ai/
<strong>    virtual_key= "together-virtual-key"   ## Grab from https://api.together.xyz/ and add to Portkey Virtual Keys
</strong>)

response = portkey.chat.completions.create(
<strong>    model= 'meta-llama/Llama-3-8b-chat-hf',
</strong>    messages= [{ "role": 'user', "content": 'Who are you?'}],
    max_tokens=500
)

print(response.choices[0].message.content)
</code></pre>

## Monitoring your Requests

Using Portkey you can monitor your Llama 3 requests and track tokens, cost, latency, and more.

![](https://portkey.ai/blog/content/images/2024/04/logs.gif)
