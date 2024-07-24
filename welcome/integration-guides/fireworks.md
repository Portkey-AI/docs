# Fireworks

Portkey provides a robust and secure gateway to facilitate the integration of various models into your apps, including [chat](fireworks.md#id-3.-invoke-chat-completions-with-fireworks), [vision](fireworks.md#using-vision-models), [image generation](fireworks.md#using-image-generation-models), and [embedding](fireworks.md#using-embeddings-models) models hosted on the [Fireworks platform](https://fireworks.ai/).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`fireworks-ai`**</mark>
{% endhint %}

## Portkey SDK Integration with Fireworks Models

Portkey provides a consistent API to interact with models from various providers. To integrate Fireworks with Portkey:

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

### **2. Initialize Portkey with the Virtual Key**

To use Fireworks with Portkey, [get your API key from here](https://fireworks.ai/api-keys), then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // Defaults to process.env["PORTKEY_API_KEY"]
<strong>    virtualKey: "FIREWORKS_VIRTUAL_KEY" // Your Virtual Key
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Defaults to os.env("PORTKEY_API_KEY")
<strong>    virtual_key="FIREWORKS_VIRTUAL_KEY"   # Your Virtual Key
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with** Fireworks

You can use the Portkey instance now to send requests to Fireworks API.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    model: 'accounts/fireworks/models/llama-v3-70b-instruct',
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model= 'accounts/fireworks/models/llama-v3-70b-instruct'
</strong>)

print(completion)
</code></pre>
{% endtab %}
{% endtabs %}

Now, let's explore how you can use Portkey to call other models (vision, embedding, image) on the Fireworks API:

### **Using Embeddings Models**

Call any [embedding model hosted on Fireworks](https://readme.fireworks.ai/docs/querying-embeddings-models#list-of-available-models) with the familiar OpenAI embeddings signature:

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript"><strong>const embeddings = await portkey.embeddings.create({
</strong>    input: "create vector representation on this sentence",
<strong>    model: "thenlper/gte-large",
</strong>});

console.log(embeddings);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python"><strong>embeddings = portkey.embeddings.create(
</strong>    input='create vector representation on this sentence',
<strong>    model='thenlper/gte-large'
</strong>)
print(embeddings)
</code></pre>
{% endtab %}
{% endtabs %}

### **Using Vision Models**

Portkey natively supports [vision models hosted on Fireworks](https://readme.fireworks.ai/docs/querying-vision-language-models):

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const completion = await portkey.chat.completions.create(
    messages: [
        { "role": "user", "content": [
            { "type": "text","text": "Can you describe this image?" },
<strong>                { "type": "image_url", "image_url":
</strong><strong>                    { "url": "https://images.unsplash.com/photo-1582538885592-e70a5d7ab3d3?ixlib=rb-4.0.3&#x26;ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&#x26;auto=format&#x26;fit=crop&#x26;w=1770&#x26;q=80" }
</strong><strong>                }
</strong>            ]
        }
    ],
<strong>    model: 'accounts/fireworks/models/firellava-13b'
</strong>)

console.log(completion);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [
        { "role": "user", "content": [
            { "type": "text","text": "Can you describe this image?" },
<strong>                { "type": "image_url", "image_url":
</strong><strong>                    { "url": "https://images.unsplash.com/photo-1582538885592-e70a5d7ab3d3?ixlib=rb-4.0.3&#x26;ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&#x26;auto=format&#x26;fit=crop&#x26;w=1770&#x26;q=80" }
</strong><strong>                }
</strong>            ]
        }
    ],
<strong>    model= 'accounts/fireworks/models/firellava-13b'
</strong>)

print(completion)
</code></pre>
{% endtab %}
{% endtabs %}

### **Using Image Generation Models**

Portkey also supports calling [image generation models hosted on Fireworks](https://readme.fireworks.ai/reference/image\_generationaccountsfireworksmodelsstable-diffusion-xl-1024-v1-0) in the familiar OpenAI signature:

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai';
import fs from 'fs';

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
<strong>    virtualKey: "FIREWORKS_VIRTUAL_KEY"
</strong>});

async function main(){
<strong>    const image = await portkey.images.generate({
</strong><strong>        model: "accounts/fireworks/models/stable-diffusion-xl-1024-v1-0",
</strong><strong>        prompt: "An orange elephant in a purple pond"
</strong><strong>    });
</strong>
    const imageData = image.data[0].b64_json as string;
    fs.writeFileSync("fireworks-image-gen.png", Buffer.from(imageData, 'base64'));
}

main()
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey
import base64
from io import BytesIO
from PIL import Image

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="FIREWORKS_VIRTUAL_KEY"
)

<strong>image = portkey.images.generate(
</strong><strong>  model="accounts/fireworks/models/stable-diffusion-xl-1024-v1-0",
</strong><strong>  prompt="An orange elephant in a purple pond"
</strong><strong>)
</strong>
Image.open(BytesIO(base64.b64decode(image.data[0].b64_json))).save("fireworks-image-gen.png")
</code></pre>
{% endtab %}
{% endtabs %}

***

## Fireworks Grammar Mode

Fireworks lets you define [formal grammars](https://en.wikipedia.org/wiki/Formal\_grammar) to constrain model outputs. You can use it to force the model to generate valid JSON, speak only in emojis, or anything else. ([Originally created by GGML](https://github.com/ggerganov/llama.cpp/tree/master/grammars))

Grammar mode is set with the `response_format` param. Just pass your grammar definition with `{"type": "grammar", "grammar": grammar_definition}`

Let's say you want to classify patient requests into 3 pre-defined classes:

{% tabs %}
{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Defaults to os.env("PORTKEY_API_KEY")
    virtual_key="FIREWORKS_VIRTUAL_KEY"   # Your Virtual Key
)

<strong>patient_classification = """
</strong><strong>root      ::= diagnosis
</strong><strong>diagnosis ::= "flu" | "dengue" | "malaria"
</strong><strong>"""
</strong>
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    response_format={"type": "grammar", "grammar": patient_classification},
</strong>    model= 'accounts/fireworks/models/llama-v3-70b-instruct'
)

print(completion)
</code></pre>
{% endtab %}

{% tab title="NodeJS" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // Defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "FIREWORKS_VIRTUAL_KEY" // Your Virtual Key
})

<strong>const patient_classification = `
</strong><strong>root ::= diagnosis
</strong><strong>diagnosis ::= "flu" | "dengue" | "malaria"
</strong><strong>`;
</strong>
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    response_format: {"type": "grammar", "grammar": patient_classification},
</strong>    model: 'accounts/fireworks/models/llama-v3-70b-instruct',
});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
NOTE: Fireworks Grammer Mode is not supported on Portkey prompts playground
{% endhint %}

[Explore the Fireworks guide for more examples and a deeper dive on Grammer node](https://readme.fireworks.ai/docs/structured-output-grammar-based).

## Fireworks JSON Mode

You can force the model to return (1) **An arbitrary JSON**, or (2) **JSON with given schema** with Fireworks' JSON mode.

{% tabs %}
{% tab title="Python" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Defaults to os.env("PORTKEY_API_KEY")
    virtual_key="FIREWORKS_VIRTUAL_KEY"   # Your Virtual Key
)

class Recipe(BaseModel):
    title: str
    description: str
    steps: List[str]
    
json_response = portkey.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Give me a recipe for making Ramen, in JSON format' }],
    model = 'accounts/fireworks/models/llama-v3-70b-instruct',
    response_format = {
        "type":"json_object",
        "schema": Recipe.schema_json()
    }
)

print(json_response.choices[0].message.content)
```
{% endtab %}

{% tab title="NodeJS" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // Defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "FIREWORKS_VIRTUAL_KEY" // Your Virtual Key
})

asyn function main(){
  const json_response = await portkey.chat.completions.create({
    messages: [{role: "user",content: `Give me a recipe for making Ramen, in JSON format`}],
    model: "accounts/fireworks/models/llama-v3-70b-instruct",
<strong>    response_format: {
</strong><strong>      type: "json_object",
</strong><strong>      schema: {
</strong><strong>        type: "object",
</strong><strong>        properties: {
</strong><strong>          title: { type: "string" },
</strong><strong>          description: { type: "string" },
</strong><strong>          steps: { type: "array" }
</strong><strong>        }
</strong><strong>      }
</strong><strong>    }
</strong>  });
}

console.log(json_response.choices[0].message.content);

main()
</code></pre>
{% endtab %}
{% endtabs %}

[Explore Fireworks docs for JSON mode for more examples](https://readme.fireworks.ai/docs/structured-response-formatting).

## Fireworks Function Calling

Portkey also supports function calling mode on Fireworks. [Explore this cookbook for a deep dive and examples](../../guides/getting-started/function-calling.md).

## Managing Fireworks Prompts

You can manage all Fireworks prompts in the [Prompt Library](../../product/prompt-library.md). All the current 49+ language models available on Fireworks are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your ](../../product/ai-gateway-streamline-llm-integrations/configs.md)[requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Firework APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
