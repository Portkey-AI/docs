# Google Vertex AI

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Google Vertex AI](https://cloud.google.com/vertex-ai?hl=en).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

## Portkey SDK Integration with Google Vertex AI

Portkey provides a consistent API to interact with models from various providers. To integrate Google Vertex AI with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Google Vertex AI API through Portkey's gateway.

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

Set up Portkey with your virtual key as part of the initialization configuration. You can create a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) for Vertex AI in the UI.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
<strong>    virtualKey: "VERTEX_VIRTUAL_KEY", // Your Vertex AI Virtual Key
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
<strong>    virtual_key="VERTEX_VIRTUAL_KEY"   # Replace with your virtual key for Google
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If you do not want to add your Vertex AI details to Portkey vault, you can directly pass them while instantiating the Portkey client. [More on that here](vertex-ai.md#making-requests-without-virtual-keys).
{% endhint %}

### **3. Invoke Chat Completions with** Vertex AI and Gemini&#x20;

Use the Portkey instance to send requests to Gemini models hosted on Vertex AI. You can also override the virtual key directly in the API call if needed.

{% hint style="warning" %}
Vertex AI uses OAuth2 to authenticate its requests, so you need to send the **access token** additionally along with the request.
{% endhint %}

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gemini-pro',
<strong>}, {authorization: "vertex ai access token here"});
</strong>
console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python"><strong>completion = portkey.with_options(authorization="...").chat.completions.create(
</strong>    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'gemini-pro'
)

print(completion)
</code></pre>
{% endtab %}
{% endtabs %}

## Function Calling

Portkey supports function calling mode on Google's Gemini Models. Explore this :arrow\_down: Cookbook for a deep dive and examples:

{% content-ref url="../../guides/practitioners-cookbooks/quickstarts/function-calling.md" %}
[function-calling.md](../../guides/practitioners-cookbooks/quickstarts/function-calling.md)
{% endcontent-ref %}

## Managing Vertex AI Prompts

You can manage all prompts to Google Gemini in the [Prompt Library](../../product/prompt-library.md). All the models in the model garden are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Making Requests Without Virtual Keys <a href="#making-requests-without-virtual-keys" id="making-requests-without-virtual-keys"></a>

You can also pass your Vertex AI details & secrets directly without using the Virtual Keys in Portkey.

Vertex AI expects a **`region`**, a **`project ID`** and the **`access token`** in the request for a successful completion request. This is how you can specify these fields directly in your requests:

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
<strong>    vertexProjectId: "sample-55646",
</strong><strong>    vertexRegion: "us-central1",
</strong><strong>    provider:"vertex_ai",
</strong><strong>    authorization: "VERTEX_AI_ACCESS_TOKEN"
</strong>})

const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gemini-pro',
});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
<strong>    vertex_project_id="sample-55646",
</strong><strong>    vertex_region="us-central1",
</strong><strong>    provider="vertex_ai",
</strong><strong>    authorization="VERTEX_AI_ACCESS_TOKEN"
</strong>)

completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'gemini-pro'
)

print(completion)
</code></pre>
{% endtab %}

{% tab title="REST" %}
<pre class="language-bash"><code class="lang-bash">curl 'https://api.portkey.ai/v1/chat/completions' \
-H 'Content-Type: application/json' \
-H 'x-portkey-api-key: PORTKEY_API_KEY' \
<strong>-H 'x-portkey-provider: vertex-ai' \
</strong><strong>-H 'Authorization: Bearer VERTEX_AI_ACCESS_TOKEN' \
</strong><strong>-H 'x-portkey-vertex-project-id: sample-94994' \
</strong><strong>-H 'x-portkey-vertex-region: us-central1' \
</strong>--data '{
    "messages": [{"role": "user","content": "Hello"}],
    "max_tokens": 20,
    "model": "gemini-pro"
}'
</code></pre>
{% endtab %}
{% endtabs %}

For further questions on custom Vertex AI deployments or fine-grained access tokens, reach out to us on support@portkey.ai

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Vertex AI requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Vertex AI requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Vertex AI APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
