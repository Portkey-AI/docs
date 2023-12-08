# Portkey's December Migration

> **Date: 8th Dec, 2024**

This December, we're pushing out some exciting new updates to Portkey's **SDKs**, **APIs**, and **Configs**. Key updates include major version upgrades to SDKs (now 1.0), new API endpoints, and enhanced Configs (version 2.0).  \
\
We recommend upgrading to these new versions promptly to take full advantage of their capabilities. While your existing code will continue to work until the deprecation date around Q2 '24, transitioning now ensures you stay ahead of the curve and avoid any future service interruptions. Follow along with this guide!&#x20;

{% hint style="success" %}
**Portkey's SDKs** are upped to _<mark style="color:purple;">**major version 1.0**</mark>_ bringing parity with the new OpenAI SDK structure and adding Portkey production features to it. We are also bringing native Langchain & Llamaindex integrations inside the SDK.\
\
This is a <mark style="background-color:red;">**Breaking Change**</mark> that <mark style="background-color:orange;">**Requires Migration**</mark>.
{% endhint %}

{% hint style="success" %}
**Portkey's APIs** are upgraded with _<mark style="color:purple;">**new endpoints**</mark>_, making it simpler to do `/chat/completions` and `/completions` calls and adding Portkey's production functionalities to them.

\
This is a <mark style="background-color:red;">**Breaking Change**</mark> that <mark style="background-color:orange;">**Requires Migration**</mark>.
{% endhint %}

{% hint style="success" %}
**Configs** are upgraded to _<mark style="color:purple;">**version**</mark>_ _<mark style="color:purple;">**2.0**</mark>_, bringing nested gateway strategies with granular handling. \
\
This is <mark style="background-color:green;">**NOT a Breaking Change**</mark> and we will <mark style="background-color:blue;">**Auto Migrate**</mark> your Configs.
{% endhint %}

***

## Compatibility & Deprecation List

<table><thead><tr><th width="330">List</th><th width="247">Compatibility</th><th>Deprecation Date</th></tr></thead><tbody><tr><td><strong>API (Old)</strong><br><br><code>/v1/proxy</code><br><code>/v1/complete</code><br><code>/v1/chatComplete</code><br><code>/v1/embed</code><br><code>/v1/prompts/ID/generate</code></td><td>✅ SDK (Old)<br>❌ SDK (New)<br>✅ Configs (Old)<br>✅ Configs (New)</td><td>Q2 '24</td></tr><tr><td><strong>API (New)</strong><br><br><code>/v1</code><br><code>/v1/completions</code><br><code>/v1/chat/completions</code><br><code>/v1/embeddings</code><br><code>/v1/prompts/ID/completions</code></td><td>❌ SDK (Old)<br>✅ SDK (New)<br>❌ Configs (Old)<br>✅ Configs (New)</td><td>-</td></tr><tr><td><strong>SDK Version &#x3C; 1 (Old)</strong></td><td>✅ API (Old)<br>❌ API (New)<br>✅ Configs (Old)<br>✅ Configs (new)</td><td>Q2 '24</td></tr><tr><td><strong>SDK Version = 1 (New)</strong></td><td>❌ API (Old)<br>✅ API (New)<br>✅ Configs (Old)<br>✅ Configs (new)</td><td>-</td></tr><tr><td><strong>Configs 1.0 (Old)</strong></td><td><p>✅ API (Old)</p><p>✅ API (New)<br>✅ SDK (Old)<br>✅ SDK (new)</p></td><td>Q2 '24</td></tr><tr><td><strong>Configs 2.0 (New)</strong></td><td><p>✅ API (Old)</p><p>✅ API (New)<br>✅ SDK (Old)<br>✅ SDK (new)</p></td><td>-</td></tr></tbody></table>

***

## Major Version Release of the SDK

### Here's What's New:

1. More extensible with new methods, can be used with many more LLM providers
2. Supports streaming out of the box
3. Follows OpenAI signature completely
4. Native support for Langchain & Llamaindex within the SDK (Python)
5. Support for the Portkey Feedback endpoint
6. Support for Portkey prompt templates
7. Older SDK versions to be deprecated soon

### Here's What's Changed:

{% tabs %}
{% tab title="Portkey Python SDK" %}
**FROM ➡**

<pre class="language-python"><code class="lang-python"><strong>import portkey
</strong><strong>from portkey import Config, LLMOptions
</strong>
<strong>portkey.config = Config(
</strong><strong>    mode="single",
</strong><strong>    llms=LLMOptions(provider="openai", api_key="OPENAI_API_KEY")
</strong><strong>)
</strong>
response = portkey.ChatCompletions.create(
    model="gpt-4", 
    messages=[
        {"role": "user","content": "Hello World!"}
    ]
)
</code></pre>

**TO ⬇**

<pre class="language-python"><code class="lang-python"><strong>from portkey_ai import Portkey
</strong>
<strong>portkey = Portkey(
</strong><strong>    api_key="PORTKEY_API_KEY",
</strong><strong>    virtual_key="VIRTUAL_KEY"
</strong><strong>)
</strong>
<strong>response = portkey.chat.completions.create(
</strong>    messages=[{'role': 'user', 'content': 'Say this is a test'}],
    model='gpt-3.5-turbo'
)

print(response)
</code></pre>

**Installing the  New SDK,**

```bash
pip install -U portkey_ai
```
{% endtab %}

{% tab title="Portkey Node SDK" %}
**FROM ➡**

<pre class="language-javascript"><code class="lang-javascript"><strong>import { Portkey } from "portkey-ai";
</strong>
<strong>const portkey = new Portkey({
</strong><strong>    mode: "single",
</strong><strong>    llms: [{ provider: "openai", virtual_key: "open-ai-xxx" }]
</strong><strong>});
</strong>
async function main() {
    const chatCompletion = await portkey.chat.completions.create({
        messages: [{ role: 'user', content: 'Say this is a test' }],
        model: 'gpt-4'
    });

    console.log(chatCompletion.choices);
};

main();
</code></pre>

**TO ⬇**

<pre class="language-javascript"><code class="lang-javascript"><strong>import Portkey from 'portkey-ai';
</strong>
// Initialize the Portkey client
<strong>const portkey = new Portkey({
</strong><strong>    apiKey: "PORTKEY_API_KEY",  // Replace with your Portkey API key
</strong><strong>    virtualKey: "VIRTUAL_KEY"   // Optional: For virtual key management
</strong><strong>});
</strong>
// Generate a chat completion
async function getChatCompletion() {
    const chatCompletion = await portkey.chat.completions.create({
        messages: [{ role: 'user', content: 'Say this is a test' }],
        model: 'gpt-3.5-turbo',
    });

    console.log(chatCompletion);
}
getChatCompletion();
</code></pre>

**Installing the  New SDK:**

```bash
npm i -U portkey-ai
```
{% endtab %}
{% endtabs %}

{% content-ref url="../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

***

## All-New APIs

### Here's What's New:

1. Introduced 3 new routes `/chat/completions`, `/completions`, and `/embeddings`&#x20;
2. Simplified the headers:
   1. `x-portkey-mode` header is deprecated and replaced with `x-portkey-provider` which takes values of `openai`, `anyscale`, `cohere,` `palm`, `azure-openai`, and more.
   2. New header `x-portkey-virtual-key` is introduced.
3. `/complete` and `/chatComplete` endpoints to be deprecated soon
4. Prompts endpoint `/prompts/$PROMPT_ID/generate` is upgraded to `/prompts/$PROMPT_ID/completions` and the old route will be deprecated soon
5. New `/gateway` endpoint that lets you make calls to third-party LLM providers easily

### Here's What's Changed

{% tabs %}
{% tab title="OpenAI Python SDK" %}
**FROM** ➡

<pre class="language-python"><code class="lang-python">from openai import OpenAI

client = OpenAI(
    api_key="OPENAI_API_KEY", # defaults to os.environ.get("OPENAI_API_KEY")
<strong>    base_url="https://api.portkey.ai/v1/proxy",
</strong><strong>    default_headers= {
</strong><strong>        "x-portkey-api-key": "PORTKEY_API_KEY",
</strong><strong>        "x-portkey-mode": "proxy openai",
</strong><strong>        "Content-Type": "application/json"
</strong><strong>    }
</strong>)

chat_complete = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
)

print(chat_complete.choices[0].message.content)
</code></pre>

**TO ⬇**

<pre class="language-python"><code class="lang-python">from openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
client = OpenAI(
    api_key="OPENAI_API_KEY", # defaults to os.environ.get("OPENAI_API_KEY")
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        provider="openai",
</strong><strong>        api_key="PORTKEY_API_KEY" # defaults to os.environ.get("PORTKEY_API_KEY")
</strong><strong>    )
</strong>)

chat_complete = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
)

print(chat_complete.choices[0].message.content)
</code></pre>
{% endtab %}

{% tab title="OpenAI Node SDK" %}
**FROM** ➡

<pre class="language-javascript"><code class="lang-javascript">import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
<strong>  baseURL: "https://api.portkey.ai/v1/proxy",
</strong><strong>  defaultHeaders:{
</strong><strong>    "x-portkey-api-key": "PORTKEY_API_KEY",
</strong><strong>    "x-portkey-mode": "proxy openai",
</strong><strong>    "Content-Type": "application/json"
</strong><strong>  }
</strong>});

async function main() {
  const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
  });

  console.log(chatCompletion.choices);
}

main();
</code></pre>

**TO ⬇**

<pre class="language-javascript"><code class="lang-javascript">import OpenAI from 'openai'; // We're using the v4 SDK
<strong>import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'
</strong>
const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
<strong>  baseURL: PORTKEY_GATEWAY_URL,
</strong><strong>  defaultHeaders: createHeaders({
</strong><strong>    provider: "openai",
</strong><strong>    apiKey: "PORTKEY_API_KEY" // defaults to process.env["PORTKEY_API_KEY"]
</strong><strong>  })
</strong>});

async function main() {
  const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
  });

  console.log(chatCompletion.choices);
}

main();
</code></pre>
{% endtab %}

{% tab title="REST API" %}
**FROM ➡**

<pre class="language-bash"><code class="lang-bash"><strong>curl http://api.portkey.ai/v1/proxy/completions \
</strong><strong>  -H 'x-portkey-api-key: $PORTKEY_API_KEY' \
</strong><strong>  -H 'x-portkey-mode: proxy openai' \
</strong>  -H 'Authorization: Bearer &#x3C;OPENAI_API_KEY>' \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "gpt-3.5-turbo-instruct",
    "prompt": "Top 20 tallest buildings in the world"
  }'
</code></pre>

**TO ⬇**

<pre class="language-bash"><code class="lang-bash"><strong>curl https://api.portkey.ai/v1/completions \
</strong><strong>  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
</strong><strong>  -H "x-portkey-provider: openai" \ 
</strong>  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-3.5-turbo-instruct",
    "prompt": "Top 20 tallest buildings in the world"
  }'
</code></pre>
{% endtab %}
{% endtabs %}

{% content-ref url="../api-reference/chat-completions.md" %}
[chat-completions.md](../api-reference/chat-completions.md)
{% endcontent-ref %}

{% content-ref url="../api-reference/completions.md" %}
[completions.md](../api-reference/completions.md)
{% endcontent-ref %}

### Simlarly, for Prompts

{% tabs %}
{% tab title="REST API" %}
**FROM ➡**

<pre class="language-bash"><code class="lang-bash"><strong>curl https://api.portkey.ai/v1/prompts/$PROMPT_ID/generate \
</strong><strong>  -H 'x-portkey-api-key: $PORTKEY_API_KEY' \
</strong>  -H 'Content-Type: application/json' \
  -d '{"variables": {"variable_a": "", "variable_b": ""}}'
</code></pre>

**TO ⬇**

<pre class="language-bash"><code class="lang-bash"><strong>curl https://api.portkey.ai/v1/prompts/$PROMPT_ID/completions \
</strong><strong>  -H 'x-portkey-api-key: $PORTKEY_API_KEY' \
</strong>  -H 'Content-Type: application/json' \
  -d '{"variables": {"variable_a": "", "variable_b": ""}}'
</code></pre>
{% endtab %}
{% endtabs %}

{% content-ref url="../api-reference/prompts/" %}
[prompts](../api-reference/prompts/)
{% endcontent-ref %}

***

## Configs 2.0

### Here's What's New

1. New concept of `strategy` instead of standalone `mode`. You can now build bespoke gateway strategies and nest them in a single config.&#x20;
2. You can also trigger a specific strategy on specific error codes.
3. New concept of `targets` that replace `options` in the previous Config&#x20;
4. If you are adding `virtual_key` to the target array, you don't need to add `provider` any longer. Portkey will pick up the Provider directly from the Virtual Key!
5. For Azure, only now pass the `virtual_key` - it takes care of all other Azure params like Deployment name, API version etc.

{% hint style="info" %}
The Configs UI on Portkey app will auto write Configs ONLY in the new format. All your existing Configs are auto migrated.
{% endhint %}

### Here's What's Changed

{% tabs %}
{% tab title="Config Builder" %}
**FROM ➡**

```
{
	"mode": "single",
	"options": [
		{
			"provider": "openai",
			"virtual_key": "open-4110dd",
		}
	]
}
```

**TO ⬇**

```
{
	"strategy": {
		"mode":"single"
	},
	"targets": [
		{
			"virtual_key": "open-4110dd"
		}
	]
}
```
{% endtab %}
{% endtabs %}

{% content-ref url="../api-reference/config-object.md" %}
[config-object.md](../api-reference/config-object.md)
{% endcontent-ref %}

***

## Support

Shoot ANY questions or queries you have on the migration to the Portkey team [**on our Discord**](https://discord.gg/yn6QtVZJgV) and we should try to get back to you ASAP.
