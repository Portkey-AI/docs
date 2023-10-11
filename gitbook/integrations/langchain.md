---
description: Portkey adds core production capabilities to any Langchain app.
---

# ➡ Langchain

<figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

[![\\"Open](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/19LFG4az9j-0pUixiVhhtHLpmLSZlz1Ei?usp=sharing)

#### Key Features of the Portkey Integration: <a href="#key-features-of-portkeys-integration-with-llamaindex" id="key-features-of-portkeys-integration-with-llamaindex"></a>

1. **🚪 AI Gateway**:
   * **Automated Fallbacks & Retries**: Ensure your application remains functional even if a primary service fails.
   * **Load Balancing**: Efficiently distribute incoming requests among multiple models.
   * **Semantic Caching**: Reduce costs and latency by intelligently caching results.
2. **🔬 Observability**:
   * **Logging**: Keep track of all requests for monitoring and debugging.
   * **Requests Tracing**: Understand the journey of each request for optimization.
   * **Custom Tags**: Segment and categorize requests for better insights.
3. **📝 Continuous Improvement with User Feedback**:
   * **Feedback Collection**: Seamlessly gather feedback on any served request, be it on a generation or conversation level.
   * **Weighted Feedback**: Obtain nuanced information by attaching weights to user feedback values.
   * **Feedback Metadata**: Incorporate custom metadata with the feedback to provide context, allowing for richer insights and analyses.
4. **🔑 Secure Key Management**:
   * **Virtual Keys**: Portkey transforms original provider keys into virtual keys, ensuring your primary credentials remain untouched.
   * **Multiple Identifiers**: Ability to add multiple keys for the same provider or the same key under different names for easy identification without compromising security.

#### To harness these features, just start with:

{% tabs %}
{% tab title="Python" %}
```python
# Installing Langchain & Portkey SDK
!pip install langchain
!pip install -U portkey-ai

# Importing necessary libraries and modules
from langchain.chat_models import ChatPortkey
from langchain.schema import HumanMessage, SystemMessage
import portkey as pk
import os

# Set the Portkey API key
os.environ["PORTKEY_API_KEY"] = ""

# Set virtual keys for providers
openai_virtual_key_a = ""
openai_virtual_key_b = ""

# Now construct your Portkey client
portkey = ChatPortkey(mode="loadbalance")

# Construct two LLMs using portkey's LLMOptions

llm_a = pk.LLMOptions(
    provider="openai",
    model="gpt-3.5-turbo",
    virtual_key=openai_virtual_key_a,
)

llm_b = pk.LLMOptions(
    provider="openai",
    model="gpt-4",
    virtual_key=openai_virtual_key_b,
)

# Add the defined LLMs to the Portkey client
portkey.add_llms(llm_params=[llm_a,llm_b])

# Creating the prompt
messages = [
    SystemMessage(content="You are a helpful assistant that translates English to French."),
    HumanMessage(content="Translate this sentence from English to French. I love programming."),
]

# We are ready to generate a response by calling Portkey client!
response = portkey(messages)
print(response.content)
```
{% endtab %}
{% endtabs %}

## Portkey Client (Portkey or ChatPortkey)

**`api_key`** and **`mode`** are required values.

* **`api_key`**
  * You can set your Portkey API key using the Portkey constructor or you can also set it as an environment variable.
* **`mode`** There are **3** modes - Single, Fallback, Loadbalance.
  * **Single** - This is the standard mode. Use it if you do not want Fallback OR Loadbalance features.
  * **Fallback** - Set this mode if you want to enable the Fallback feature.
  * **Loadbalance** - Set this mode if you want to enable the Loadbalance feature.

## LLMOptions

| Feature                  | Config Key                     | Value(Type)                                                                                                                                                | Required   |
| ------------------------ | ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- |
| API Key OR Virtual Key   | `api_key` **OR** `virtual_key` | `string`                                                                                                                                                   | ✅ Required |
| Provider                 | `provider`                     | <p><code>openai</code>, <br><code>cohere</code>,<br><code>anthropic, huggingface</code>,<br><code>azure-openai</code></p>                                  | ✅ Required |
| Model                    | `model`                        | <p>The relevant model name from the provider. For example,<br><strong><code>gpt-3.5-turbo</code></strong> OR<br><strong><code>claude-2</code></strong></p> | ❔ Optional |
| Weight (For Loadbalance) | `weight`                       | `integer`                                                                                                                                                  | ❔ Optional |
| Cache Type               | `cache_status`                 | `simple`, `semantic`                                                                                                                                       | ❔ Optional |
| Force Cache Refresh      | `cache_force_refresh`          | `True`, `False`                                                                                                                                            | ❔ Optional |
| Cache Age                | `cache_age`                    | `integer` (in seconds)                                                                                                                                     | ❔ Optional |
| Trace ID                 | `trace_id`                     | `string`                                                                                                                                                   | ❔ Optional |
| Retries                  | `retry`                        | `integer` \[0,5]                                                                                                                                           | ❔ Optional |
| Metadata                 | `metadata`                     | `json object` [More info](https://docs.portkey.ai/key-features/custom-metadata)                                                                            | ❔ Optional |
| All Model Params         | As per the model/provider      | This is params like **`top_p`**, **`temperature`**, etc                                                                                                    | ❔ Optional |

LLMs constructed through LLMOptions can be added to the Portkey client with **`portkey.add_llms(llm_params=[llm_a])`**

### Test all functionalities easily: [![\\"Open](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/19LFG4az9j-0pUixiVhhtHLpmLSZlz1Ei?usp=sharing)

