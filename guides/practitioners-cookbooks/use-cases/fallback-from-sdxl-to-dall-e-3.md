# Fallback from SDXL to Dall-e-3

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Portkey-AI/portkey-cookbook/blob/main/ai-gateway/set-up-fallback-from-stable-diffusion-to-dall-e.ipynb)

## Set up Fallback from Stable Diffusion to Dall-E

Generative AI models have revolutionized text generation and opened up new possibilities for developers. What next? A new category of image generation models.

This cookbook introduces Portkey’s multimodal AI gateway, which helps you switch between multiple image generation models without any code changes — all with OpenAI SDK. You will learn to set up fallbacks from Stable Diffusion to Dall-E.

### 1. Integrate Image Gen Models with Portkey

Begin by storing API keys in the Portkey Vault.

To save your OpenAI and StabilityAI keys in the Portkey Vault:

1. Go to **portkey.ai**
2. Click **Virtual Keys** and then **Create**
   1. Enter **Name** and **API Key**,
   2. Hit **Create**
3. Copy the virtual key from the **KEY** column

We successfully have set up virtual keys!

For more information, refer the [docs](https://portkey.ai/docs/product/ai-gateway-streamline-llm-integrations/virtual-keys).

The multi-modal AI gateway will use these virtual keys in the future to apply a fallback mechanism to every request from your app.

### 2. Making a call to Stability AI using OpenAI SDK

With Portkey, you can call Stability AI models like SDXL right from inside the OpenAI SDK. Just change the `base_url` to Portkey Gateway and add `defaultHeaders` while instantiating your OpenAI client, and you're good to go

Import the `openai` and `portkey_ai` libraries to send the requests, whereas the rest of the utility libraries will help decode the base64 response and print them onto Jupyter Notebook.

```python
from IPython.display import display
from PIL import Image
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
import requests, io, base64, json
```

```python
PORTKEY_API_KEY="YOUR_PORTKEY_API_KEY_HERE"
OPENAI_VIRTUAL_KEY="YOUR_OPENAI_VIRTUAL_KEY_HERE"
CONFIG_ID="YOUR_CONFIG_ID_HERE"
OPENAI_API_KEY="REDUNDANT"
```

Declare the arguments to pass to the parameters of OpenAI SDK and initialize a client instance.

```python
STABILITYAI_VIRTUAL_KEY="YOUR_STABILITYAI_VIRTUAL_KEY_HERE"

client = OpenAI(
    api_key="REDUNDANT",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="stabilityai",
        api_key=PORTKEY_API_KEY,
        virtual_key=STABILITYAI_VIRTUAL_KEY,
    )
)
```

The `api_key` parameter is passed a random string since it’s redundant as the request will be handled through Portkey.

To generate an image:

```python
image = client.images.generate(
  model="stable-diffusion-v1-6",
  prompt="Kraken in the milkyway galaxy",
  n=1,
  size="1024x1024",
  response_format="b64_json"
)

base64_image = image.data[0].b64_json

image_data = base64.b64decode(base64_image)

image = Image.open(io.BytesIO(image_data))
display(image)
```

The image you receive in the response is encoded in base64 format, which requires you to decode it before you can view it in the Jupyter Notebook. In addition, Portkey offers logging for observability. To find all the information for every request, simply check the requests on the **Dashboard > Logs**.

### 3. Now, Setup a Fallback from SDXL to Dall-E

Let’s learn how to enhance the reliability of your Stability AI requests by configuring automatic fallbacks to Dall-E in case of failures. You can use Gateway Configs on Portkey to implement this automated fallback logic. These configurations can be passed while creating your OpenAI client.

From the Portkey Dashboard, open **Configs** and then click **Create**. In the config editor, write the JSON for Gateway Configs:

```json
{
  "strategy": {
    "mode": "fallback"
  },
  "targets": [
    {
      "virtual_key": "stability-ai-virtualkey",
      "override_params": {
        "model": "stable-diffusion-v1-6"
      }
    },
    {
      "virtual_key": "open-ai-virtual-key",
      "override_params": {
        "model": "dall-e-3"
      }
    }
  ]
}
```

These configs tell the AI gateway to follow an `fallback` strategy, where the primary target to forward requests to is _Stability AI_ (automatically inferred from the virtual key) and then to _OpenAI_. The `override_params` let’s you override the default models for the provider. Finally, surprise surprise! — we also enabled caching with just one more key-value pair.

Learn about [Gateway Configs](https://portkey.ai/docs/product/ai-gateway-streamline-llm-integrations/configs) and [Caching](https://portkey.ai/docs/product/ai-gateway-streamline-llm-integrations/cache-simple-and-semantic) from the docs.

Hit **Save Config** on the top right corner and grab the \*\*Config ID. \*\*Next up, we are going to use the \_Config ID \_in our requests to activate fallback mechanism.

### 4. Make a request with gateway configs

Finally, the requests will be sent like we did with OpenAI SDK earlier, but with one specific difference - the `config` parameter. The request is sent through Portkey and uses saved gateway configs as references to activate the fallback mechanism.

```python
client = OpenAI(
    api_key=OPENAI_API_KEY,
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key=PORTKEY_API_KEY,
        config=CONFIG_ID
    )
)

image = client.images.generate(
  model="stable-diffusion-v1-6",
  prompt="Harry Potter travelling the world using Portkey",
  n=1,
  size="1024x1024",
  response_format="b64_json"
)

base64_image = image.data[0].b64_json

image_data = base64.b64decode(base64_image)

image = Image.open(io.BytesIO(image_data))
display(image)
```

### Afterthoughts

All the requests that go through Portkey will appear in the Logs page within the Portkey Dashboard. You can apply filters or even trace the specific set of requests. Check out [Request Tracing](https://portkey.ai/docs/product/observability-modern-monitoring-for-llms/traces). Simultaneously, a fallback icon is turned on for the log where the fallback is activated.

Portkey supports multiple providers offering multimodal capabilities, such as OpenAI, Anthropic, and Stability AI, all accessible through a unified API interface following OpenAI Signature.

For further exploration, why not [play with Vision capabilities](https://portkey.ai/docs/product/ai-gateway-streamline-llm-integrations/multimodal-capabilities/vision)?
