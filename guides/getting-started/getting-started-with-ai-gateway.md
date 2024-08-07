# Getting started with AI Gateway

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1nQa-9EYcv9-O6VnwLATnVd9Q2wFtthOA?usp=sharing)

[Portkey](https://app.portkey.ai/) is the Control Panel for AI apps. With it's popular AI Gateway and Observability Suite, hundreds of teams ship reliable, cost-efficient, and fast apps.

## Quickstart

Since Portkey is fully compatible with the OpenAI signature, you can connect to the Portkey AI Gateway through OpenAI Client.

* Set the `base_url` as `PORTKEY_GATEWAY_URL`
* Add `default_headers` to consume the headers needed by Portkey using the `createHeaders` helper method.

{% tabs %}
{% tab title="Python" %}
Install the OpenAI and Portkey SDK

```bash
pip install -qU portkey-ai openai
```

Create the client

```python
import os
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
  api_key=os.environ.get("OPENAI_API_KEY"),
  base_url=PORTKEY_GATEWAY_URL, # 👈 or 'http://localhost:8787/v1'
  default_headers=createHeaders(
    provider="openai", # 👈 or 'anthropic', 'together-ai', 'stability-ai', etc
    api_key=os.environ.get("PORTKEY_API_KEY") # 👈 skip when self-hosting
  )
)
```
{% endtab %}

{% tab title="Javascript / Typescript" %}
Install the OpenAI and Portkey SDK

```bash
npm install --save openai portkey-ai
```

Create the client

```javascript
import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: PORTKEY_GATEWAY_URL, // 👈 or 'http://localhost:8787/v1'
  defaultHeaders: createHeaders({
    provider: "openai", // 👈 or 'anthropic', 'together-ai', 'stability-ai', etc
    apiKey: "PORTKEY_API_KEY" // 👈 skip when self-hosting
  })
});
```
{% endtab %}

{% tab title="REST API" %}
* Replace the base URL to reflect the AI Gateway (`http://localhost:8787/v1` when running locally or `https://api.portkey.ai/v1` when using the hosted version)
* [Add the relevant headers](../../api-reference/portkey-sdk-client.md#rest-headers) to enable the AI gateway features.
{% endtab %}

{% tab title="Other SDKs" %}
1. Replace the base URL to reflect the AI Gateway (`http://localhost:8787/v1` when running locally or `https://api.portkey.ai/v1` when using the hosted version)
2. [Add the relevant headers](../../api-reference/portkey-sdk-client.md#rest-headers) to enable the AI gateway features.
{% endtab %}
{% endtabs %}

## Examples

### [OpenAI Chat Completion](../../provider-endpoints/completions.md)

Provider: `openai`

Model being tested here: `gpt-4o-mini`

{% tabs %}
{% tab title="Python" %}
```python
import os
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
  api_key=os.environ.get("OPENAI_API_KEY"),
  base_url=PORTKEY_GATEWAY_URL, # 👈 or 'http://localhost:8787/v1'
  default_headers=createHeaders(
    provider="openai",
    api_key=os.environ.get("PORTKEY_API_KEY") # 👈 skip when self-hosting
  )
)

client.chat.completions.create(
  model="gpt-4o-mini",
  messages=[{"role": "user", "content": "What's a fractal?"}],
)
```
{% endtab %}

{% tab title="JS/TS" %}
```javascript
import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: PORTKEY_GATEWAY_URL, // 👈 or 'http://localhost:8787/v1'
  defaultHeaders: createHeaders({
    provider: "openai",
    apiKey: "PORTKEY_API_KEY" // 👈 skip when self-hosting
  })
});

const chatCompletion = await openai.chat.completions.create({
  messages: [{ role: 'user', content: 'Say this is a test' }],
  model: 'gpt-4o-mini',
});
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl https://api.portkey.ai/v1/chat/completions # 👈 or 'http://localhost:8787/v1'
-H "Content-Type: application/json"   
-H "Authorization: Bearer $OPENAI_API_KEY"
-H "x-portkey-provider: openai"
-H "x-portkey-api-key: $PORTKEY_API_KEY" # 👈 skip when self-hosting
-d '{
    "model": "gpt-4o-mini",
    "messages": [{"role": "user","content": "What's a fractal"}]
}'
```
{% endtab %}
{% endtabs %}

```
A fractal is a complex geometric shape that can be split into parts, each of which is a reduced-scale copy of the whole. Fractals are typically self-similar and independent of scale, meaning they look similar at any zoom level. They often appear in nature, in things like snowflakes, coastlines, and fern leaves. The term "fractal" was coined by mathematician Benoit Mandelbrot in 1975.
```

### Anthropic

Provider: `anthropic`

Model being tested here: `claude-3-5-sonnet-20240620`

{% tabs %}
{% tab title="Python" %}
```python
import os
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
  api_key=os.environ.get("OPENAI_API_KEY"),
  base_url=PORTKEY_GATEWAY_URL, # 👈 or 'http://localhost:8787/v1'
  default_headers=createHeaders(
    provider="anthropic",
    api_key=os.environ.get("PORTKEY_API_KEY") # 👈 skip when self-hosting
  )
)

client.chat.completions.create(
  model="claude-3-5-sonnet-20240620",
  messages=[{"role": "user", "content": "What's a fractal?"}],
  max_tokens=250
)
```
{% endtab %}

{% tab title="JS/TS" %}
```javascript
import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: PORTKEY_GATEWAY_URL, // 👈 or 'http://localhost:8787/v1'
  defaultHeaders: createHeaders({
    provider: "anthropic",
    apiKey: "PORTKEY_API_KEY" // 👈 skip when self-hosting
  })
});

const chatCompletion = await openai.chat.completions.create({
  messages: [{ role: 'user', content: 'Say this is a test' }],
  model: 'claude-3-5-sonnet-20240620',
  max_tokens: 250
});
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl https://api.portkey.ai/v1/chat/completions # 👈 or 'http://localhost:8787/v1'
-H "Content-Type: application/json"   
-H "Authorization: Bearer $OPENAI_API_KEY"
-H "x-portkey-provider: anthropic"
-H "x-portkey-api-key: $PORTKEY_API_KEY" # 👈 skip when self-hosting
-d '{
    "model": "claude-3-5-sonnet-20240620",
    "messages": [{"role": "user","content": "What's a fractal"}],
    "max_tokens": 250
}'
```
{% endtab %}
{% endtabs %}

```
A fractal is a complex geometric shape that can be split into parts, each of which is a reduced-scale copy of the whole. Fractals are typically self-similar and independent of scale, meaning they look similar at any zoom level. They often appear in nature, in things like snowflakes, coastlines, and fern leaves. The term "fractal" was coined by mathematician Benoit Mandelbrot in 1975.
```

### Mistral AI

Provider: `mistral-ai`

Model being tested here: `mistral-medium`

{% tabs %}
{% tab title="Python" %}
```python
import os
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
  api_key=os.environ.get("OPENAI_API_KEY"),
  base_url=PORTKEY_GATEWAY_URL, # 👈 or 'http://localhost:8787/v1'
  default_headers=createHeaders(
    provider="mistral-ai",
    api_key=os.environ.get("PORTKEY_API_KEY") # 👈 skip when self-hosting
  )
)

client.chat.completions.create(
  model="mistral-medium",
  messages=[{"role": "user", "content": "What's a fractal?"}],
)
```
{% endtab %}

{% tab title="JS/TS" %}
```javascript
import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: PORTKEY_GATEWAY_URL, // 👈 or 'http://localhost:8787/v1'
  defaultHeaders: createHeaders({
    provider: "mistral-ai",
    apiKey: "PORTKEY_API_KEY" // 👈 skip when self-hosting
  })
});

const chatCompletion = await openai.chat.completions.create({
  messages: [{ role: 'user', content: 'Say this is a test' }],
  model: 'mistral-medium',
});
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl https://api.portkey.ai/v1/chat/completions # 👈 or 'http://localhost:8787/v1'
-H "Content-Type: application/json"   
-H "Authorization: Bearer $OPENAI_API_KEY"
-H "x-portkey-provider: mistral-ai"
-H "x-portkey-api-key: $PORTKEY_API_KEY" # 👈 skip when self-hosting
-d '{
    "model": "mistral-medium",
    "messages": [{"role": "user","content": "What's a fractal"}]
}'
```
{% endtab %}
{% endtabs %}

```
A fractal is a complex geometric shape that can be spl
```

### Together AI

Provider: `together-ai`

&#x20;Model being tested here: `togethercomputer/llama-2-70b-chat`

{% tabs %}
{% tab title="Python" %}
```python
import os
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
  api_key=os.environ.get("OPENAI_API_KEY"),
  base_url=PORTKEY_GATEWAY_URL, # 👈 or 'http://localhost:8787/v1'
  default_headers=createHeaders(
    provider="together-ai",
    api_key=os.environ.get("PORTKEY_API_KEY") # 👈 skip when self-hosting
  )
)

client.chat.completions.create(
  model="togethercomputer/llama-2-70b-chat",
  messages=[{"role": "user", "content": "What's a fractal?"}],
)
```
{% endtab %}

{% tab title="JS/TS" %}
```javascript
import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: PORTKEY_GATEWAY_URL, // 👈 or 'http://localhost:8787/v1'
  defaultHeaders: createHeaders({
    provider: "together-ai",
    apiKey: "PORTKEY_API_KEY" // 👈 skip when self-hosting
  })
});

const chatCompletion = await openai.chat.completions.create({
  messages: [{ role: 'user', content: 'Say this is a test' }],
  model: 'togethercomputer/llama-2-70b-chat',
});
```
{% endtab %}

{% tab title="cURL" %}
```bash
curl https://api.portkey.ai/v1/chat/completions # 👈 or 'http://localhost:8787/v1'
-H "Content-Type: application/json"   
-H "Authorization: Bearer $OPENAI_API_KEY"
-H "x-portkey-provider: together-ai"
-H "x-portkey-api-key: $PORTKEY_API_KEY" # 👈 skip when self-hosting
-d '{
    "model": "togethercomputer/llama-2-70b-chat",
    "messages": [{"role": "user","content": "What's a fractal"}]
}'
```
{% endtab %}
{% endtabs %}

```
A fractal is a complex geometric shape that can be spl
```



### Portkey Supports other Providers

Portkey supports **30+ providers** and all the models within those providers. To use these different providers and models with OpenAI's SDK, you just need to change the `provider` and `model names` in your code with their respective auth keys. It's that easy!\


If you want to see all the providers Portkey works with, check out the [list of providers](https://docs.portkey.ai/providers/supported-providers)[.](../integrations/)

### [OpenAI Embeddings](../../provider-endpoints/embeddings.md)

{% tabs %}
{% tab title="Python" %}
```python
import os
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
  api_key=os.environ.get("OPENAI_API_KEY"),
  base_url=PORTKEY_GATEWAY_URL, # 👈 or 'http://localhost:8787/v1'
  default_headers=createHeaders(
    provider="openai",
    api_key=os.environ.get("PORTKEY_API_KEY") # 👈 skip when self-hosting
  )
)

def get_embedding(text, model="text-embedding-3-small"):
   text = text.replace("\n", " ")
   return client.embeddings.create(input = [text], model=model).data[0].embedding

df['ada_embedding'] = df.combined.apply(lambda x: get_embedding(x, model='text-embedding-3-small'))
df.to_csv('output/embedded_1k_reviews.csv', index=False)





```
{% endtab %}

{% tab title="NodeJS" %}

{% endtab %}
{% endtabs %}

### [OpenAI Function Calling](../../product/ai-gateway-streamline-llm-integrations/multimodal-capabilities/function-calling.md)

{% tabs %}
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
}
</code></pre>
{% endtab %}
{% endtabs %}

### [OpenAI Chat-Vision](../../product/ai-gateway-streamline-llm-integrations/multimodal-capabilities/vision.md)

{% tabs %}
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

response = openai.chat.completions.create(
    model="gpt-4-vision-preview",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "What’s in this image?"},
                {
                    "type": "image_url",
                    "image_url": "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg",
                },
            ],
        }
    ],
    max_tokens=300,
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
  const response = await openai.chat.completions.create({
    model: "gpt-4-vision-preview",
    messages: [
      {
        role: "user",
        content: [
          { type: "text", text: "What’s in this image?" },
          {
            type: "image_url",
            image_url:
              "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg",
          },
        ],
      },
    ],
  });
  
  console.log(response)

}
await getChatCompletionFunctions();
```
{% endtab %}

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
  const response = await portkey.chat.completions.create({
    model: "gpt-4-vision-preview",
    messages: [
      {
        role: "user",
        content: [
          { type: "text", text: "What’s in this image?" },
          {
            type: "image_url",
            image_url:
              "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg",
          },
        ],
      },
    ],
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


response = portkey.chat.completions.create(
    model="gpt-4-vision-preview",
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "text", "text": "What’s in this image?"},
                {
                    "type": "image_url",
                    "image_url": "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg",
                },
            ],
        }
    ],
    max_tokens=300,
)

print(completion)
```
{% endtab %}

{% tab title="REST" %}
```bash
curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: openai" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "model": "gpt-4-vision-preview",
    "messages": [
      {
        "role": "user",
        "content": [
          {
            "type": "text",
            "text": "What’s in this image?"
          },
          {
            "type": "image_url",
            "image_url": {
              "url": "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg"
            }
          }
        ]
      }
    ],
    "max_tokens": 300
  }'
```
{% endtab %}
{% endtabs %}

### [Images](../../provider-endpoints/images/)

{% tabs %}
{% tab title="OpenAI Node JS" %}
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

async function main() {
  const image = await openai.images.generate({ 
    model: "dall-e-3", 
    prompt: "Lucy in the sky with diamonds" 
  });
  
  console.log(image.data);
}

main();
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
from IPython.display import display, Image

client = OpenAI(
    api_key='OPENAI_API_KEY',
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key="PORTKEY_API_KEY"
    )
)

image = client.images.generate(
  model="dall-e-3",
  prompt="Lucy in the sky with diamonds",
  n=1,
  size="1024x1024"
)

# Display the image
display(Image(url=image.data[0].url))
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
import Portkey from 'portkey-ai';

// Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",  // Replace with your Portkey API key
    virtualKey: "VIRTUAL_KEY"   // Add your provider's virtual key
});

async function main() {
  const image = await portkey.images.generate({ 
    model: "dall-e-3", 
    prompt: "Lucy in the sky with diamonds" 
  });
  
  console.log(image.data);
}

main();
```
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey
from IPython.display import display, Image

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Add your provider's virtual key
)

image = portkey.images.generate(
  model="dall-e-3",
  prompt="Lucy in the sky with diamonds"
)

# Display the image
display(Image(url=image.data[0].url))
```
{% endtab %}

{% tab title="REST" %}
```bash
curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: openai-virtual-key" \
  -d '{
    "model": "dall-e-3",
    "prompt": "Lucy in the sky with diamonds"
  }'
```
{% endtab %}
{% endtabs %}

### [OpenAI Audio](../../provider-endpoints/audio/)

Here's an example of Text-to-Speech

{% tabs %}
{% tab title="OpenAI NodeJS" %}
```javascript
import fs from "fs";
import OpenAI from "openai";
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "OPENAI_VIRTUAL_KEY"
  })
});

// Transcription

async function transcribe() {
  const transcription = await openai.audio.transcriptions.create({
    file: fs.createReadStream("/path/to/file.mp3"),
    model: "whisper-1",
  });

  console.log(transcription.text);
}
transcribe();

// Translation

async function translate() {
    const translation = await openai.audio.translations.create({
        file: fs.createReadStream("/path/to/file.mp3"),
        model: "whisper-1",
    });
    console.log(translation.text);
}
translate();
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        virtual_key="OPENAI_VIRTUAL_KEY"
    )
)

audio_file= open("/path/to/file.mp3", "rb")

# Transcription

transcription = client.audio.transcriptions.create(
  model="whisper-1", 
  file=audio_file
)
print(transcription.text)

# Translation

translation = client.audio.translations.create(
  model="whisper-1", 
  file=audio_file
)
print(translation.text)
```
{% endtab %}

{% tab title="REST" %}
For Transcriptions:

```bash
curl "https://api.portkey.ai/v1/audio/transcriptions" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: openai" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H 'Content-Type: multipart/form-data' \
  --form file=@/path/to/file/audio.mp3 \
  --form model=whisper-1
```

For Translations:

```bash
curl "https://api.portkey.ai/v1/audio/translations" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: openai" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H 'Content-Type: multipart/form-data' \
  --form file=@/path/to/file/audio.mp3 \
  --form model=whisper-1
```
{% endtab %}
{% endtabs %}

### &#x20;[OpenAI Batch - Create Batch](../../provider-endpoints/batch/)

{% tabs %}
{% tab title="OpenAI Node" %}
```typescript
import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const client = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "PROVIDER_VIRTUAL_KEY"
  })
});

async function main() {
  const batch = await client.batches.create({
    input_file_id: "file-abc123",
    endpoint: "/v1/chat/completions",
    completion_window: "24h"
  });

  console.log(batch);
}

main();
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        virtual_key="PROVIDER_VIRTUAL_KEY"
    )
)

batch = client.batches.create(
  input_file_id="file-abc123",
  endpoint="/v1/chat/completions",
  completion_window="24h"
)
```
{% endtab %}

{% tab title="Portkey Node" %}
```typescript
import Portkey from 'portkey-ai';

const client = new Portkey({
  apiKey: 'PORTKEY_API_KEY',
  virtualKey: 'PROVIDER_VIRTUAL_KEY'
});

async function main() {
  const batch = await client.batches.create({
    input_file_id: "file-abc123",
    endpoint: "/v1/chat/completions",
    completion_window: "24h"
  });

  console.log(batch);
}

main();
```
{% endtab %}

{% tab title="Portkey Python" %}
```python
from portkey_ai import Portkey

client = Portkey(
  api_key = "PORTKEY_API_KEY",
  virtual_key = "PROVIDER_VIRTUAL_KEY"
)

batch = client.batches.create(
  input_file_id="file-abc123",
  endpoint="/v1/chat/completions",
  completion_window="24h"
)
```
{% endtab %}

{% tab title="CURL" %}
```bash
curl https://api.portkey.ai/v1/batches \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: $PORTKEY_PROVIDER_VIRTUAL_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "input_file_id": "file-abc123",
    "endpoint": "/v1/chat/completions",
    "completion_window": "24h"
  }'
```
{% endtab %}
{% endtabs %}

### [Files - Upload File](../../provider-endpoints/files/)

{% tabs %}
{% tab title="OpenAI Node" %}
```typescript
import fs from "fs";
import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const client = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "PROVIDER_VIRTUAL_KEY"
  })
});

async function main() {
  const file = await client.files.create({
    file: fs.createReadStream("mydata.jsonl"),
    purpose: "batch",
  });

  console.log(file);
}

main();
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        virtual_key="PROVIDER_VIRTUAL_KEY"
    )
)

upload = client.files.create(
  file=open("mydata.jsonl", "rb"),
  purpose="batch"
)
```
{% endtab %}

{% tab title="Portkey Node" %}
```typescript
import fs from "fs";
import Portkey from 'portkey-ai';

const client = new Portkey({
  apiKey: 'PORTKEY_API_KEY',
  virtualKey: 'PROVIDER_VIRTUAL_KEY'
});

async function main() {
  const file = await client.files.create({
    file: fs.createReadStream("mydata.jsonl"),
    purpose: "batch",
  });

  console.log(file);
}

main();
```
{% endtab %}

{% tab title="Portkey Python" %}
```python
from portkey_ai import Portkey

client = Portkey(
  api_key = "PORTKEY_API_KEY",
  virtual_key = "PROVIDER_VIRTUAL_KEY"
)

upload = client.files.create(
  file=open("mydata.jsonl", "rb"),
  purpose="batch"
)
```
{% endtab %}

{% tab title="CURL" %}
```bash
curl https://api.portkey.ai/v1/files \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: $PORTKEY_PROVIDER_VIRTUAL_KEY" \
  -F purpose="fine-tune" \
  -F file="@mydata.jsonl"
```
{% endtab %}
{% endtabs %}

