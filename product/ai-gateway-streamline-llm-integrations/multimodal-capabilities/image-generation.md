# Image Generation

Portkey's AI gateway supports image generation capabilities that many foundational model providers offer. The most common use case is that of **text-to-image** where the user sends a prompt which the image model processes and returns an image.

{% hint style="info" %}
The guide for vision models is [available here](vision.md).
{% endhint %}

### Text-to-Image Usage

Portkey supports the OpenAI signature to make text-to-image requests.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
import Portkey from 'portkey-ai';

// Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",  // Replace with your Portkey API key
    virtualKey: "VIRTUAL_KEY"   // Add your provider's virtual key
});

async function main() {
  const image = await portkey.images.generate({ 
    model: "dall-e-3", 
    prompt: "Lucy in the sky with diamonds" 
  });
  
  console.log(image.data);
}

main();
```
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey
from IPython.display import display, Image

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Add your provider's virtual key
)

image = portkey.images.generate(
  model="dall-e-3",
  prompt="Lucy in the sky with diamonds"
)

# Display the image
display(Image(url=image.data[0].url))
```
{% endtab %}

{% tab title="OpenAI Node.js" %}
```javascript
import OpenAI from 'openai'; // We're using the v4 SDK
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    provider: "openai",
    apiKey: "PORTKEY_API_KEY" // defaults to process.env["PORTKEY_API_KEY"]
  })
});

async function main() {
  const image = await openai.images.generate({ 
    model: "dall-e-3", 
    prompt: "Lucy in the sky with diamonds" 
  });
  
  console.log(image.data);
}

main();
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
from IPython.display import display, Image

client = OpenAI(
    api_key='OPENAI_API_KEY',
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key="PORTKEY_API_KEY"
    )
)

image = client.images.generate(
  model="dall-e-3",
  prompt="Lucy in the sky with diamonds",
  n=1,
  size="1024x1024"
)

# Display the image
display(Image(url=image.data[0].url))
```
{% endtab %}

{% tab title="REST" %}
```bash
curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: openai-virtual-key" \
  -d '{
    "model": "dall-e-3",
    "prompt": "Lucy in the sky with diamonds"
  }'
```
{% endtab %}
{% endtabs %}

#### API Reference

{% content-ref url="../../../endpoints/completions-1.md" %}
[completions-1.md](../../../endpoints/completions-1.md)
{% endcontent-ref %}

On completion, the request will get logged in the logs UI where the image can be viewed.&#x20;

(_Note that providers may remove the hosted image after a period of time, so some logs might only contain the url_)

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

### Supported Providers and Models

The following providers are supported for image generation with more providers getting added soon. Please raise a [request](../../../welcome/integration-guides/suggest-a-new-integration.md) or a [PR](https://github.com/Portkey-AI/gateway/pulls) to add model or provider to the AI gateway.

| Provider                                                            | Models                                                   | Functions                    |
| ------------------------------------------------------------------- | -------------------------------------------------------- | ---------------------------- |
| [OpenAI](../../../welcome/integration-guides/openai.md)             | `dall-e-2`, `dall-e-3`                                   | Create Image (text to image) |
| [Azure OpenAI](../../../welcome/integration-guides/azure-openai.md) | `dall-e-2`, `dall-e-3`                                   | Create Image (text to image) |
| [Stability](../../../welcome/integration-guides/stability-ai.md)    | `stable-diffusion-v1-6`, `stable-diffusion-xl-1024-v1-0` | Create Image (text to image) |
| Replicate (Coming Soon)                                             |                                                          |                              |
| Monster API (Coming Soon)                                           |                                                          |                              |
| Segmind (Coming Soon)                                               |                                                          |                              |
| Together AI (Coming Soon)                                           |                                                          |                              |

### Cookbook

[**Here's a detailed cookbook on image generation using Portkey**](https://github.com/Portkey-AI/portkey-cookbook/blob/main/examples/image-generation.ipynb) which demonstrates the use of multiple providers and routing between them through Configs.
