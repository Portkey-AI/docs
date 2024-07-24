# Anthropic

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Anthropic's Claude APIs](https://docs.anthropic.com/claude/reference/getting-started-with-the-api).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`anthropic`**</mark>
{% endhint %}

## Portkey SDK Integration with Anthropic

Portkey provides a consistent API to interact with models from various providers. To integrate Anthropic with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Anthropic's API through Portkey's gateway.

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

To use Anthropic with Portkey, [get your Anthropic API key from here](https://console.anthropic.com/settings/keys), then add it to Portkey to create your Anthropic virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Anthropic Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Anthropic
)
```
{% endtab %}

{% tab title="OpenAI Python SDK" %}
<pre class="language-python"><code class="lang-python">from openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
client = OpenAI(
    api_key="ANTHROPIC_API_KEY",
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        api_key="PORTKEY_API_KEY",
</strong><strong>        provider="anthropic"
</strong><strong>    )
</strong>)
</code></pre>
{% endtab %}

{% tab title="OpenAI Node SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import OpenAI from "openai";
<strong>import { PORTKEY_GATEWAY_URL, createHeaders } from "portkey-ai";
</strong>
const client = new OpenAI({
  apiKey: "ANTHROPIC_API_KEY",
<strong>  baseURL: PORTKEY_GATEWAY_URL,
</strong><strong>  defaultHeaders: createHeaders({
</strong><strong>    provider: "anthropic",
</strong><strong>    apiKey: "PORTKEY_API_KEY",
</strong><strong>  }),
</strong>});
</code></pre>
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with Anthropic**

Use the Portkey instance to send requests to Anthropic. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'claude-3-opus-20240229',
    max_tokens: 250 // Required field for Anthropic
});

console.log(chatCompletion.choices[0].message.content);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
chat_completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'claude-3-opus-20240229',
    max_tokens=250 # Required field for Anthropic
)
    
print(chat_completion.choices[0].message.content)
```
{% endtab %}

{% tab title="OpenAI Python SDK" %}
```python
chat_completion = client.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'claude-3-opus-20240229',
    max_tokens = 250
)

print(chat_completion.choices[0].message.content)
```
{% endtab %}

{% tab title="OpenAI Node SDK" %}
```typescript
async function main() {
    const chatCompletion = await client.chat.completions.create({
        model: "claude-3-opus-20240229",
        max_tokens: 1024,
        messages: [{ role: "user", content: "Hello, Claude" }],
    });
    console.log(chatCompletion.choices[0].message.content);
}

main();
```
{% endtab %}
{% endtabs %}

## How to Use Anthropic System Prompt

With Portkey, we make Anthropic models interoperable with the OpenAI schema and SDK methods. So, instead of passing the `system` prompt separately, you can pass it as part of the `messages` body, similar to OpenAI:

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [
<strong>        { role: 'system', content: 'Your system prompt' },
</strong>        { role: 'user', content: 'Say this is a test' }
    ],
<strong>    model: 'claude-3-opus-20240229',
</strong>    max_tokens: 250
});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [
<strong>        { "role": 'system', "content": 'Your system prompt' },
</strong>        { "role": 'user', "content": 'Say this is a test' }
    ],
<strong>    model= 'claude-3-opus-20240229',
</strong>    max_tokens=250 # Required field for Anthropic
)
    
print(completion.choices)
</code></pre>
{% endtab %}
{% endtabs %}

For more, check out the [`chat completions`](../../provider-endpoints/chat.md) and [`completions`](../../provider-endpoints/completions.md) API reference docs.Using Anthropic Vision Models

Portkey's multimodal Gateway fully supports Anthropic's vision models `claude-3-sonnet`, `claude-3-haiku`,  `claude-3-opus`, and the newest `claude-3.5-sonnet`.

For more info, check out this guide:

{% content-ref url="../../product/ai-gateway-streamline-llm-integrations/multimodal-capabilities/vision.md" %}
[vision.md](../../product/ai-gateway-streamline-llm-integrations/multimodal-capabilities/vision.md)
{% endcontent-ref %}

## Managing Anthropic Prompts

You can manage all prompts to Anthropic in the [Prompt Library](../../product/prompt-library.md). All the current models of Anthropic are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Anthropic requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Anthropic requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Anthropic's Claude APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
