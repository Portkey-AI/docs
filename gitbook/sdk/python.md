---
description: >-
  Portkey SDK is the best way to interact with Portkey and bring your LLMs to
  production.
---

# Python

{% code fullWidth="true" %}
```bash
pip install portkey-ai
```
{% endcode %}

[**Check out the Github repo for more details.**](https://github.com/portkey-ai/portkey-python-sdk)

***

## **4-Step Guide**

### **Step 1️⃣ : Get your Portkey API Key and your Virtual Keys for AI providers**

**Portkey API Key:** Log into [Portkey here](https://app.portkey.ai/), then click on the profile icon on top left and “Copy API Key”.

```python
import os
os.environ["PORTKEY_API_KEY"] = "PORTKEY_API_KEY"
```

**Virtual Keys:** Navigate to the "Virtual Keys" page on [Portkey](https://app.portkey.ai/) and hit the "Add Key" button. Choose your AI provider and assign a unique name to your key. Your virtual key is ready!

### **Step 2️⃣ : Construct your LLM, add Portkey features, provider features, and prompt**

**Portkey Features**: You can find a comprehensive [list of Portkey features here](https://github.com/portkey-ai/portkey-python-sdk#%F0%9F%93%94-list-of-portkey-features). This includes settings for caching, retries, metadata, and more.

**Provider Features**: Portkey is designed to be flexible. All the features you're familiar with from your LLM provider, like `top_p`, `top_k`, and `temperature`, can be used seamlessly. Check out the [complete list of provider features here](https://github.com/Portkey-AI/portkey-python-sdk/blob/af0814ebf4f1961b5dfed438918fe68b26ef5f1e/portkey/api\_resources/utils.py#L137).

**Setting the Prompt Input**: You can set the input in two ways. For models like Claude and GPT3, use `prompt` = `(str)`, and for models like GPT3.5 & GPT4, use `messages` = `[array]`.

Here's how you can combine everything:

```python
from portkey import LLMOptions

# Portkey Config
provider = "openai"
virtual_key = "key_a"
trace_id = "portkey_sdk_test"

# Model Settings
model = "gpt-4"
temperature = 1

# User Prompt
messages = [{"role": "user", "content": "Who are you?"}]

# Construct LLM
llm = LLMOptions(provider=provider, virtual_key=virtual_key, trace_id=trace_id, model=model, temperature=temperature, messages=messages)
```

### **Step 3️⃣ : Construct the Portkey Client**

Portkey client's config takes 3 params: `api_key`, `mode`, `llms`.

* `api_key`: You can set your Portkey API key here or with `os.ennviron` as done above.
* `mode`: There are **3** modes - Single, Fallback, Loadbalance.
  * **Single** - This is the standard mode. Use it if you do not want Fallback OR Loadbalance features.
  * **Fallback** - Set this mode if you want to enable the Fallback feature.
  * **Loadbalance** - Set this mode if you want to enable the Loadbalance feature.
* `llms`: This is an array where we pass our LLMs constructed using the LLMOptions constructor.

```python
import portkey
from portkey import Config

portkey.config = Config(mode="single",llms=[llm])
```

### **Step 4️⃣ : Let's Call the Portkey Client!**

The Portkey client can do `ChatCompletions` and `Completions`.

Since our LLM is GPT4, we will use ChatCompletions:

```python
response = portkey.ChatCompletions.create()

print(response.choices[0].message)
```

You have integrated Portkey's Python SDK in just 4 steps!

***

### **📔 Full List of Portkey Config**

| Feature                | Config Key                 | Value(Type)                                                                     | Required                           |
| ---------------------- | -------------------------- | ------------------------------------------------------------------------------- | ---------------------------------- |
| Provider Name          | `provider`                 | `string`                                                                        | ✅ Required                         |
| Model Name             | `model`                    | `string`                                                                        | ✅ Required                         |
| Virtual Key OR API Key | `virtual_key` or `api_key` | `string`                                                                        | ✅ Required (can be set externally) |
| Cache Type             | `cache_status`             | `simple`, `semantic`                                                            | ❔ Optional                         |
| Force Cache Refresh    | `cache_force_refresh`      | `True`, `False` (Boolean)                                                       | ❔ Optional                         |
| Cache Age              | `cache_age`                | `integer` (in seconds)                                                          | ❔ Optional                         |
| Trace ID               | `trace_id`                 | `string`                                                                        | ❔ Optional                         |
| Retries                | `retry`                    | `integer` \[0,5]                                                                | ❔ Optional                         |
| Metadata               | `metadata`                 | `json object` [More info](https://docs.portkey.ai/key-features/custom-metadata) | ❔ Optional                         |

### **🤝 Supported Providers**

<table><thead><tr><th width="67"></th><th>Provider</th><th>Support Status</th><th>Supported Endpoints</th></tr></thead><tbody><tr><td><a href="https://github.com/Portkey-AI/portkey-python-sdk/blob/main/docs/images/openai.png"><img src="https://github.com/Portkey-AI/portkey-python-sdk/raw/main/docs/images/openai.png" alt="" data-size="line"></a></td><td>OpenAI</td><td>✅ Supported</td><td><code>/completion</code>, <code>/embed</code></td></tr><tr><td><a href="https://github.com/Portkey-AI/portkey-python-sdk/blob/main/docs/images/azure.png"><img src="https://github.com/Portkey-AI/portkey-python-sdk/raw/main/docs/images/azure.png" alt="" data-size="line"></a></td><td>Azure OpenAI</td><td>✅ Supported</td><td><code>/completion</code>, <code>/embed</code></td></tr><tr><td><a href="https://github.com/Portkey-AI/portkey-python-sdk/blob/main/docs/images/anthropic.png"><img src="https://github.com/Portkey-AI/portkey-python-sdk/raw/main/docs/images/anthropic.png" alt="" data-size="line"></a></td><td>Anthropic</td><td>✅ Supported</td><td><code>/complete</code></td></tr><tr><td><a href="https://github.com/Portkey-AI/portkey-python-sdk/blob/main/docs/images/cohere.png"><img src="https://github.com/Portkey-AI/portkey-python-sdk/raw/main/docs/images/cohere.png" alt="" data-size="line"></a></td><td>Cohere</td><td>🚧 Coming Soon</td><td><code>generate</code>, <code>embed</code></td></tr></tbody></table>
