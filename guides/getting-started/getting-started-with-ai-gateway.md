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

### OpenAI Chat Completion

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

Model being tested here: `togethercomputer/llama-2-70b-chat`

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

### Other Providers

Portkey supports 30+ providers and all the models within those providers. To use them with the OpenAI SDKs or APIs, update the provider and the model to start using those models.

[List of all providers](../../welcome/integration-guides/#supported-ai-providers)
