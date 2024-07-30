# LlamaIndex (Python)

The **Portkey x LlamaIndex** integration brings advanced **AI gateway** capabilities, full-stack **observability**, and **prompt management** to apps built on LlamaIndex.

In a nutshell, Portkey extends the familiar OpenAI schema to make Llamaindex work with **200+ LLMs** without the need for importing different classes for each provider or having to configure your code separately. Portkey makes your Llamaindex apps _reliable_, _fast_, and _cost-efficient_.

## Getting Started

### 1. Install the Portkey SDK

```bash
pip install -U portkey-ai
```

### 2. Import the necessary classes and functions

Import the **`OpenAI`** class in Llamaindex as you normally would, along with Portkey's helper functions **`createHeaders`** and **`PORTKEY_GATEWAY_URL`**

<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong></code></pre>

### 3. Configure model details

Configure your model details using Portkey's [**Config object schema**](../../api-reference/config-object.md). This is where you can define the provider and model name, model parameters, set up fallbacks, retries, and more.

<pre class="language-python"><code class="lang-python">config = {
<strong>    "provider":"openai",
</strong><strong>    "api_key":"YOUR_OPENAI_API_KEY",
</strong>    "override_params": {
<strong>        "model":"gpt-4o",
</strong>        "max_tokens":64
    }
}
</code></pre>

### 4. Pass Config details to OpenAI client with necessary headers

<pre class="language-python"><code class="lang-python">portkey = OpenAI(
<strong>    api_base=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        api_key="YOUR_PORTKEY_API_KEY",
</strong><strong>        config=config
</strong><strong>    )
</strong>)
</code></pre>

## Example: OpenAI

Here are basic integrations examples on using the `complete` and `chat` methods with `streaming` on & off.

{% tabs %}
{% tab title="Chat + Streaming" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
<strong>config = {
</strong><strong>    "provider":"openai",
</strong><strong>    "api_key":"YOUR_OPENAI_API_KEY",
</strong><strong>    "override_params": {
</strong><strong>        "model":"gpt-4o",
</strong><strong>        "max_tokens":64
</strong><strong>    }
</strong><strong>}
</strong>
#### You can also reference a saved Config ####
#### config = "pc-anthropic-xx"

<strong>portkey = OpenAI(
</strong><strong>    api_base=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        api_key="YOUR_PORTKEY_API_KEY",
</strong><strong>        config=config
</strong><strong>    )
</strong><strong>)
</strong>
messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]

resp = portkey.chat(messages)
print(resp)

##### Streaming Mode #####

resp = portkey.stream_chat(messages)

for r in resp:
    print(r.delta, end="")
</code></pre>

> assistant: Arrr, matey! They call me Captain Barnacle Bill, the most colorful pirate to ever sail the seven seas! With a parrot on me shoulder and a treasure map in me hand, I'm always ready for adventure! What be yer name, landlubber?
{% endtab %}

{% tab title="Complete + Streaming" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
<strong>config = {
</strong><strong>    "provider":"openai",
</strong><strong>    "api_key":"YOUR_OPENAI_API_KEY",
</strong><strong>    "override_params": {
</strong><strong>        "model":"gpt-4o",
</strong><strong>        "max_tokens":64
</strong><strong>    }
</strong><strong>}
</strong>
#### You can also reference a saved Config ####
#### config = "pc-anthropic-xx"

<strong>portkey = OpenAI(
</strong><strong>    api_base=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        api_key="YOUR_PORTKEY_API_KEY",
</strong><strong>        config=config
</strong><strong>    )
</strong><strong>)
</strong>
resp=portkey.complete("Paul Graham is ")
print(resp)

##### Streaming Mode #####

resp=portkey.stream_complete("Paul Graham is ")
for r in resp:
    print(r.delta, end="")
</code></pre>

> a computer scientist, entrepreneur, and venture capitalist. He is best known for co-founding the startup accelerator Y Combinator and for his work on programming languages and web development. Graham is also a prolific writer and has published essays on a wide range of topics, including startups, technology, and education.
{% endtab %}

{% tab title="Async" %}
<pre class="language-python"><code class="lang-python">import asyncio
from llama_index.llms.openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
<strong>config = {
</strong><strong>    "provider":"openai",
</strong><strong>    "api_key":"YOUR_OPENAI_API_KEY",
</strong><strong>    "override_params": {
</strong><strong>        "model":"gpt-4o",
</strong><strong>        "max_tokens":64
</strong><strong>    }
</strong><strong>}
</strong>
#### You can also reference a saved Config ####
#### config = "pc-anthropic-xx"

async def main():
<strong>    portkey = OpenAI(
</strong><strong>        api_base=PORTKEY_GATEWAY_URL,
</strong><strong>        default_headers=createHeaders(
</strong><strong>            api_key="PORTKEY_API_KEY",
</strong><strong>            config=config
</strong><strong>        )
</strong><strong>    )
</strong>    
<strong>    resp = await portkey.acomplete("Paul Graham is ")
</strong>    print(resp)
    
    ##### Streaming Mode #####
    
<strong>    resp = await portkey.astream_complete("Paul Graham is ")
</strong>    async for delta in resp:
        print(delta.delta, end="")

asyncio.run(main())
</code></pre>
{% endtab %}
{% endtabs %}

***

## Enabling Portkey Features

By routing your LlamaIndex requests through Portkey, you get access to the following production-grade features:

1. [**Interoperability**](llama-index-python.md#id-1.-interoperability-calling-anthropic-gemini-mistral-and-more): Call various LLMs like Anthropic, Gemini, Mistral, Azure OpenAI, Google Vertex AI, and AWS Bedrock with minimal code changes.
2. [**Caching**](llama-index-python.md#id-2.-caching): Speed up your requests and save money on LLM calls by storing past responses in the Portkey cache. Choose between Simple and Semantic cache modes.
3. [**Reliability**](llama-index-python.md#id-3.-reliability): Set up fallbacks between different LLMs or providers, load balance your requests across multiple instances or API keys, set automatic retries, and request timeouts.
4. [**Observability**](llama-index-python.md#id-4.-observability): Portkey automatically logs all the key details about your requests, including cost, tokens used, response time, request and response bodies, and more. Send custom metadata and trace IDs for better analytics and debugging.
5. [**Prompt Management**](llama-index-python.md#id-5.-prompt-management): Use Portkey as a centralized hub to store, version, and experiment with prompts across multiple LLMs, and seamlessly retrieve them in your LlamaIndex app for easy integration.
6. [**Continuous Improvement**](llama-index-python.md#id-6.-continuous-improvement): Improve your LlamaIndex app by capturing qualitative & quantitative user feedback on your requests.
7. [**Security & Compliance**](llama-index-python.md#id-7.-security-and-compliance): Set budget limits on provider API keys and implement fine-grained user roles and permissions for both the app and the Portkey APIs.

Much of these features are driven by **Portkey's Config architecture**. On the Portkey app, we make it easy to help you _create_, _manage_, and _version_ your Configs so that you can reference them easily in Llamaindex.

## Saving Configs in the Portkey App

Head over to the Configs tab in Portkey app where you can save various provider Configs along with the reliability and caching features. Each Config has an associated slug that you can reference in your Llamaindex code.

<figure><img src="../../.gitbook/assets/CleanShot 2024-06-01 at 00.18.08@2x.png" alt=""><figcaption></figcaption></figure>

## Overriding a Saved Config

If you want to use a saved Config from the Portkey app in your LlamaIndex code but need to modify certain parts of it before making a request, you can easily achieve this using Portkey's Configs API. This approach allows you to leverage the convenience of saved Configs while still having the flexibility to adapt them to your specific needs.

#### Here's an example of how you can fetch a saved Config using the Configs API and override the `model` parameter:

{% tabs %}
{% tab title="Overriding Model in a Saved Config" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
import requests
import os

<strong>def create_config(config_slug,model):
</strong><strong>    url = f'https://api.portkey.ai/v1/configs/{config_slug}'
</strong><strong>    headers = {
</strong><strong>        'x-portkey-api-key': os.environ.get("PORTKEY_API_KEY"),
</strong><strong>        'content-type': 'application/json'
</strong><strong>    }
</strong><strong>    response = requests.get(url, headers=headers).json()
</strong><strong>    config = json.loads(response['config'])
</strong><strong>    config['override_params']['model']=model
</strong><strong>    return config
</strong>
<strong>config=create_config("pc-llamaindex-xx","gpt-4-turbo")
</strong>
portkey = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
<strong>    default_headers=createHeaders(
</strong><strong>        api_key=os.environ.get("PORTKEY_API_KEY"),
</strong><strong>        config=config
</strong><strong>    )
</strong>)

messages = [ChatMessage(role="user", content="1729")]

resp = portkey.chat(messages)
print(resp)

</code></pre>
{% endtab %}
{% endtabs %}

In this example:

1. We define a helper function `get_customized_config` that takes a `config_slug` and a `model` as parameters.
2. Inside the function, we make a GET request to the Portkey Configs API endpoint to fetch the saved Config using the provided `config_slug`.
3. We extract the `config` object from the API response.
4. We update the `model` parameter in the `override_params` section of the Config with the provided `custom_model`.
5. Finally, we return the customized Config.

We can then use this customized Config when initializing the OpenAI client from LlamaIndex, ensuring that our specific `model` override is applied to the saved Config.

For more details on working with Configs in Portkey, refer to the [**Config documentation**.](../../product/ai-gateway-streamline-llm-integrations/configs.md)

***

## 1. Interoperability - Calling Anthropic, Gemini, Mistral, and more

Now that we have the OpenAI code up and running, let's see how you can use Portkey to send the request across multiple LLMs - we'll show **Anthropic**, **Gemini**, and **Mistral**. For the full list of providers & LLMs supported, check out [**this doc**](../../guides/integrations/).

Switching providers just requires **changing 3 lines of code:**

1. Change the `provider name`
2. Change the `API key`, and
3. Change the `model name`

{% tabs %}
{% tab title="Anthropic" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

config = {
<strong>    "provider":"anthropic",
</strong><strong>    "api_key":"YOUR_ANTHROPIC_API_KEY",
</strong>    "override_params": {
<strong>        "model":"claude-3-opus-20240229",
</strong>        "max_tokens":64
    }
}

#### You can also reference a saved Config ####
#### config = "pc-anthropic-xx"

portkey = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="YOUR_PORTKEY_API_KEY",
        config=config
    )
)

messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]

resp = portkey.chat(messages)
print(resp)
</code></pre>
{% endtab %}

{% tab title="Gemini" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

config = {
<strong>    "provider":"google",
</strong><strong>    "api_key":"YOUR_GOOGLE_GEMINI_API_KEY",
</strong>    "override_params": {
<strong>        "model":"gemini-1.5-flash-latest",
</strong>        "max_tokens":64
    }
}

#### You can also reference a saved Config instead ####
#### config = "pc-gemini-xx"

portkey = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="YOUR_PORTKEY_API_KEY",
        config=config
    )
)

messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]

resp = portkey.chat(messages)
print(resp)
</code></pre>
{% endtab %}

{% tab title="Mistral" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

config = {
<strong>    "provider":"mistral-ai",
</strong><strong>    "api_key":"YOUR_MISTRAL_AI_API_KEY",
</strong>    "override_params": {
<strong>        "model":"codestral-latest",
</strong>        "max_tokens":64
    }
}

#### You can also reference a saved Config instead ####
#### config = "pc-mistral-xx"

portkey = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="YOUR_PORTKEY_API_KEY",
        config=config
    )
)

messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]

resp = portkey.chat(messages)
print(resp)
</code></pre>
{% endtab %}
{% endtabs %}

### Calling Azure, Google Vertex, AWS Bedrock

{% hint style="info" %}
We recommend saving your cloud details to [**Portkey vault**](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) and getting a corresponding Virtual Key.&#x20;

[**Explore the Virtual Key documentation here**](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/)**.**
{% endhint %}

{% tabs %}
{% tab title="Azure OpenAI" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

<strong>config = {
</strong><strong>    "virtual_key":"AZURE_OPENAI_PORTKEY_VIRTUAL_KEY"
</strong><strong>}
</strong>
#### You can also reference a saved Config instead ####
#### config = "pc-azure-xx"

portkey = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="YOUR_PORTKEY_API_KEY",
        config=config
    )
)

messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]

resp = portkey.chat(messages)
print(resp)
</code></pre>
{% endtab %}

{% tab title="AWS Bedrock" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

<strong>config = {
</strong><strong>    "virtual_key":"AWS_BEDROCK_PORTKEY_VIRTUAL_KEY"
</strong><strong>}
</strong>
#### You can also reference a saved Config instead ####
#### config = "pc-bedrock-xx"

portkey = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="YOUR_PORTKEY_API_KEY",
        config=config
    )
)

messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]

resp = portkey.chat(messages)
print(resp)
</code></pre>
{% endtab %}

{% tab title="Google Vertex AI" %}
{% hint style="info" %}
Vertex AI uses OAuth2 to authenticate its requests, so you need to send the **access token** additionally along with the request - you can do this while by sending it as the `api_key` in the OpenAI client.\
\
Run `gcloud auth print-access-token` in your terminal to get your Vertex AI access token.
{% endhint %}

<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

<strong>config = {
</strong><strong>    "virtual_key":"VERTEX_AI_PORTKEY_VIRTUAL_KEY"
</strong><strong>}
</strong>
#### You can also reference a saved Config instead ####
#### config = "pc-vertex-xx"

portkey = OpenAI(
<strong>    api_key="YOUR_VERTEX_AI_ACCESS_TOKEN", # Get by running gcloud auth print-access-token in terminal
</strong>    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="YOUR_PORTKEY_API_KEY",
        config=config
    )
)

messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]

resp = portkey.chat(messages)
print(resp)
</code></pre>
{% endtab %}
{% endtabs %}

### Calling Local or Privately Hosted Models like Ollama

Check out [**Portkey docs for Ollama**](ollama.md) and [**other privately hosted models**](byollm.md).

{% tabs %}
{% tab title="Ollama" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

<strong>config = {
</strong><strong>    "provider":"ollama",
</strong><strong>    "custom_host":"https://7cc4-3-235-157-146.ngrok-free.app", # Your Ollama ngrok URL
</strong><strong>    "override_params": {
</strong><strong>        "model":"llama3"
</strong><strong>    }    
</strong><strong>}
</strong>
#### You can also reference a saved Config instead ####
#### config = "pc-azure-xx"

portkey = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="YOUR_PORTKEY_API_KEY",
        config=config
    )
)

messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]

resp = portkey.chat(messages)
print(resp)
</code></pre>
{% endtab %}
{% endtabs %}

[**Explore full list of the providers supported on Portkey here**](../../guides/integrations/).

***

## 2. Caching

You can speed up your requests and save money on your LLM requests by storing past responses in the Portkey cache. There are 2 cache modes:

* **Simple:** Matches requests verbatim. Perfect for repeated, identical prompts. Works on **all models** including image generation models.
* **Semantic:** Matches responses for requests that are semantically similar. Ideal for denoising requests with extra prepositions, pronouns, etc.

To enable Portkey cache, just add the **`cache`** params to your [config object](https://portkey.ai/docs/api-reference/config-object#cache-object-details).

{% tabs %}
{% tab title="Simple Cache Config" %}
<pre class="language-python"><code class="lang-python">config = {
    "provider":"mistral-ai",
    "api_key":"YOUR_MISTRAL_AI_API_KEY",
    "override_params": {
        "model":"codestral-latest",
        "max_tokens":64
    },
<strong>    "cache": {
</strong><strong>        "mode": "simple",
</strong><strong>        "max_age": 60000
</strong><strong>  }
</strong>}
</code></pre>
{% endtab %}

{% tab title="Semantic Cache Config" %}
<pre class="language-python"><code class="lang-python">config = {
    "provider":"mistral-ai",
    "api_key":"YOUR_MISTRAL_AI_API_KEY",
    "override_params": {
        "model":"codestral-latest",
        "max_tokens":64
    },
<strong>    "cache": {
</strong><strong>        "mode": "semantic",
</strong><strong>        "max_age": 60000
</strong><strong>  }
</strong>}
</code></pre>
{% endtab %}
{% endtabs %}

[**For more cache settings, check out the documentation here**](../../product/ai-gateway-streamline-llm-integrations/cache-simple-and-semantic.md)**.**

***

## 3. Reliability

Set up fallbacks between different LLMs or providers, load balance your requests across multiple instances or API keys, set automatic retries, or set request timeouts - all set through **Configs**.

{% tabs %}
{% tab title="Fallback from OpenAI to Anthropic" %}
<pre class="language-python"><code class="lang-python">config = {
    "strategy": {
<strong>      "mode": "fallback"
</strong>    },
    "targets": [
    {
<strong>      "virtual_key": "openai-virtual-key",
</strong>      "override_params": {
        "model": "gpt-4o"
      }
    },
    {
<strong>      "virtual_key": "anthropic-virtual-key",
</strong>      "override_params": {
          "model": "claude-3-opus-20240229",
          "max_tokens":64
      }
    }
  ]
}
</code></pre>
{% endtab %}

{% tab title="Load Balance 2 API Keys" %}
<pre class="language-python"><code class="lang-python">config = {
    "strategy": {
<strong>      "mode": "loadbalance"
</strong>    },
    "targets": [
    {
<strong>      "virtual_key": "openai-virtual-key-1",
</strong><strong>      "weight":1
</strong>    },
    {
<strong>      "virtual_key": "openai-virtual-key-2",
</strong><strong>      "weight":1
</strong>    }
  ]
}
</code></pre>
{% endtab %}

{% tab title="Automatic Retries" %}
<pre class="language-python"><code class="lang-python">config = {
<strong>    "retry": {
</strong><strong>        "attempts": 5
</strong><strong>    },
</strong>    "virtual_key": "virtual-key-xxx"
}
</code></pre>
{% endtab %}

{% tab title="Request Timeouts" %}
```python
config = {
  "strategy": { "mode": "fallback" },
  "request_timeout": 10000,
  "targets": [
    { "virtual_key": "open-ai-xxx" },
    { "virtual_key": "azure-open-ai-xxx" }
  ]
}
```
{% endtab %}
{% endtabs %}

Explore deeper documentation for each feature here - [**Fallbacks**](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md), [**Loadbalancing**](../../product/ai-gateway-streamline-llm-integrations/load-balancing.md), [**Retries**](../../product/ai-gateway-streamline-llm-integrations/automatic-retries.md), [**Timeouts**](../../product/ai-gateway-streamline-llm-integrations/request-timeouts.md).

***

## 4. Observability

Portkey automatically logs all the key details about your requests, including cost, tokens used, response time, request and response bodies, and more.

Using Portkey, you can also send custom metadata with each of your requests to further segment your logs for better analytics. Similarly, you can also trace multiple requests to a single trace ID and filter or view them separately in Portkey logs.

{% hint style="info" %}
**Custom Metadata and Trace ID information is sent in `default_headers`.**
{% endhint %}

{% tabs %}
{% tab title="Sending Custom Metadata" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

<strong>config = "pc-xxxx"
</strong>
portkey = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="YOUR_PORTKEY_API_KEY",
        config=config,
<strong>        metadata={
</strong><strong>            "_user": "USER_ID",
</strong><strong>            "environment": "production",
</strong><strong>            "session_id": "1729"
</strong><strong>        }
</strong>    )
)

messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]

resp = portkey.chat(messages)
print(resp)
</code></pre>
{% endtab %}

{% tab title="Sending Trace ID" %}
<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

<strong>config = "pc-xxxx"
</strong>
portkey = OpenAI(
    api_base=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="YOUR_PORTKEY_API_KEY",
        config=config,
<strong>        trace_id="YOUR_TRACE_ID_HERE"
</strong>    )
)

messages = [
    ChatMessage(role="system", content="You are a pirate with a colorful personality"),
    ChatMessage(role="user", content="What is your name"),
]

resp = portkey.chat(messages)
print(resp)
</code></pre>
{% endtab %}
{% endtabs %}

#### Portkey shows these details separately for each log:

<div align="left">

<figure><img src="../../.gitbook/assets/CleanShot 2024-06-01 at 00.01.11@2x.png" alt="" width="356"><figcaption></figcaption></figure>

</div>

[**Check out Observability docs here.**](../../product/observability-modern-monitoring-for-llms/)

***

## 5. Prompt Management

Portkey features an advanced Prompts platform tailor-made for better prompt engineering. With Portkey, you can:

* **Store Prompts with Access Control and Version Control:** Keep all your prompts organized in a centralized location, easily track changes over time, and manage edit/view permissions for your team.
* **Parameterize Prompts**: Define variables and [mustache-approved tags](../../product/prompt-library/prompt-templates.md#templating-engine) within your prompts, allowing for dynamic value insertion when calling LLMs. This enables greater flexibility and reusability of your prompts.
* **Experiment in a Sandbox Environment**: Quickly iterate on different LLMs and parameters to find the optimal combination for your use case, without modifying your LlamaIndex code.

#### Here's how you can leverage Portkey's Prompt Management in your LlamaIndex application:

1. Create your prompt template on the Portkey app, and save it to get an associated **`Prompt ID`**
2. Before making a Llamaindex request, render the prompt template using the Portkey SDK
3. Transform the retrieved prompt to be compatible with LlamaIndex and send the request!

#### Example: Using a Portkey Prompt Template in LlamaIndex

{% tabs %}
{% tab title="Portkey Prompts in LlamaIndex" %}
<pre class="language-python"><code class="lang-python">import json
import os
from llama_index.llms.openai import OpenAI
from llama_index.core.llms import ChatMessage
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders, Portkey
</strong>
###  Initialize Portkey client with API key

client = Portkey(api_key=os.environ.get("PORTKEY_API_KEY"))

### Render the prompt template with your prompt ID and variables

<strong>prompt_template = client.prompts.render(
</strong><strong>    prompt_id="pp-prompt-id",
</strong><strong>    variables={ "movie":"Dune 2" }
</strong><strong>).data.dict()
</strong>
config = {
    "virtual_key":"GROQ_VIRTUAL_KEY", # You need to send the virtual key separately
<strong>    "override_params":{
</strong><strong>        "model":prompt_template["model"], # Set the model name based on the value in the prompt template
</strong><strong>        "temperature":prompt_template["temperature"] # Similarly, you can also set other model params
</strong><strong>    }
</strong>}

portkey = OpenAI(
<strong>    api_base=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        api_key=os.environ.get("PORTKEY_API_KEY"),
</strong><strong>        config=config
</strong><strong>    )
</strong>)

### Transform the rendered prompt into LlamaIndex-compatible format

<strong>messages = [ChatMessage(content=msg["content"], role=msg["role"]) for msg in prompt_template["messages"]]
</strong>
resp = portkey.chat(messages)
print(resp)

</code></pre>
{% endtab %}
{% endtabs %}

[**Explore Prompt Management docs here**](../../product/prompt-library.md).

***

## 6. Continuous Improvement

Now that you know how to trace & log your Llamaindex requests to Portkey, you can also start capturing user feedback to improve your app!

You can append qualitative as well as quantitative feedback to any `trace ID` with the `portkey.feedback.create` method:

{% tabs %}
{% tab title="Adding Feedback" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY"
)

feedback = portkey.feedback.create(
    trace_id="YOUR_LLAMAINDEX_TRACE_ID",
    value=5,  # Integer between -10 and 10
    weight=1,  # Optional
    metadata={
        # Pass any additional context here like comments, _user and more
    }
)

print(feedback)
```
{% endtab %}
{% endtabs %}

[**Check out the Feedback documentation for a deeper dive**](../../product/observability-modern-monitoring-for-llms/feedback.md).

***

## 7. Security & Compliance

When you onboard more team members to help out on your Llamaindex app - permissioning, budgeting, and access management can become a mess! Using Portkey, you can set **budget limits** on provide API keys and implement **fine-grained user roles** and **permissions** to:

* **Control access**: Restrict team members' access to specific features, Configs, or API endpoints based on their roles and responsibilities.
* **Manage costs**: Set budget limits on API keys to prevent unexpected expenses and ensure that your LLM usage stays within your allocated budget.
* **Ensure compliance**: Implement strict security policies and audit trails to maintain compliance with industry regulations and protect sensitive data.
* **Simplify onboarding**: Streamline the onboarding process for new team members by assigning them appropriate roles and permissions, eliminating the need to share sensitive API keys or secrets.
* **Monitor usage**: Gain visibility into your team's LLM usage, track costs, and identify potential security risks or anomalies through comprehensive monitoring and reporting.

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

[**Read more about Portkey's Security & Enterprise offerings here**](../../product/enterprise-offering/).

***

## Join Portkey Community

Join the Portkey Discord to connect with other practitioners, discuss your LlamaIndex projects, and get help troubleshooting your queries.

[**Link to Discord**](https://portkey.ai/community)

For more detailed information on each feature and how to use them, please refer to the [Portkey Documentation](https://portkey.ai/docs).
