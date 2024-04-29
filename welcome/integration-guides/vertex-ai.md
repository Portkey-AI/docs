# Google Vertex AI

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Google Vertex AI](https://cloud.google.com/vertex-ai?hl=en).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) system.

## Portkey SDK Integration with Google Vertex AI

Portkey provides a consistent API to interact with models from various providers. To integrate Google Vertex AI with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Google Vertex AI API through Portkey's gateway.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
npm install --save portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```python
pip install portkey-ai
```
{% endtab %}
{% endtabs %}

### **2. Initialize Portkey with the Virtual Key**

Set up Portkey with your virtual key as part of the initialization configuration. You can create a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) for Vertex AI in the UI.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY", // Your Vertex AI Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Google
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with** Vertex AI and Gemini&#x20;

Use the Portkey instance to send requests to Gemini models hosted on Vertex AI. You can also override the virtual key directly in the API call if needed.

{% hint style="warning" %}
Vertex AI uses OAuth2 to authenticate its requests, so you need to send the access token additionally in the config along with the request.
{% endhint %}

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gemini-pro',
}, {authorization: "vertex ai access token here"});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.with_options(authorization="...").chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'gemini-pro'
)

print(completion)
```
{% endtab %}
{% endtabs %}

## Managing Vertex AI Prompts

You can manage all prompts to Google Gemini in the [Prompt Library](../../product/prompt-library.md). All the models in the model garden are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Making Requests Without Virtual Keys <a href="#making-requests-without-virtual-keys" id="making-requests-without-virtual-keys"></a>

Here's how you can pass your Vertex AI details & secrets directly without using the Virtual Keys in Portkey.

Vertex AI expects a region, a project ID and the access token in the request for a successful completion request.

This is how you can specify these fields directly in your requests.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    vertexProjectId: "sample-55646",
    vertexRegion: "us-central1",
    provider:"vertex_ai",
    authorization: "VERTEX_AI_ACCESS_TOKEN"
})

const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gemini-pro',
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    vertex_project_id="sample-55646",
    vertex_region="us-central1",
    provider="vertex_ai",
    authorization="VERTEX_AI_ACCESS_TOKEN"
)

completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'gemini-pro'
)

print(completion)
```
{% endtab %}

{% tab title="REST" %}
```bash
curl --location 'https://api.portkey.ai/v1/chat/completions' \
--header 'x-portkey-provider: vertex-ai' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer VERTEX_AI_ACCESS_TOKEN' \
--header 'x-portkey-api-key: PORTKEY_API_KEY' \
--header 'x-portkey-vertex-project-id: sample-94994' \
--header 'x-portkey-vertex-region: us-central1' \
--data '{
    "messages": [
        {
            "role": "user",
            "content": "Say this is a test"
        }
    ],
    "max_tokens": 20,
    "model": "gemini-pro"
}'
```
{% endtab %}
{% endtabs %}

For further questions on custom Vertex AI deployments or fine-grained access tokens, reach out to us on support@portkey.ai

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Vertex AI requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Vertex AI requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Vertex AI APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
