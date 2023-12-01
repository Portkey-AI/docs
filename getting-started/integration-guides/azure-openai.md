---
description: >-
  Azure OpenAI is a great alternative to accessing the best models including
  GPT-4 and more in your private environments. Portkey provides complete support
  for Azure OpenAI.
---

# Azure OpenAI

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) system.

## Portkey SDK Integration with Azure OpenAI

Portkey provides a consistent API to interact with models from various providers. To integrate Azure OpenAI with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Azure OpenAI's API through Portkey's gateway.

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

Set up Portkey with your virtual key as part of the initialization configuration. You can create a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) for Azure in the UI.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Azure Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    apiKey="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtualKey="VIRTUAL_KEY"   # Replace with your virtual key for Azure
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with Azure OpenAI**&#x20;

Use the Portkey instance to send requests to your Azure deployments. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt4', // This would be your deployment or model name
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ role: 'user', content: 'Say this is a test' }],
    model= 'claude-2'
})

print(completion.choices)
```
{% endtab %}
{% endtabs %}

## Managing Azure OpenAI Prompts

You can manage all prompts to Azure OpenAI in the [Prompt Library](../../product/prompt-library.md). All the current models of OpenAI are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.
