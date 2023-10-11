---
description: Building Resilient Llamaindex Apps
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# ➡ Llamaindex

[![\\"Open](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jerryjliu/llama\_index/blob/main/docs/examples/llm/portkey.ipynb)

Portkey adds core production capabilities to any Llamaindex app.

<figure><img src="../../.gitbook/assets/Colab Version 2.png" alt=""><figcaption></figcaption></figure>

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

To harness these features, just start with:

```python
# Installing Llamaindex & Portkey SDK
!pip install -U llama_index
!pip install -U portkey-ai

# Importing necessary libraries and modules
from llama_index.llms import Portkey, ChatMessage
import portkey as pk
```

You do not need to install **any** other SDKs or import them in your Llamaindex app.\


**Step 1: Get your Portkey API Key and your Virtual Keys for AI Providers**

Log into [Portkey here](https://app.portkey.ai/), then click on the profile icon on top right and "Copy API Key".&#x20;

```python
import os
os.environ["PORTKEY_API_KEY"] = ""
```

[**Virtual Keys**](https://docs.portkey.ai/key-features/ai-provider-keys)

1. Navigate to the “Virtual Keys” page on [Portkey dashboard](https://app.portkey.ai/) and hit the “Add Key” button located at the top right corner.
2. Choose your AI provider (OpenAI, Anthropic, Cohere, HuggingFace, etc.), assign a unique name to your key, and, if needed, jot down any relevant usage notes. Your virtual key is ready!
3. Now copy and paste the keys below - you can use them anywhere within the Portkey ecosystem and keep your original key secure and untouched.

```python
openai_virtual_key_a = ""
openai_virtual_key_b = ""

anthropic_virtual_key_a = ""
anthropic_virtual_key_b = ""

cohere_virtual_key_a = ""
cohere_virtual_key_b = ""
```

If you don’t want to use Portkey’s Virtual keys, you can also use your AI provider keys directly.

```python
os.environ["OPENAI_API_KEY"] = ""
os.environ["ANTHROPIC_API_KEY"] = ""
```

**Step 2: Configure Portkey Features**

To harness the full potential of Portkey's integration with Llamaindex, you can configure various features as illustrated above. Here's a guide to all Portkey features and the expected values:

| Feature             | Config Key            | Value(Type)                                                                     | Required                           |
| ------------------- | --------------------- | ------------------------------------------------------------------------------- | ---------------------------------- |
| API Key             | `api_key`             | `string`                                                                        | ✅ Required (can be set externally) |
| Mode                | `mode`                | `fallback`, `loadbalance`, `single`                                             | ✅ Required                         |
| Cache Type          | `cache_status`        | `simple`, `semantic`                                                            | ❔ Optional                         |
| Force Cache Refresh | `cache_force_refresh` | `True`, `False`                                                                 | ❔ Optional                         |
| Cache Age           | `cache_age`           | `integer` (in seconds)                                                          | ❔ Optional                         |
| Trace ID            | `trace_id`            | `string`                                                                        | ❔ Optional                         |
| Retries             | `retry`               | `integer` \[0,5]                                                                | ❔ Optional                         |
| Metadata            | `metadata`            | `json object` [More info](https://docs.portkey.ai/key-features/custom-metadata) | ❔ Optional                         |
| Base URL            | `base_url`            | `url`                                                                           | ❔ Optional                         |

* `api_key` and `mode` are required values.
* You can set your Portkey API key using the Portkey constructor or you can also set it as an environment variable.
* There are **3** modes - Single, Fallback, Loadbalance.
  * **Single** - This is the standard mode. Use it if you do not want Fallback OR Loadbalance features.
  * **Fallback** - Set this mode if you want to enable the Fallback feature. Check out the guide here.
  * **Loadbalance** - Set this mode if you want to enable the Loadbalance feature. Check out the guide here.

Here’s an example of how to set up some of these features:

```python
portkey_client = Portkey(
    mode="single",
)

# Since we have defined the Portkey API Key with os.environ, we do not need to set api_key again here
```

**Step 3: Constructing the LLM**

With the Portkey integration, constructing an LLM is simplified. Use the `LLMOptions` function for all providers, with the exact same keys you’re accustomed to in your OpenAI or Anthropic constructors. The only new key is `weight`, essential for the load balancing feature.

```python
openai_llm = pk.LLMOptions(
    provider="openai",
    model="gpt-4",
    virtual_key=openai_virtual_key_a,
)
```

The above code illustrates how to utilize the `LLMOptions` function to set up an LLM with the OpenAI provider and the GPT-4 model. This same function can be used for other providers as well, making the integration process streamlined and consistent across various providers.

**Step 4: Activate the Portkey LLM**

Once you’ve constructed the LLM using the `LLMOptions` function, the next step is to activate it with Portkey. This step is essential to ensure that all the Portkey features are available for your LLM.

```python
pk_llm.add_llms(openai_llm)
```

And, that's it! In just 4 steps, you have infused your Llamaindex app with sophisticated production capabilities.

**Testing the Integration**

Let's ensure that everything is set up correctly. Below, we create a simple chat scenario and pass it through our Portkey-enhanced LLM to see the response.

```python
messages = [
    ChatMessage(role="system", content="You are a helpful assistant"),
    ChatMessage(role="user", content="What can you do?"),
]
print("Testing Portkey Llamaindex integration:")
response = portkey_client.chat(messages)
print(response)
```

#### Test all functionalities easily: [![\\"Open](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jerryjliu/llama\_index/blob/main/docs/examples/llm/portkey.ipynb)

### Next: [🔁 Implementing Fallbacks and Retries](implementing-fallbacks-and-retries.md)
