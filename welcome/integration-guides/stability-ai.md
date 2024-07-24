# Stability AI

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Stability AI APIs](https://platform.stability.ai/docs/api-reference).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`stability-ai`**</mark>
{% endhint %}

## Portkey SDK Integration with Stability AI

Portkey provides a consistent API to interact with image generation models from various providers. To integrate Stability AI with Portkey:

### 1. Install the Portkey SDK

Add the Portkey SDK to your application to interact with the Stability API through Portkey's gateway.

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

### 2. Initialize Portkey with the Virtual Key

To use Stability AI with Portkey, [get your API key from here](https://platform.stability.ai/account/keys).  Then add it to Portkey to create the virtual key

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Stability AI Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Stability AI
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Image Generation with** Stability AI

Use the Portkey instance to send requests to Stability AI. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const image = await portkey.images.generate({
  model:"stable-diffusion-v1-6",
  prompt:"Lucy in the sky with diamonds",
  size:"1024x1024"
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
image = portkey.images.generate(
  model="stable-diffusion-v1-6",
  prompt="Lucy in the sky with diamonds",
  size="1024x1024"
)
```
{% endtab %}
{% endtabs %}

Notice how we're using the OpenAI's image generation signature to prompt Stability allowing greater flexibility to change models and providers later if necessary.

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Stability AI requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Stability AI's requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Stability](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
5. [Image generation API Reference](../../provider-endpoints/images/create-image.md)
