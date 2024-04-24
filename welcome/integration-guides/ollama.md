# Ollama

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including your **locally hosted models through Ollama**.

## Portkey SDK Integration with Ollama Models

Portkey provides a consistent API to interact with models from various providers. To integrate Ollama with Portkey:

### **1. Install the Portkey SDK**

Install the Portkey SDK in your application to interact with your Ollama localhost API through Portkey.

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

### **2. Initialize Portkey with Ollama URL**

Provide your Ollama localhost URL when instantiating the Portkey client, and you can start sending requests to it through the Portkey Gateway.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
<strong>    provider: "ollama",
</strong><strong>    customHost: "http://localhost:11434/v1/" // Your Ollama localhost URL
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
<strong>    provider="ollama",
</strong><strong>    custom_host="http://localhost:11434/v1/" # Your Ollama localhost URL    
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with** Ollama

Use the Portkey SDK to invoke chat completions from your Ollama model, just as you would with any other provider.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    model: 'llama3',
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model= 'llama3'
</strong>)

print(completion)
</code></pre>
{% endtab %}
{% endtabs %}

## Next Steps

Explore the complete list of features supported in the SDK:

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

***

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Ollama requests](../../product/ai-gateway-streamline-llm-integrations/universal-api.md#ollama-in-configs)
3. [Tracing Ollama requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Ollama APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)

