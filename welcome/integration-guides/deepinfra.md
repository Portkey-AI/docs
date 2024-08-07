# Deepinfra

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including the models hosted on [Deepinfra API](https://deepinfra.com/models/text-generation).

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`deepinfra`**</mark>
{% endhint %}

## Portkey SDK Integration with Deepinfra Models

Portkey provides a consistent API to interact with models from various providers. To integrate Deepinfra with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Mistral AI's API through Portkey's gateway.

{% tabs %}
{% tab title="NodeJS" %}
```bash
npm install --save portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```bash
pip install portkey-ai
```
{% endtab %}
{% endtabs %}

### **2. Initialize Portkey with the Virtual Key**

To use Deepinfra with Virtual Key, [get your API key from here](https://deepinfra.com/dash/api\_keys).  Then add it to Portkey to create the virtual key

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Deepinfra Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="DEEPINFRA_VIRTUAL_KEY"
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions**

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'nvidia/Nemotron-4-340B-Instruct',
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'nvidia/Nemotron-4-340B-Instruct'
)

print(completion)
```
{% endtab %}
{% endtabs %}

***

## Supported Models

Here's the list of all the Deepinfra models you can route to using Portkey -&#x20;

{% embed url="https://deepinfra.com/models/text-generation" %}

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Deepinfra](../../product/ai-gateway-streamline-llm-integrations/configs.md)[ requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Deepinfra requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Deepinfra](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
