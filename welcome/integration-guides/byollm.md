# Bring Your Own LLM

Portkey provides a robust and secure platform to observe, integrate, and manage your **locally or privately hosted custom models.**

## Integrating Custom Models with Portkey SDK

You can integrate any custom LLM with Portkey as long as it's API is compliant with any of the **15+** providers Portkey already supports.

### **1. Install the Portkey SDK**

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

### **2. Initialize Portkey with your Custom URL**

Instead of using a `provider` + `Authorization` pair or a `virtualKey` referring to the provider, you can specify a **`provider`** + **`custom_host`** pair while instantiating the Portkey client.

`custom_host` here refers to the URL where your custom model is hosted, including the API version identifier. (e.g., `http://localhost:11434/v1` for Ollama).

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
<strong>    provider: "PROVIDER_NAME", // This can be mistral-ai, openai, or anything else
</strong><strong>    customHost: "http://MODEL_URL/v1/", // Your custom URL with version identifier
</strong><strong>    Authorization: "AUTH_KEY", // If you need to pass auth
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
<strong>    provider="PROVIDER_NAME", # This can be mistral-ai, openai, or anything else
</strong><strong>    custom_host="http://MODEL_URL/v1/", # Your custom URL with version identifier
</strong><strong>    Authorization="AUTH_KEY", # If you need to pass auth
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

More on `custom_host` [here](../../product/ai-gateway-streamline-llm-integrations/universal-api.md#integrating-local-or-private-models).

### **3. Invoke Chat Completions**

Use the Portkey SDK to invoke chat completions from your model, just as you would with any other provider.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }]
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }]
)

print(completion)
```
{% endtab %}
{% endtabs %}

## Forward Sensitive Headers Securely

When integrating custom LLMs with Portkey, you may have sensitive information in your request headers that you don't want Portkey to track or log. Portkey provides a secure way to forward specific headers directly to your model's API without any processing.

Just specify an array of header names using the **`forward_headers`** property when initializing the Portkey client. Portkey will then forward these headers directly to your custom host URL without logging or tracking them.

Here's an example:

{% tabs %}
{% tab title="Node" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    provider: "PROVIDER_NAME", // This can be mistral-ai, openai, or anything else
    customHost: "http://MODEL_URL/v1/", // Your custom URL with version identifier
    Authorization: "AUTH_KEY", // If you need to pass auth
<strong>    forwardHeaders: [ "Authorization" ]
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    provider="PROVIDER_NAME", # This can be mistral-ai, openai, or anything else
    custom_host="http://MODEL_URL/v1/", # Your custom URL with version identifier
    Authorization="AUTH_KEY", # If you need to pass auth
<strong>    forward_headers= [ "Authorization" ]
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

You can also configure `forward_headers` in your Config object:

<pre class="language-python"><code class="lang-python">config = {
		"provider": "mistral-ai",
		"custom_host": "http://MODEL_URL/v1/",
		"access_token": "xxx",
		"key_id": "xxx",
		"pod_id": "xxx",
<strong>		"forward_headers": [
</strong><strong>				"key_id",
</strong><strong>				"pod_id",
</strong><strong>				"access_token"
</strong><strong>		]
</strong>}

from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
<strong>    config=config
</strong>)
</code></pre>

## Next Steps

Explore the complete list of features supported in the SDK:

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

***

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your requests](../../product/ai-gateway-streamline-llm-integrations/universal-api.md#ollama-in-configs)
3. [Tracing requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to your local LLM](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
