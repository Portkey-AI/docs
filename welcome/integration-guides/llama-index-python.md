# LlamaIndex (Python)

The **Portkey x LlamaIndex** integration brings advanced AI gateway capabilities and full-stack observability to LlamaIndex, making it a breeze to build production-ready Llamaindex apps.

In a nutshell, Portkey extends the familiar OpenAI schema to make Llamaindex work with **200+ LLMs** without having to import different classes for each provider or having to configuring your code separately. And by doing that, Portkey makes your Llamaindex calls reliable, fast, and cost-efficient, along with logging all calls to help you debug errors and get granular insights.

## Quick Start

1. Import the `OpenAI` class in Llamaindex, along with Portkey's helper functions `createHeaders` and `PORTKEY_GATEWAY_URL`
2. Configure your model details separately following Portkey's [Config object schema](../../api-reference/config-object.md) - this is where you can define provider and model name, model parameters, set up fallbacks / retries / etc, and even pass your prompt templates saved on Portkey!
3. Pass your Config details along with the necessary headers while instantiating your OpenAI client

That's it!

## Install the Portkey SDK

```bash
pip install -U portkey-ai
```

## Integrate OpenAI

Here are basic examples on using the `complete` and `chat` methods with `streaming` on & off.

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

## Enabling Portkey Features

By routing your Llamaindex requests through Portkey, you get the following 6 production-grade features:

1. Interoperability
2. Caching
3. Reliability
4. Observability
5. Continuous Improvement
6. Security & Compliance

Let's explore each.

As you will see below, much of these features are driven by Portkey's Config architecture. On the Portkey app, we make it very easy to help you create, manage, and version Configs so that you can reference them easily here.

## Saving Configs in the Portkey App

Just go to Portkey app and you can save all of the Config snippets shown in the examples below and reference them with just a slug.

<figure><img src="../../.gitbook/assets/CleanShot 2024-06-01 at 00.18.08@2x.png" alt=""><figcaption></figcaption></figure>

## Interoperability - Calling Anthropic, Gemini, Mistral, and more

Now that we have the OpenAI code up and running, let's see how you can use Portkey to send the request across multiple LLMs - we'll show **Anthropic**, **Gemini**, and **Mistral**. For the full list of providers & LLMs supported, check out [this doc](../../guides/practitioners-cookbooks/integrations/).

For each provider, it is just a matter of changing 3 lines of code in the Config. Change the provider name, change the API key, and change the model name, that's it!

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

## Save Configs in Portkey App

The Configs that you are defining in the above (and below) examples - you can also create, manage, and version them in the Portkey app! This makes it eas

Let's see how you can send the same requests across Azure OpenAI, Google Vertex AI, and AWS Bedrock.

{% hint style="info" %}
We recommend to save your cloud details to Portkey vault and get a corresponding Virtual Key. This reduces the hassle of remembering deployment name, resource name, api version for Azure, and similarly region, access key, project id details etc for Bedrock and Vertex.\
\
[Explore the Virtual Key documentation here](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/).
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

As you can see across ALL these examples, it is ONLY the `config` that's changing, while the rest of the code stays identical. This will continue for other Portkey features as well!

## Caching

You can speed up your requests and save money on your LLM requests by storing past responses in the Portkey cache. There are 2 cache modes:

* **Simple:** Matches requests verbatim. Perfect for repeated, identical prompts. Works on **all models** including image generation models.
* **Semantic:** Matches responses for requests that are semantically similar. Ideal for denoising requests with extra prepositions, pronouns, etc. Works on any model available on **`/chat/completions`** or **`/completions`** routes.

To enable Portkey cache, just add the `cache` params to your [config object](https://portkey.ai/docs/api-reference/config-object#cache-object-details).

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

[For more cache settings, check out the documentation here](../../product/ai-gateway-streamline-llm-integrations/cache-simple-and-semantic.md).

## Reliability

Set up fallbacks between different LLMs or providers, load balance your requests across multiple instances or API keys, set automatic retries, or set request timeouts - all again set through Configs. Here's how:

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

Explore deeper documentation for each feature here - [Fallbacks](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md), [Loadbalancing](../../product/ai-gateway-streamline-llm-integrations/load-balancing.md), [Retries](../../product/ai-gateway-streamline-llm-integrations/automatic-retries.md), [Timeouts](../../product/ai-gateway-streamline-llm-integrations/request-timeouts.md).

## Observability

Portkey automatically logs all the key details about your requests automatically - cost, tokens used, response time, request and response bodies, and more. Not just that, using Portkey, you can also send custom metadata with each of your requests to further segment your logs on Portkey for better analytics. Similarly, if you are maintaining a chat session with a single user, or maintaing any kind of long context, you can also trace multiple requests to a single trace ID and filter or view them separately in Portkey logs.

Custom Metadata and Trace ID information is sent in headers.

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

When you do this, Portkey will show you these details separately for each log, and let you filter the logs & analytics on them easily:

<div align="left">

<figure><img src="../../.gitbook/assets/CleanShot 2024-06-01 at 00.01.11@2x.png" alt="" width="356"><figcaption></figcaption></figure>

</div>

## Continuous Improvement

Now that you know how to trace & log your Llamaindex requests to Portkey, you can also start capturing the user feedback on your requests - it's very easy!

Just make sure to add trace ID to your requests, and you can just append qualitative as well as quantitative feedback with `portkey.feedback.create` method:

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

## Security & Compliance

When you onboard more team members to your Llamaindex app - you may not want to share all of your secrets with everyone, or give all team members permissions to edit your various Configs and just restrict them to do generations. Using Portkey, you can set budget limits on provide API keys, and implement fine-grained user roles and permissions for both the app and the Portkey APIs.

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

[**Read more about Portkey's Security & Enterprise offerings here**](../../product/enterprise-offering/).

## Join Portkey Community

On the Portkey Discord, you will often find engineers discussing what they are building on top of Llamaindex and help each other out. Join the community to connect with other practitioners and troubleshoot your queries!

[**Link to Discord**](https://portkey.ai/community)
