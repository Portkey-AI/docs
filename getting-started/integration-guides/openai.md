---
description: >-
  Learn to integrate OpenAI with Portkey, enabling seamless completions, prompt
  management, and advanced functionalities like streaming, function calling and
  fine-tuning.
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

# OpenAI

Portkey seamlessly integrates with OpenAI SDKs for Node.js, Python, and REST APIs. For OpenAI integration using other frameworks, explore our partnerships, including [Langchain](langchain-python.md), [LlamaIndex](llama-index-python.md), among [others](./).

## Using the Portkey Gateway

To integrate the Portkey gateway with OpenAI,&#x20;

* Set the `baseURL` to the Portkey Gateway URL
* Include Portkey-specific headers such as `mode` and `apiKey`and others.

Here's how to apply it to a **chat completion** request:

{% tabs %}
{% tab title="OpenAI NodeJS" %}
1. Install the Portkey SDK in your application

{% code fullWidth="true" %}
```sh
npm i --save portkey-ai
```
{% endcode %}

2. Next, insert the Portkey-specific code as shown in the highlighted lines to your OpenAI completion calls. `PORTKEY_GATEWAY_URL` is portkey's gateway URL to route your requests and `createHeaders` is a convenience function that generates the headers object. ([All supported params/headers](../../api-reference/portkey-sdk-client.md#nodejs-3))

<pre class="language-javascript"><code class="lang-javascript">import OpenAI from 'openai'; // We're using the v4 SDK
<strong>import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'
</strong>
const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: PORTKEY_GATEWAY_URL,
<strong>  defaultHeaders: createHeaders({
</strong><strong>    provider: "openai",
</strong><strong>    apiKey: "PORTKEY_API_KEY" // defaults to process.env["PORTKEY_API_KEY"]
</strong><strong>  })
</strong>});

async function main() {
  const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
  });

  console.log(chatCompletion.choices);
}

main();
</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
1. Install the Portkey SDK in your application

```sh
pip install portkey-ai
```

2. Next, insert the Portkey-specific code as shown in the highlighted lines to your OpenAI function calls. `PORTKEY_GATEWAY_URL` is portkey's gateway URL to route your requests and `createHeaders` is a convenience function that generates the headers object. ([All supported params/headers](../../api-reference/portkey-sdk-client.md#python-3))

```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    api_key="OPENAI_API_KEY", # defaults to os.environ.get("OPENAI_API_KEY")
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key="PORTKEY_API_KEY" # defaults to os.environ.get("PORTKEY_API_KEY")
    )
)

chat_complete = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
)

print(chat_complete.choices[0].message.content)
```
{% endtab %}

{% tab title="REST API" %}
```bash
curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: openai" \ 
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{
        "role": "system",
        "content": "You are a helpful assistant."
      },{
        "role": "user",
        "content": "Hello!"
      }]
  }'
```

[List of all possible headers](../../api-reference/portkey-sdk-client.md#rest-headers)
{% endtab %}
{% endtabs %}

This request will be automatically logged by Portkey. You can view this in your logs dashboard. Portkey logs the tokens utilized, execution time, and cost for each request. Additionally, you can delve into the details to review the precise request and response data.

The same integration approach applies to APIs for [`completions`](https://platform.openai.com/docs/guides/text-generation/completions-api), [`embeddings`](https://platform.openai.com/docs/api-reference/embeddings/create), [`vision`](https://platform.openai.com/docs/guides/vision/quick-start), [`moderation`](https://platform.openai.com/docs/api-reference/moderations/create), [`transcription`](https://platform.openai.com/docs/api-reference/audio/createTranscription), [`translation`](https://platform.openai.com/docs/api-reference/audio/createTranslation), [`speech`](https://platform.openai.com/docs/api-reference/audio/createSpeech) and [`files`](https://platform.openai.com/docs/api-reference/files/create)&#x20;

## Using the Prompts API

Portkey also supports creating and managing prompt templates in the [prompt library](../../product/prompt-library.md). This enables the collaborative development of prompts directly through the user interface.

1. Create a prompt template with variables and set the hyperparameters.&#x20;

<figure><img src="../../.gitbook/assets/prompt creation.gif" alt=""><figcaption></figcaption></figure>

2. Use this prompt in your codebase using the Portkey SDK.

{% tabs %}
{% tab title="Node" %}
```javascript
import Portkey from 'portkey-ai'

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
})

// Make the prompt creation call with the variables
const promptCompletion = portkey.prompts.completions.create({
    promptID: "Your Prompt ID",
    variables: {
       // The variables specified in the prompt
    }
})
```

```javascript
// We can also override the hyperparameters
const promptCompletion = portkey.prompts.completions.create({
    promptID: "Your Prompt ID",
    variables: {
       // The variables specified in the prompt
    },
    max_tokens: 250,
    presence_penalty: 0.2
})
```
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey

client = Portkey(
    api_key="PORTKEY_API_KEY",  # defaults to os.environ.get("PORTKEY_API_KEY")
)

prompt_completion = client.prompts.completions.create(
    prompt_id="Your Prompt ID",
    variables={
       # The variables specified in the prompt
    }
)

print(prompt_completion)

# We can also override the hyperparameters
prompt_completion = client.prompts.completions.create(
    prompt_id="Your Prompt ID",
    variables={
       # The variables specified in the prompt
    },
    max_tokens=250,
    presence_penalty=0.2
)
print(prompt_completion)
```
{% endtab %}

{% tab title="REST API" %}
```bash
curl -X POST "https://api.portkey.ai/v1/prompts/PROMPT_ID/create" \
-H "Content-Type: application/json" \
-H "x-portkey-api-key: $PORTKEY_API_KEY" \
-d '{
    "variables": {
        # The variables to use
    },
    "max_tokens": 250, # Optional
    "presence_penalty": 0.2 # Optional
}'
```
{% endtab %}
{% endtabs %}

Observe how this streamlines your code readability and simplifies prompt updates via the UI without altering the codebase.

## Advanced Use Cases

### Streaming Responses

Portkey supports streaming responses using Server Sent Events (SSE).

{% tabs %}
{% tab title="OpenAI NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    mode: "openai",
    apiKey: "PORTKEY_API_KEY" // defaults to process.env["PORTKEY_API_KEY"]
  })
});

async function main() {
  const stream = await openai.chat.completions.create({
    model: 'gpt-4',
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    stream: true,
</strong>  });
  for await (const chunk of stream) {
    process.stdout.write(chunk.choices[0]?.delta?.content || '');
  }
}

main();

</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
<pre class="language-python"><code class="lang-python">from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    api_key="OPENAI_API_KEY",  # defaults to os.environ.get("OPENAI_API_KEY")
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key="PORTKEY_API_KEY" # defaults to os.environ.get("PORTKEY_API_KEY")
    )
)

chat_complete = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
<strong>    stream=True
</strong>)

for chunk in chat_complete:
    print(chunk.choices[0].delta.content, end="", flush=True)

</code></pre>
{% endtab %}
{% endtabs %}

### Function Calling

Function calls within your OpenAI or Portkey SDK operations remain standard. These logs will appear in Portkey, highlighting the utilized functions and their outputs.

Additionally, you can define functions within your prompts and invoke the `portkey.prompts.completions.create` method as above.

### Fine-tuning

Please refer to our fine-tuning guides to take advantage of Portkey's advanced [continuous fine-tuning](../../product/autonomous-fine-tuning/) capabilities.

### Portkey Features

Portkey supports the complete host of it's functionality via the OpenAI SDK so you don't need to migrate away from it.

Please find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to the OpenAI client or a single request](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing OpenAI requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback to Azure OpenAI](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
