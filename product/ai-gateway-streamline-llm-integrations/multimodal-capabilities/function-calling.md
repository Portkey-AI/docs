# Function Calling

Portkey's AI gateway supports function calling capabilities that many foundational model providers offer. In the API call you can describe functions and the model can choose to output text or this function name with parameters.

### Functions Usage

Portkey supports the OpenAI signature to define functions as part of the API request. The `tools` parameter accepts functions which can be sent specifically for models that support function calling.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
import Portkey from 'portkey-ai';

// Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",  // Replace with your Portkey API key
    virtualKey: "VIRTUAL_KEY"   // Add your provider's virtual key
});

// Generate a chat completion with streaming
async function getChatCompletionFunctions(){
  const messages = [{"role": "user", "content": "What's the weather like in Boston today?"}];
  const tools = [
      {
        "type": "function",
        "function": {
          "name": "get_current_weather",
          "description": "Get the current weather in a given location",
          "parameters": {
            "type": "object",
            "properties": {
              "location": {
                "type": "string",
                "description": "The city and state, e.g. San Francisco, CA",
              },
              "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
            },
            "required": ["location"],
          },
        }
      }
  ];

  const response = await portkey.chat.completions.create({
    model: "gpt-3.5-turbo",
    messages: messages,
    tools: tools,
    tool_choice: "auto",
  });
  
  console.log(response)

}
await getChatCompletionFunctions();
```
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Add your provider's virtual key
)

tools = [
  {
    "type": "function",
    "function": {
      "name": "get_current_weather",
      "description": "Get the current weather in a given location",
      "parameters": {
        "type": "object",
        "properties": {
          "location": {
            "type": "string",
            "description": "The city and state, e.g. San Francisco, CA",
          },
          "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
        },
        "required": ["location"],
      },
    }
  }
]
messages = [{"role": "user", "content": "What's the weather like in Boston today?"}]
completion = portkey.chat.completions.create(
  model="gpt-3.5-turbo",
  messages=messages,
  tools=tools,
  tool_choice="auto"
)

print(completion)
```
{% endtab %}

{% tab title="OpenAI NodeJS" %}
```javascript
import OpenAI from 'openai'; // We're using the v4 SDK
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    provider: "openai",
    apiKey: "PORTKEY_API_KEY" // defaults to process.env["PORTKEY_API_KEY"]
  })
});

// Generate a chat completion with streaming
async function getChatCompletionFunctions(){
  const messages = [{"role": "user", "content": "What's the weather like in Boston today?"}];
  const tools = [
      {
        "type": "function",
        "function": {
          "name": "get_current_weather",
          "description": "Get the current weather in a given location",
          "parameters": {
            "type": "object",
            "properties": {
              "location": {
                "type": "string",
                "description": "The city and state, e.g. San Francisco, CA",
              },
              "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
            },
            "required": ["location"],
          },
        }
      }
  ];

  const response = await openai.chat.completions.create({
    model: "gpt-3.5-turbo",
    messages: messages,
    tools: tools,
    tool_choice: "auto",
  });
  
  console.log(response)

}
await getChatCompletionFunctions();
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

openai = OpenAI(
    api_key='OPENAI_API_KEY',
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key="PORTKEY_API_KEY"
    )
)

tools = [
  {
    "type": "function",
    "function": {
      "name": "get_current_weather",
      "description": "Get the current weather in a given location",
      "parameters": {
        "type": "object",
        "properties": {
          "location": {
            "type": "string",
            "description": "The city and state, e.g. San Francisco, CA",
          },
          "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
        },
        "required": ["location"],
      },
    }
  }
]
messages = [{"role": "user", "content": "What's the weather like in Boston today?"}]
completion = openai.chat.completions.create(
  model="gpt-3.5-turbo",
  messages=messages,
  tools=tools,
  tool_choice="auto"
)

print(completion)
```
{% endtab %}

{% tab title="REST" %}
<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-provider: openai" \
</strong><strong>  -H "Authorization: Bearer $OPENAI_API_KEY" \
</strong>  -d '{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "What is the weather like in Boston?"
    }
  ],
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "get_current_weather",
        "description": "Get the current weather in a given location",
        "parameters": {
          "type": "object",
          "properties": {
            "location": {
              "type": "string",
              "description": "The city and state, e.g. San Francisco, CA"
            },
            "unit": {
              "type": "string",
              "enum": ["celsius", "fahrenheit"]
            }
          },
          "required": ["location"]
        }
      }
    }
  ],
  "tool_choice": "auto"
}'
</code></pre>
{% endtab %}
{% endtabs %}

#### [API Reference](../../../api-reference/chat-completions.md#id-4.-functions)

On completion, the request will get logged in the logs UI where the tools and functions can be viewed. Portkey will automatically format the JSON blocks in the input and output which makes a great debugging experience.

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### Managing functions and tools in Prompts

TODO

### Supported Providers and Models

The following providers are supported for image generation with more providers getting added soon. Please raise a [request](../../../welcome/integration-guides/suggest-a-new-integration.md) or a [PR](https://github.com/Portkey-AI/gateway/pulls) to add model or provider to the AI gateway.

| Provider                                                            | Models                                                   | Functions                    |
| ------------------------------------------------------------------- | -------------------------------------------------------- | ---------------------------- |
| [OpenAI](../../../welcome/integration-guides/openai.md)             | `dall-e-2`, `dall-e-3`                                   | Create Image (text to image) |
| [Azure OpenAI](../../../welcome/integration-guides/azure-openai.md) | `dall-e-2`, `dall-e-3`                                   | Create Image (text to image) |
| [Stability](../../../welcome/integration-guides/stability-ai.md)    | `stable-diffusion-v1-6`, `stable-diffusion-xl-1024-v1-0` | Create Image (text to image) |
| Replicate (Coming Soon)                                             |                                                          |                              |
| Monster API (Coming Soon)                                           |                                                          |                              |

### Cookbook

Here's a detailed cookbook on image generation using Portkey which demonstrates the use of multiple providers and routing between them through [Configs](../configs.md).

{% hint style="success" %}
[https://github.com/Portkey-AI/portkey-cookbook/blob/main/examples/image-generation.ipynb](https://github.com/Portkey-AI/portkey-cookbook/blob/main/examples/image-generation.ipynb)
{% endhint %}
