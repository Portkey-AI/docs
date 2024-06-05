---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Completions

## Create Completion

`POST /completions`&#x20;

Generate text completions using the selected Large Language Model (LLM).

{% swagger src="../.gitbook/assets/openapi.yaml" path="/completions" method="post" %}
[openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

The [request body](https://platform.openai.com/docs/api-reference/completions/create) for this endpoint is structured to generate text completions based on a given prompt and model selection. The response will be a [Completion Object](https://platform.openai.com/docs/api-reference/completions/object).

Pass the config parameters for the request in the headers as defined [here](../product/ai-gateway-streamline-llm-integrations/configs.md).

Portkey automatically transforms the parameters for LLMs other than OpenAI. If some parameters don't exist in the other LLMs, they will be dropped.

## SDK Usage

The `completions.create` method in the Portkey SDK allows you to generate text completions using various LLMs. This method provides a straightforward interface for requesting text completions similar to the OpenAI API.

### Method Signature

{% tabs %}
{% tab title="NodeJS" %}
```js
portkey.completions.create(requestParams[configParams]);
```
{% endtab %}

{% tab title="Python" %}
```py
# with only request params
portkey.completions.create(requestParams);
# with request and config params
portkey.with_options(configParams).completions.create(requestParams);
```
{% endtab %}
{% endtabs %}

For REST API examples, scroll [here](completions.md#rest-api-example).

### Parameters

1. **requestParams (Object):** Parameters for the completion request. These parameters should include the prompt and model, and are transformed automatically by Portkey for LLMs other than OpenAI. Unsupported parameters for other LLMs will be dropped.
2. **configParams (Object):** Additional configuration options for the request. This is an optional parameter that can include custom config options for this specific request. These will override the configs set in the Portkey Client.

### Example Usage

{% tabs %}
{% tab title="NodeJS" %}
```javascript
import Portkey from 'portkey-ai';

// Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",  // Replace with your Portkey API key
    virtualKey: "VIRTUAL_KEY"   // Optional: For virtual key management
});

// Generate a text completion
async function getTextCompletion() {
    const completion = await portkey.completions.create({
        prompt: "Say this is a test",
        model: "gpt-3.5-turbo-instruct",
    });

    console.log(completion);
}
await getTextCompletion();
```

```javascript
// Generate a streaming text completion
async function getTextCompletionStream(){
    const completionStream = await portkey.completions.create({
        prompt: "Continuously stream this test",
        model: "gpt-3.5-turbo-instruct",
        stream: true
    });

    for await (const chunk of completionStream) {
        console.log(chunk.content);
    }
}
await getTextCompletionStream();
```

```javascript
// Generate a text completion with config params
async function getTextCompletionWithConfig() {
    const completion = await portkey.completions.create({
        prompt: "Say this is a test with specific config",
        model: "gpt-3.5-turbo-instruct",
    }, {config: "custom-config-123"});

    console.log(completion);
}
await getTextCompletionWithConfig();
```
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Optional: For virtual key management
)

# Generate a text completion
def get_text_completion():
    completion = portkey.completions.create(
        prompt="Say this is a test",
        model="gpt-3.5-turbo-instruct"
    )
    print(completion)

get_text_completion()

```

```python
# Example with config parameters
def get_chat_completion_with_config():
    completion = portkey.with_options({'config': 'sample-7g5tr4'}).completions.create(
        messages=[{'role': 'user', 'content': 'Say this is a test'}],
        model='gpt-3.5-turbo-instruct'
    )
    print(completion)

get_chat_completion_with_config()
```

```python
# Generate a streaming chat completion
async def get_chat_completion_stream():
    completion_stream = portkey.completions.create(
        messages=[{'role': 'user', 'content': 'Say this is a test'}],
        model='gpt-3.5-turbo-instruct',
        stream=True
    })

    for chunk in completion:
        print(chunk.choices[0].delta)

await get_chat_completion_stream()
```
{% endtab %}
{% endtabs %}

<details>

<summary>REST API Example</summary>

In REST calls, `x-portkey-api-key` is a compulsory header, it can be paired with the following options for sending provider details:

1. `x-portkey-provider` & `Authorization` (or similar auth headers)
2. `x-portkey-virtual-key`&#x20;
3. `x-portkey-config`

**Example request using Provider + Auth:**

<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-provider: openai" \
</strong><strong>  -H "Authorization: Bearer $OPENAI_API_KEY" \
</strong>  -d '{
    "model": "gpt-3.5-turbo-instruct",
    "prompt": "Hello!"
  }'
</code></pre>

**Example request using Virtual Key:**&#x20;

<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-virtual-key: openai-virtual-key" \
</strong>  -d '{
    "model": "gpt-3.5-turbo-instruct",
    "prompt": "Hello!"
  }'
</code></pre>

**Example request using Config:**

<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-config: config-key" \
</strong>  -d '{
    "model": "gpt-3.5-turbo-instruct",
    "prompt": "Hello!"
  }'
</code></pre>

**You can send 3 other headers in your Portkey requests**

* `x-portkey-trace-id`: Send trace id&#x20;
* `x-portkey-metadata`: Send custom metadata
* `x-portkey-cache-force-refresh`: Force refresh cache for this request

**Example request using these 3:**

```bash
curl "https://api.portkey.ai/v1/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-config: config-key" \
  -H "x-portkey-trace-id: $UNIQUE_TRACE_ID" \
  -H "x-portkey-metadata: {\"_user\":\"john\"}" \
  -H "x-portkey-cache-force-refresh: True" \
  -d '{
    "model": "gpt-3.5-turbo-instruct",
    "prompt": "Hello!"
  }'
```

</details>

### Response Format

The response will conform to the Text Completions Object schema from the Portkey API, typically including the generated text based on the prompt and the selected model.
