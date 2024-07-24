# Jina AI

Portkey provides a robust and secure gateway to facilitate the integration of various models into your applications, including [Jina AI embedding & reranker models](https://jina.ai/).

With Portkey, you can take advantage of features like fast AI gateway access, observability, and more, all while ensuring the secure management of your API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`jina`**</mark>
{% endhint %}

## Portkey SDK Integration with Jina AI Models

Portkey provides a consistent API to interact with models from various providers. To integrate Jina AI with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Jina AI's API through Portkey's gateway.

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

To use JinaAI with Portkey, [get your API key from here](https://jina.ai/), then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
<strong>    virtualKey: "JINA_AI_VIRTUAL_KEY" // Your Jina AI Virtual Key
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
<strong>    virtual_key="JINA_AI_VIRTUAL_KEY"   # Replace with your virtual key for Jina AI
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

### **3. Invoke Embeddings with** Jina AI

Use the Portkey instance to send your embeddings requests to Jina AI. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const embeddings = await portkey.embeddings.create({
    input: "embed this",
<strong>    model: "jina-embeddings-v2-base-es",
</strong>});
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">embeddings = portkey.embeddings.create(
    input='embed this',
<strong>    model='jina-embeddings-v2-base-de'
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

### Using Jina AI Reranker Models

Portkey also supports the Reranker models by Jina AI through the REST API.

{% tabs %}
{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash"><strong>curl https://api.portkey.ai/v1/rerank \
</strong>  -H "Content-Type: application/json" \
<strong>  -H "Authorization: Bearer $JINA_AI_API_KEY" \
</strong><strong>  -H "x-portkey-provider: jina" \
</strong>  -d '{
<strong>  "model": "jina-reranker-v1-base-en",
</strong>  "query": "Organic skincare products for sensitive skin",
  "documents": [
    "Eco-friendly kitchenware for modern homes",
    "Biodegradable cleaning supplies for eco-conscious consumers",
    "Organic cotton baby clothes for sensitive skin"
  ],
  "top_n": 2
}'

</code></pre>
{% endtab %}
{% endtabs %}

## Supported Models

Portkey works with all the embedding & reranker models offered by Jina AI. You can browse the full list of Jina AI models [here](https://jina.ai/embeddings#apiform).

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your J](../../product/ai-gateway-streamline-llm-integrations/configs.md)ina AI[ requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Jina AI requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI Embeddings to Jina AI](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
