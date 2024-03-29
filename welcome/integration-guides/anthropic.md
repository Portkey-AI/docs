# Anthropic

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Anthropic's Claude APIs](https://docs.anthropic.com/claude/reference/getting-started-with-the-api).&#x20;

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) system.

## Portkey SDK Integration with Anthropic

Portkey provides a consistent API to interact with models from various providers. To integrate Anthropic with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Anthropic's API through Portkey's gateway.

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

Set up Portkey with your virtual key as part of the initialization configuration. You can create a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) for Anthropic in the UI.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Anthropic Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Anthropic
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with Anthropic**&#x20;

Use the Portkey instance to send requests to Anthropic. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'claude-instant-1',
    max_tokens: 250 // Required field for Anthropic
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'claude-instant-1',
    max_tokens=250 # Required field for Anthropic
)
    
print(completion.choices)
```
{% endtab %}
{% endtabs %}

## Using Anthropic Vision Models

Portkey's multimodal Gateway fully supports Anthropic's vision models `claude-3-sonnet`, `claude-3-haiku`, and `claude-3-opus`

For more info, check out this guide:

{% content-ref url="../../product/ai-gateway-streamline-llm-integrations/multimodal-capabilities/vision.md" %}
[vision.md](../../product/ai-gateway-streamline-llm-integrations/multimodal-capabilities/vision.md)
{% endcontent-ref %}

## Managing Anthropic Prompts

You can manage all prompts to Anthropic in the [Prompt Library](../../product/prompt-library.md). All the current models of Anthropic are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Anthropic requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Anthropic requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Anthropic's Claude APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
