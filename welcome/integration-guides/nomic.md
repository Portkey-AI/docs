# Nomic

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Nomic](https://docs.nomic.ai/reference/getting-started/).

Nomic has especially become popular due to it's superior embeddings and is now available through Portkey's AI gateway as well.

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`nomic`**</mark>
{% endhint %}

## Portkey SDK Integration with Nomic

Portkey provides a consistent API to interact with embedding models from various providers. To integrate Nomic with Portkey:

### 1. Create a Virtual Key for Nomic in your Portkey account

You can head over to the virtual keys tab and create one for Nomic. This will be then used to make API requests to Nomic without needing the protected API key. [Grab your Nomic API key from here](https://atlas.nomic.ai/data/randomesid/org/keys).

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption><p>Create a virtual key for Nomic in Portkey</p></figcaption></figure>

### 2. Install the Portkey SDK and Initialize with this Virtual Key

Add the Portkey SDK to your application to interact with Nomic's API through Portkey's gateway.&#x20;

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Nomic Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Nomic
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke the Embeddings API with Nomic**

Use the Portkey instance to send requests to your Nomic API. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const embeddings = await portkey.embeddings.create({
    input: "create vector representation on this sentence",
    model: "nomic-embed-text-v1.5",
});

console.log(embeddings);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
embeddings = portkey.embeddings.create(
    input='create vector representation on this sentence',
    model='nomic-embed-text-v1.5'
)
print(embeddings)
```
{% endtab %}
{% endtabs %}

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [API Reference for Embeddings](../../provider-endpoints/embeddings.md)
2. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
3. [Add gateway configs to your Nomic requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
4. [Tracing Nomic requests](../../product/observability-modern-monitoring-for-llms/traces.md)
