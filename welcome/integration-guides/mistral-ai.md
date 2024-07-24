# Mistral AI

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Mistral AI APIs](https://docs.mistral.ai/api/).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`mistral-ai`**</mark>
{% endhint %}

## Portkey SDK Integration with Mistral AI Models

Portkey provides a consistent API to interact with models from various providers. To integrate Mistral AI with Portkey:

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

To use Mistral AI with Portkey, [get your API key from here](https://console.mistral.ai/api-keys/), then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Mistral AI Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Mistral AI
)
```
{% endtab %}
{% endtabs %}

### **3.1. Invoke Chat Completions with** Mistral AI

Use the Portkey instance to send requests to Mistral AI. You can also override the virtual key directly in the API call if needed.

{% hint style="info" %}
You can also call the new Codestral model here!
{% endhint %}

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'codestral-latest',
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'codestral-latest'
)

print(completion)
```
{% endtab %}
{% endtabs %}

***

## Invoke Codestral Endpoint

Using Portkey, you can also call Mistral API's new Codestral endpoint. Just pass the Codestral URL `https://codestral.mistral.ai/v1` with the `customHost` property.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "MISTRAL_VIRTUAL_KEY",
<strong>    customHost: "https://codestral.mistral.ai/v1"
</strong>})

const codeCompletion = await portkey.chat.completions.create({
<strong>    model: "codestral-latest",
</strong>    messages: [{"role": "user", "content": "Write a minimalist Python code to validate the proof for the special number 1729"}]
});

console.log(codeCompletion.choices[0].message.content);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY", 
    virtual_key="MISTRAL_VIRTUAL_KEY",
<strong>    custom_host="https://codestral.mistral.ai/v1"
</strong>)

code_completion = portkey.chat.completions.create(
<strong>    model="codestral-latest",
</strong>    messages=[{"role": "user", "content": "Write a minimalist Python code to validate the proof for the special number 1729"}]
)

print(code_completion.choices[0].message.content)
</code></pre>
{% endtab %}
{% endtabs %}

#### Your Codestral requests will show up on Portkey logs with the code snippets rendered beautifully!

<figure><img src="../../.gitbook/assets/CleanShot 2024-05-29 at 23.16.22@2x.png" alt=""><figcaption></figcaption></figure>

## Codestral v/s Mistral API Endpoint

Here's a handy guide for when you might want to make your requests to the Codestral endpoint v/s the original Mistral API endpoint:

<figure><img src="../../.gitbook/assets/CleanShot 2024-05-30 at 08.07.22@2x.png" alt=""><figcaption></figcaption></figure>

[For more, check out Mistral's Code Generation guide here](https://docs.mistral.ai/capabilities/code\_generation/#operation/listModels).

***

## Managing Mistral AI Prompts

You can manage all prompts to Mistral AI in the [Prompt Library](../../product/prompt-library.md). All the current models of Mistral AI are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Mistral AI](../../product/ai-gateway-streamline-llm-integrations/configs.md)[ requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Mistral AI requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Mistral AI APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
