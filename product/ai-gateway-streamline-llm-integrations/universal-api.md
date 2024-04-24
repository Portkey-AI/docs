# Universal API

Portkey's Universal API provides a consistent interface to integrate a wide range of modalities (text, vision, audio) and LLMs (hosted OR local) into your apps. So, instead of maintaining separate integrations for different multimodal LLMs, you can interact with models from OpenAI, Anthropic, Meta, Cohere, Mistral, and many more (100+ models, 15+ providers) - all  using a common, unified API signature.

### Portkey Follows OpenAI Spec

Portkey API is powered by its [battle-tested open-source AI Gateway](https://github.com/portkey-ai/gateway), which converts all incoming requests to the OpenAI signature and returns OpenAI-compliant responses.

### Switching Providers is a Breeze

{% tabs %}
{% tab title="Node" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai';

<strong>// Calling OpenAI
</strong>const portkey = new Portkey({
    provider: "openai",
    Authorization: "Bearer sk-xxxxx"
})
const response = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Hello' }],
    model: 'gpt-4',
});

<strong>// Swithing to Anthropic
</strong>const portkey = new Portkey({
    provider: "anthropic",
    Authorization: "Bearer sk-ant-xxxxx"
})
const response = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Hello' }],
    model: 'claude-3-opus-20240229',
});
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

<strong># Calling OpenAI
</strong>portkey = Portkey(
    provider = "openai",
    Authorization = "sk-xxxxx"
)
response = portkey.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Hello' }],
    model = 'gpt-4'
)

<strong># Switching to Anthropic
</strong>portkey = Portkey(
    provider = "anthropic",
    Authorization = "sk-ant-xxxxx"
)
response = portkey.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Hello' }],
    model = 'claude-3-opus-20240229'
)
</code></pre>
{% endtab %}
{% endtabs %}

## Integrating Local or Private Models

Portkey can also route to and observe your locally or privately hosted LLMs, as long as the model is compliant with one of the 15+ providers supported by Portkey.

Simply specify the **`custom_host`** parameter along with the **`provider`** name, and Portkey will handle the communication with your local model.

{% tabs %}
{% tab title="Node" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai';

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
<strong>    provider: "ollama",
</strong><strong>    customHost: "http://localhost:11434/v1/" // Point Portkey to where Ollama's Llama 3 is hosted
</strong>})

async function main(){
    const response = await portkey.chat.completions.create({
        messages: [{ role: 'user', content: '1729' }],
<strong>        model: 'llama3',
</strong>    });
    console.log(response)
}

main()
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
<strong>    provider="ollama",
</strong><strong>    custom_host="http://localhost:11434/v1/" # Point Portkey to where Llama 3 is hosted using Ollama
</strong>)

chat = portkey.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model="llama3"
</strong>)

print(chat)
</code></pre>
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash">curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-provider: ollama" \
</strong><strong>  -H "x-portkey-custom-host: http://localhost:11434/v1/" \
</strong>  -d '{
<strong>    "model": "llama3",
</strong>    "messages": [{ "role": "user", "content": "Say this is a test" }]
  }'
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note:**&#x20;

When using **`custom_host`**, include the version identifier (e.g., **`/v1`**) in the URL. Portkey will append the actual endpoint path (**`/chat/completions`**, **`/completions`**, or **`/embeddings`**) automatically.
{% endhint %}

## Powerful Routing and Fallback Strategies

With Portkey you can implement sophisticated routing and fallback strategies. Route requests to different providers based on various criteria, loadbalance them, set up retries or fallbacks to alternative models in case of failures or resource constraints.

Here's an example config where we set up a fallback from OpenAI to a locally hosted Llama3 on Ollama:

<pre class="language-python"><code class="lang-python">config = {
	"strategy": { "mode": "loadbalance" },
	"targets": [
		{
			"provider": "openai",
			"api_key": "xxx",
			"weight": 1,
			"override_params": { "model": "gpt-3.5-turbo" }
		},
		{
<strong>			"provider": "ollama",
</strong><strong>			"custom_host": "http://localhost:11434/v1/",
</strong>			"weight": 1,
			"override_params": { "model": "llama3" }
		}
	]
}

from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
<strong>    config=config
</strong>)
</code></pre>

## Multimodality

Portkey integrates with multimodal models through the same unified API and supports vision, audio, image generation, and more capabilities across providers.

{% content-ref url="multimodal-capabilities/" %}
[multimodal-capabilities](multimodal-capabilities/)
{% endcontent-ref %}
