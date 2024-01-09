# Chat Completions

## Create Chat Completion

`POST /chat/completions`

Generate a chat message completion from the selected LLM.

{% swagger src="../.gitbook/assets/openapi.yaml" path="/chat/completions" method="post" %}
[openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}

The body is similar to the [chat completion request](https://platform.openai.com/docs/api-reference/chat/create) of OpenAI and the response will be the [Chat Completion Object](https://platform.openai.com/docs/api-reference/chat/object). When choosing `stream:true` the response will be a stream of [Chat Completion Chunk](https://platform.openai.com/docs/api-reference/chat/streaming) objects.

Portkey automatically transforms the parameters for LLMs other than OpenAI. If some parameters don't exist in the other LLMs, they will be dropped.

## SDK Usage

The `chat.completions.create` method in the Portkey SDK enables you to generate chat completions using various Large Language Models (LLMs). This method is designed to be similar to the OpenAI chat completions API, offering a familiar interface for those accustomed to OpenAI's services.

### Method Signature

{% tabs %}
{% tab title="NodeJS" %}
```javascript
portkey.chat.completions.create(requestParams[, configParams]);
```
{% endtab %}

{% tab title="Python" %}
```python
# with only request params
portkey.chat.completions.create(**requestParams);

# with request and config params
portkey.with_options(**configParams).chat.completions.create(**requestParams);
```
{% endtab %}
{% endtabs %}

For REST API examples, scroll [here](chat-completions.md#rest-api-example).

### Parameters

1. **requestParams (Object)**:  Parameters for the chat completion request, detailing the chat interaction. These are similar to the [OpenAI request signature](https://platform.openai.com/docs/api-reference/chat/create). Portkey automatically transforms the parameters for LLMs other than OpenAI. If some parameters don't exist in the other LLMs, they will be dropped.
2. **configParams(Object)**: Additional configuration options for the request. This is an optional parameter that can include custom config options for this specific request. These will override the configs set in the Portkey Client. A full list of these config parameters can be found [here](../product/ai-gateway-streamline-llm-integrations/configs.md).

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

// Generate a chat completion
async function getChatCompletion() {
    const chatCompletion = await portkey.chat.completions.create({
        messages: [{ role: 'user', content: 'Say this is a test' }],
        model: 'gpt-3.5-turbo',
    });

    console.log(chatCompletion);
}
await getChatCompletion();
```

```javascript
// Generate a chat completion with streaming
async function getChatCompletionStream(){
    const chatCompletion = await portkey.chat.completions.create({
        messages: [{ role: 'user', content: 'Say this is a test' }],
        model: 'gpt-3.5-turbo',
        stream: true
    });

    for await (const chunk of chatCompletion) {
        console.log(chunk.choices[0].delta.content);
    }
}
await getChatCompletionStream();
```

```javascript
// Generate streaming chat completion with config params
async function getChatCompletionWithConfig() {
    const chatCompletion = await portkey.chat.completions.create({
        messages: [{ role: 'user', content: 'Say this is a test' }],
        model: 'gpt-3.5-turbo',
        stream: true
    }, {config: "sample-7g5tr4"});

    for await (const chunk of chatCompletion) {
        console.log(chunk.choices[0].delta.content);
    }
}
await getChatCompletionWithConfig();
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

# Generate a chat completion
def get_chat_completion():
    chat_completion = portkey.chat.completions.create(
        messages=[{'role': 'user', 'content': 'Say this is a test'}],
        model='gpt-3.5-turbo'
    )
    print(chat_completion)

get_chat_completion()
```

```python
# Example with config parameters
def get_chat_completion_with_config():
    chat_completion = portkey.with_options(config='sample-7g5tr4').chat.completions.create(
        messages=[{'role': 'user', 'content': 'Say this is a test'}],
        model='gpt-3.5-turbo'
    )
    print(chat_completion)

get_chat_completion_with_config()
```

```python
# Generate a streaming chat completion
def get_chat_completion_stream():
    chat_completion_stream = portkey.with_options(config='sample-7g5tr4').chat.completions.create(
        messages=[{'role': 'user', 'content': 'Say this is a test'}],
        model='gpt-3.5-turbo',
        stream=True
    )

    for chunk in chat_completion_stream:
        print(chunk.choices[0].delta)

get_chat_completion_stream()
```
{% endtab %}
{% endtabs %}

<details>

<summary>REST API Examples</summary>

In REST calls, `x-portkey-api-key` is a compulsory header, it can be paired with the following options for sending provider details:

1. `x-portkey-provider` & `Authorization` (or similar auth headers)
2. `x-portkey-virtual-key`&#x20;
3. `x-portkey-config`&#x20;

**Example request using Provider + Auth:**

<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-provider: openai" \
</strong><strong>  -H "Authorization: Bearer $OPENAI_API_KEY" \
</strong>  -d '{
    "model": "gpt-4",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
</code></pre>

**Example request using Virtual Key:**

<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-virtual-key: openai-virtual-key" \
</strong>  -d '{
    "model": "gpt-4",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
</code></pre>

**Example request using Config:**

<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-config: config-key" \
</strong>  -d '{
    "model": "gpt-4",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
</code></pre>

**You can send 3 other headers in your Portkey requests**

* `x-portkey-trace-id`: Send trace id&#x20;
* `x-portkey-metadata`: Send custom metadata
* `x-portkey-cache-force-refresh`: Force refresh cache for this request

**Example request using these 3:**

<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-config: config-key" \
<strong>  -H "x-portkey-trace-id: $UNIQUE_TRACE_ID" \
</strong><strong>  -H "x-portkey-metadata: {\"_user\":\"john\"}" \
</strong><strong>  -H "x-portkey-cache-force-refresh: True" \
</strong>  -d '{
    "model": "gpt-4",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
</code></pre>

</details>

### Response Format

The response will conform to the `Chat Completions Object` schema from the Portkey API, typically including generated messages and relevant metadata.&#x20;
