---
description: >-
  Azure OpenAI is a great alternative to accessing the best models including
  GPT-4 and more in your private environments. Portkey provides complete support
  for Azure OpenAI.
---

# Azure OpenAI

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`azure-openai`**</mark>
{% endhint %}

## Portkey SDK Integration with Azure OpenAI

Portkey provides a consistent API to interact with models from various providers. To integrate Azure OpenAI with Portkey:

### First, add your Azure details to Portkey's Virtual Keys

<figure><img src="../../.gitbook/assets/CleanShot 2024-07-27 at 13.30.46@2x.png" alt=""><figcaption></figcaption></figure>

**Here's a step-by-step guide:**

1. Request access to Azure OpenAI [here](https://aka.ms/oai/access).
2. Create a resource in the Azure portal [here](https://portal.azure.com/?microsoft\_azure\_marketplace\_ItemHideKey=microsoft\_openai\_tip#create/Microsoft.CognitiveServicesOpenAI). (This will be your **Resource Name**)
3. Deploy a model in Azure OpenAI Studio [here](https://oai.azure.com/). (This will be your **Deployment Name)**
4. Select your `Foundation Model` from the dropdowon on the modal.
5. Now, on Azure OpenAI studio, go to any playground (chat or completions), click on a UI element called "View code". Note down the API version & API key from here. (This will be your **Azure API Version** & **Azure API Key**)

When you input these details, the foundation model will be auto populated. More details in [this guide](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/create-resource?pivots=web-portal).&#x20;

{% hint style="info" %}
If you do not want to add your Azure details to Portkey vault, you can also directly pass them while instantiating the Portkey client. [More on that here.](azure-openai.md#making-requests-without-virtual-keys)
{% endhint %}

**Now, let's make a request using this virtual key!**

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Azure OpenAI's API through Portkey's gateway.

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

Set up Portkey with your virtual key as part of the initialization configuration. You can create a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) for Azure in the Portkey UI.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
<strong>    virtualKey: "AZURE_VIRTUAL_KEY" // Your Azure Virtual Key
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
<strong>    virtual_key="AZURE_VIRTUAL_KEY"   # Replace with your virtual key for Azure
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with Azure OpenAI**&#x20;

Use the Portkey instance to send requests to your Azure deployments. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    model: 'gpt4', // This would be your deployment or model name
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model= 'custom_model_name'
</strong>)

print(completion.choices)
</code></pre>
{% endtab %}
{% endtabs %}

## Managing Azure OpenAI Prompts

You can manage all prompts to Azure OpenAI in the [Prompt Library](../../product/prompt-library.md). All the current models of OpenAI are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Image Generation

Portkey supports multiple modalities for Azure OpenAI and you can make image generation requests through Portkey's AI Gateway the same way as making completion calls.

{% tabs %}
{% tab title="Portkey NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
<strong>    virtualKey: "DALL-E_VIRTUAL_KEY" // Referencing a Dall-E Azure deployment with Virtual Key
</strong>})

const image = await portkey.images.generate({
  prompt:"Lucy in the sky with diamonds",
  size:"1024x1024"
})
</code></pre>
{% endtab %}

{% tab title="Portkey Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  
<strong>    virtual_key="DALL-E_VIRTUAL_KEY"   # Referencing a Dall-E Azure deployment with Virtual Key
</strong>)

image = portkey.images.generate(
  prompt="Lucy in the sky with diamonds",
  size="1024x1024"
)
</code></pre>
{% endtab %}
{% endtabs %}

Portkey's fast AI gateway captures the information about the request on your Portkey Dashboard. On your logs screen, you'd be able to see this request with the request and response.

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption><p>Log view for an image generation request on Azure OpenAI</p></figcaption></figure>

More information on image generation is available in the [API Reference](https://portkey.ai/docs/api-reference/completions-1#create-image).

***

## Making Requests Without Virtual Keys <a href="#making-requests-without-virtual-keys" id="making-requests-without-virtual-keys"></a>

Here's how you can pass your Azure OpenAI details & secrets directly without using the Virutal Keys feature.

### Key Mapping

In a typical Azure OpenAI request,&#x20;

<pre class="language-bash"><code class="lang-bash">curl https://<a data-footnote-ref href="#user-content-fn-1">{YOUR_RESOURCE_NAME}</a>.openai.azure.com/openai/deployments/<a data-footnote-ref href="#user-content-fn-2">{YOUR_DEPLOYMENT_NAME}</a>/chat/completions?api-version=<a data-footnote-ref href="#user-content-fn-3">{API_VERSION}</a> \
  -H "Content-Type: application/json" \
  -H "api-key: <a data-footnote-ref href="#user-content-fn-4">{YOUR_API_KEY}</a>" \
  -d '{"messages": [{"role": "system", "content": "Hello"}] }'
</code></pre>

| Parameter               | Node SDK                              | Python SDK                             | REST Headers                    |
| ----------------------- | ------------------------------------- | -------------------------------------- | ------------------------------- |
| `AZURE RESOURCE NAME`   | `azureResourceName`                   | `azure_resource_name`                  | `x-portkey-azure-resource-name` |
| `AZURE DEPLOYMENT NAME` | `azureDeploymentId`                   | `azure_deployment_id`                  | `x-portkey-azure-deployment-id` |
| `API VERSION`           | `azureApiVersion`                     | `azure_api_version`                    | `x-portkey-azure-api-version`   |
| `AZURE API KEY`         | `Authorization: "Bearer + {API_KEY}"` | `Authorization = "Bearer + {API_KEY}"` | `Authorization`                 |
| `AZURE MODEL NAME`      | `azureModelName`                      | `azure_model_name`                     | `x-portkey-azure-model-name`    |

### Example

{% tabs %}
{% tab title="Node" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
<strong>    provider: "azure-openai",
</strong><strong>    azureResourceName: "AZURE_RESOURCE_NAME",
</strong><strong>    azureDeploymentId: "AZURE_DEPLOYMENT_NAME",
</strong><strong>    azureApiVersion: "AZURE_API_VERSION",
</strong><strong>    azureModelName: "AZURE_MODEL_NAME"
</strong><strong>    Authorization: "Bearer API_KEY"
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key = "PORTKEY_API_KEY",  
<strong>    provider = "azure-openai",
</strong><strong>    azure_resource_name = "AZURE_RESOURCE_NAME",
</strong><strong>    azure_deployment_id = "AZURE_DEPLOYMENT_NAME",
</strong><strong>    azure_api_version = "AZURE_API_VERSION",
</strong><strong>    azure_model_name = "AZURE_MODEL_NAME",
</strong><strong>    Authorization = "Bearer API_KEY"
</strong>)
</code></pre>
{% endtab %}

{% tab title="REST" %}
```bash
curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $AZURE_OPENAI_API_KEY" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: azure-openai" \
  -H "x-portkey-azure-resource-name: $AZURE_RESOURCE_NAME" \
  -H "x-portkey-azure-deployment-id: $AZURE_DEPLOYMENY_ID" \
  -H "x-portkey-azure-model-name: $AZURE_MODEL_NAME" \
  -H "x-portkey-azure-api-version: $AZURE_API_VERSION" \
  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user","content": "Hello!"}]
  }'
```
{% endtab %}
{% endtabs %}

### How to Pass JWT (JSON Web Tokens)

If you have configured fine-grained access for Azure OpenAI and need to use **`JSON web token (JWT)`** in the **`Authorization`** header instead of the regular **`API Key`**, you can use the `forwardHeaders` parameter to do this.

{% tabs %}
{% tab title="Node" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    provider: "azure-openai",
    azureResourceName: "AZURE_RESOURCE_NAME",
    azureDeploymendId: "AZURE_DEPLOYMENT_NAME",
    azureApiVersion: "AZURE_API_VERSION",
    azureModelName: "AZURE_MODEL_NAME",
<strong>    authorization: "Bearer JWT_KEY", // Pass your JWT here
</strong><strong>    forwardHeaders: [ "authorization" ]
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    api_key = "PORTKEY_API_KEY",
    provider = "azure-openai",
    azure_resource_name = "AZURE_RESOURCE_NAME",
    azure_deploymend_id = "AZURE_DEPLOYMENT_NAME",
    azure_api_version = "AZURE_API_VERSION",
    azure_model_name = "AZURE_MODEL_NAME",
<strong>    authorization = "Bearer API_KEY", # Pass your JWT here
</strong><strong>    forward_headers= [ "authorization" ]
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

For further questions on custom Azure deployments or fine-grained access tokens, reach out to us on support@portkey.ai

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

[^1]: Azure Parameter

[^2]: Azure Parameter

[^3]: Azure Parameter

[^4]: Azure Parameter
