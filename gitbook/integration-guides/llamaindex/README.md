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

1. **AI Gateway**:
   * **Automated Fallbacks & Retries**: Ensure your application remains functional even if a primary service fails.
   * **Load Balancing**: Efficiently distribute incoming requests among multiple models.
   * **Semantic Caching**: Reduce costs and latency by intelligently caching results.
2. **Observability**:
   * **Logging**: Keep track of all requests for monitoring and debugging.
   * **Requests Tracing**: Understand the journey of each request for optimization.
   * **Custom Tags**: Segment and categorize requests for better insights.

To harness these features, just start with:

```
# Installing the Rubeus AI gateway developed by the Portkey team
$ pip install rubeus
$ pip install llama_index

# Importing necessary libraries and modules
from llama_index.llms import Portkey, ChatMessage
from rubeus import LLMBase
```

You do not need to install **any** other SDKs or import them in your Llamaindex app.\


**Step 1: Get your Portkey API key**

Log into [Portkey here](https://app.portkey.ai/), then click on the profile icon on top right and "Copy API Key". Let's also set OpenAI & Anthropic API keys.

```
import os

os.environ["PORTKEY_API_KEY"] = ""
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

Here's an example of how to set up some of these features:

```
metadata = {
    "_environment": "production",
    "_prompt": "test",
    "_user": "user",
    "_organisation": "acme",
}

pk_llm = Portkey(
    mode="single",
    cache_status="semantic",
    cache_force_refresh=True,
    cache_age=1000,
    trace_id="portkey_llamaindex",
    retry=5,
    metadata=metadata,
)

# Since we have defined the Portkey API Key with os.environ, we do not need to set api_key again here
```

**Step 3: Constructing the LLM**

With the Portkey integration, constructing an LLM is simplified. Use the `LLMBase` function for all providers, with the exact same keys you're accustomed to in your OpenAI or Anthropic constructors. The only new key is `weight`, essential for the load balancing feature.

```
openai_llm = LLMBase(provider="openai", model="gpt-4")
```

The above code illustrates how to utilize the `LLMBase` function to set up an LLM with the OpenAI provider and the GPT-4 model. This same function can be used for other providers as well, making the integration process streamlined and consistent across various providers.

**Step 4: Activate the Portkey LLM**

Once you've constructed the LLM using the `LLMBase` function, the next step is to activate it with Portkey. This step is essential to ensure that all the Portkey features are available for your LLM.

```
pk_llm.add_llms(openai_llm)
```

And, that's it! In just 4 steps, you have infused your Llamaindex app with sophisticated production capabilities.

**Testing the Integration**

Let's ensure that everything is set up correctly. Below, we create a simple chat scenario and pass it through our Portkey-enhanced LLM to see the response.

```
messages = [
    ChatMessage(role="system", content="You are a helpful assistant"),
    ChatMessage(role="user", content="What can you do?"),
]
print("Testing Portkey Llamaindex integration:")
response = pk_llm.chat(messages)
print(response)
```

#### Test all functionalities easily: [![\\"Open](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jerryjliu/llama\_index/blob/main/docs/examples/llm/portkey.ipynb)

### Next: [🔁 Implementing Fallbacks and Retries](implementing-fallbacks-and-retries.md)
