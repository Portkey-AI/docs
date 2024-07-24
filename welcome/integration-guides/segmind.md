# Segmind

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Segmind APIs](https://docs.segmind.com/).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`segmind`**</mark>
{% endhint %}

## Portkey SDK Integration with Segmind

Portkey provides a consistent API to interact with image generation models from various providers. To integrate Segmind with Portkey:

### 1. Install the Portkey SDK

Add the Portkey SDK to your application to interact with the Segmind API through Portkey's gateway.

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

To use Segmind with Portkey, [get your API key from here](https://cloud.segmind.com/console/api-keys), then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Segmind Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Segmind
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Image Generation with** Segmind

Use the Portkey instance to send requests to Stability AI. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const image = await portkey.images.generate({
  model:"sdxl1.0-txt2img",
  prompt:"Lucy in the sky with diamonds",
  size:"1024x1024"
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
image = portkey.images.generate(
  model="sdxl1.0-txt2img",
  prompt="Lucy in the sky with diamonds",
  size="1024x1024"
)
```
{% endtab %}
{% endtabs %}

Notice how we're using the OpenAI's image generation signature to prompt Segmind's hosted serverless endpoints allowing greater flexibility to change models and providers later if necessary.

### Supported Models

The following models are supported, newer models added to Segmind should also be automatically supported.

| Model String               | Model Name            | Extra Keys (if any)                                                            |
| -------------------------- | --------------------- | ------------------------------------------------------------------------------ |
| `sdxl1.0-txt2img`          | SDXL                  |                                                                                |
| `sd1.5-526mix`             | 526 Mix               |                                                                                |
| `sd1.5-allinonepixel`      | All In One Pixel      |                                                                                |
| `sd1.5-disneyB`            | Cartoon               |                                                                                |
| `sd1.5-colorful`           | Colorful              |                                                                                |
| `sd1.5-cuterichstyle`      | Cute Rich Style       |                                                                                |
| `sd1.5-cyberrealistic`     | Cyber Realistic       |                                                                                |
| `sd1.5-deepspacediffusion` | Deep Spaced Diffusion |                                                                                |
| `sd1.5-dreamshaper`        | Dream Shaper          |                                                                                |
| `sd1.5-dvarch`             | Dv Arch               |                                                                                |
| `sd1.5-edgeofrealism`      | Edge of Realism       |                                                                                |
| `sd1.5-epicrealism`        | Epic Realism          |                                                                                |
| `sd1.5-fantassifiedicons`  | Fantassified Icons    |                                                                                |
| `sd1.5-flat2d`             | Flat 2D               |                                                                                |
| `sd1.5-fruitfusion`        | Fruit Fusion          |                                                                                |
| `sd1.5-icbinp`             | Icbinp                |                                                                                |
| `sd1.5-juggernaut`         | Juggernaut            |                                                                                |
| `kandinsky2.2-txt2img`     | Kandinsky             |                                                                                |
| `sd1.5-majicmix`           | Majicmix              |                                                                                |
| `sd1.5-manmarumix`         | Manmarumix            |                                                                                |
| `sd1.5-paragon`            | Paragon               |                                                                                |
| `potraitsd1.5-txt2img`     | Potrait SD            |                                                                                |
| `qrsd1.5-txt2img`          | QR Generator          | `control_scale`, `control_scale`, `control_scale`, `qr_text`, `invert`, `size` |
| `sd1.5-rcnz`               | RCNZ                  |                                                                                |
| `sd1.5-rpg`                | RPG                   |                                                                                |
| `sd1.5-realisticvision`    | Realistic Vision      |                                                                                |
| `sd1.5-reliberate`         | Reliberate            |                                                                                |
| `sd1.5-revanimated`        | Revanimated           |                                                                                |
| `sd1.5-samaritan-3d`       | Samaritan             |                                                                                |
| `sd1.5-scifi`              | SciFi                 |                                                                                |
| `smallsd1.5-txt2img`       | Small SD              |                                                                                |
| `tinysd1.5-txt2img`        | Tiny SD               |                                                                                |

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Segmind requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Segmind's requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Segmind](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
5. [Image generation API Reference](../../provider-endpoints/images/create-image.md)
