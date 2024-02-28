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

### First, add your Azure details to Portkey's Virtual Keys

<div align="left">

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (2).png" alt="" width="375"><figcaption></figcaption></figure>

</div>

**Here's a step-by-step guide:**

1. Request access to Azure OpenAI [here](https://aka.ms/oai/access).
2. Create a resource in the Azure portal [here](https://portal.azure.com/?microsoft\_azure\_marketplace\_ItemHideKey=microsoft\_openai\_tip#create/Microsoft.CognitiveServicesOpenAI). (This will be your **Resource Name**)
3. Deploy a model in Azure OpenAI Studio [here](https://oai.azure.com/). (This will be your **Deployment Name)**
4. Now, on Azure OpenAI studio, go to any playground (chat or completions), click on a UI element called "View code". Note down the API version & API key from here. (This will be your **Azure API Version** & **Azure API Key**)

When you input these details, the foundation model will be auto populated. More details in [this guide](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource?pivots=web-portal).

Now, let's make a request using this virtual key!

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
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Azure
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
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'custom_model_name'
)

print(completion.choices)
```
{% endtab %}
{% endtabs %}

## Managing Azure OpenAI Prompts

You can manage all prompts to Azure OpenAI in the [Prompt Library](../../product/prompt-library.md). All the current models of OpenAI are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Azure OpenAI requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Azure OpenAI requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Azure OpenAI APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
