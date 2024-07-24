---
description: >-
  MonsterAPIs provides access to generative AI model APIs at 80% lower costs.
  Connect to MonsterAPI LLM APIs seamlessly through Portkey's AI gateway.
---

# Monster API

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [MonsterAPI APIs](https://developer.monsterapi.ai/docs/getting-started).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`monsterapi`**</mark>
{% endhint %}

## Portkey SDK Integration with MonsterAPI Models

Portkey provides a consistent API to interact with models from various providers. To integrate MonsterAPI with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with MonsterAPI's API through Portkey's gateway.

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

To use Monster API with Portkey, [get your API key from here,](https://monsterapi.ai/user/dashboard) then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your MonsterAPI Virtual Key
})
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with** MonsterAPI

Use the Portkey instance to send requests to MonsterAPI. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'TinyLlama/TinyLlama-1.1B-Chat-v1.0',
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

## Managing MonsterAPI Prompts

You can manage all prompts to MonsterAPI in the [Prompt Library](../../product/prompt-library.md). All the current models of MonsterAPI are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Supported Models

[Find the latest list of supported models here.](https://llm.monsterapi.ai/docs)

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your MonsterAPI requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing MonsterAPI requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to MonsterAPI APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
