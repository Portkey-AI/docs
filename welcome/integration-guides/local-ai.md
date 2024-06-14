# LocalAI

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including your **locally hosted models through** [**LocalAI**](https://localai.io/).

## Portkey SDK Integration with LocalAI

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

### **2. Initialize Portkey with LocalAI URL**

First, ensure that your API is externally accessible. If you're running the API on `http://localhost`, consider using a tool like `ngrok` to create a public URL. Then, instantiate the Portkey client by adding your LocalAI URL (along with the version identifier) to the `customHost` property, and add the provider name as `openai`.

{% hint style="success" %}
**Note:** Don't forget to include the version identifier (e.g., `/v1`) in the `customHost` URL
{% endhint %}

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
<strong>    provider: "openai",
</strong><strong>    customHost: "https://7cc4-3-235-157-146.ngrok-free.app/v1" // Your LocalAI ngrok URL
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
<strong>    provider="openai",
</strong><strong>    custom_host="https://7cc4-3-235-157-146.ngrok-free.app/v1" # Your LocalAI ngrok URL    
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Portkey currently supports all endpoints that adhere to the OpenAI specification. This means, you can access and observe any of your LocalAI models that are exposed through OpenAI-compliant routes.&#x20;

[List of supported endpoints here](local-ai.md#localai-endpoints-supported).
{% endhint %}

### **3. Invoke Chat Completions**

Use the Portkey SDK to invoke chat completions from your LocalAI model, just as you would with any other provider.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    model: 'ggml-koala-7b-model-q4_0-r2.bin',
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model= 'ggml-koala-7b-model-q4_0-r2.bin'
</strong>)

print(completion)
</code></pre>
{% endtab %}
{% endtabs %}

## LocalAI Endpoints Supported

<table data-header-hidden><thead><tr><th width="533">Endpoint</th><th>Portkey API Reference</th></tr></thead><tbody><tr><td><code>/chat/completions</code> (Chat, Vision, Tools support)</td><td><a href="../../provider-endpoints/chat.md">Doc</a></td></tr><tr><td><code>/images/generations</code></td><td><a href="../../provider-endpoints/images/create-image.md">Doc</a></td></tr><tr><td><code>/embeddings</code></td><td><a href="../../provider-endpoints/embeddings.md">Doc</a></td></tr><tr><td><code>/audio/transcriptions</code></td><td><a href="../../product/ai-gateway-streamline-llm-integrations/multimodal-capabilities/vision-1.md">Doc</a></td></tr></tbody></table>

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
