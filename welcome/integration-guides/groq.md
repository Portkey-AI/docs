# Groq

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Groq APIs](https://console.groq.com/docs/quickstart).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`groq`**</mark>
{% endhint %}

## Portkey SDK Integration with Groq Models

Portkey provides a consistent API to interact with models from various providers. To integrate Groq with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Groq AI's API through Portkey's gateway.

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

To use Groq with Portkey, [get your API key from here](https://console.groq.com/keys), then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Groq Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Groq
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with** Groq

Use the Portkey instance to send requests to Groq. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'mixtral-8x7b-32768',
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'mistral-medium'
)

print(completion)
```
{% endtab %}
{% endtabs %}

## Managing Groq Prompts

You can manage all prompts to Groq in the [Prompt Library](../../product/prompt-library.md). All the current models of Groq are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Supported Models

| Model Name   | Model String to Use in API calls |
| ------------ | -------------------------------- |
| Llama3 8B    | `llama3-8b-8192`                 |
| Llama3 70B   | `llama3-70b-8192`                |
| Mixtral 8x7b | `mixtral-8x7b-32768`             |
| Gemma 7b     | `gemma-7b-it`                    |

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Groq](../../product/ai-gateway-streamline-llm-integrations/configs.md)[ requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Groq requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Groq APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
