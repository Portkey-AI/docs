# What is Portkey?

Teams use Portkey to improve the cost, performance, and accuracy of their Gen AI apps.

It takes <2 mins to integrate and with that, it already starts monitoring all of your LLM requests and also makes your app resilient, secure, performant, and more accurate at the same time.

Here's a product walkthrough (3 mins):

{% embed url="https://youtu.be/PgwK5dmvwTk" %}

***

### Integrate in 3 Lines of Code

{% tabs %}
{% tab title="OpenAI Python" %}
<pre class="language-python"><code class="lang-python"># pip install portkey-ai

from openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
client = OpenAI(
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(provider="openai", api_key="PORTKEY_API_KEY")
</strong>)

chat_complete = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
)

print(chat_complete.choices[0].message.content)
</code></pre>
{% endtab %}

{% tab title="OpenAI Node" %}
<pre class="language-javascript"><code class="lang-javascript">// npm i portkey-ai

import OpenAI from 'openai';
<strong>import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'
</strong>
const openai = new OpenAI({
<strong>  baseURL: PORTKEY_GATEWAY_URL,
</strong><strong>  defaultHeaders: createHeaders({provider: "openai", apiKey: "PORTKEY_API_KEY"})
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
{% endtabs %}

{% content-ref url="welcome/make-your-first-request.md" %}
[make-your-first-request.md](welcome/make-your-first-request.md)
{% endcontent-ref %}

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><h3>✨ Product Features</h3></td><td>Jump to the product section to learn more about the Portkey modules and the use cases they solve.</td><td></td><td><a href="broken-reference">Broken link</a></td></tr><tr><td><h3>🚀 Quickstart</h3></td><td>Setting up Portkey takes . Check out how.</td><td></td><td><a href="welcome/make-your-first-request.md">make-your-first-request.md</a></td></tr><tr><td><h3>📔 API Reference</h3></td><td>Head to the API reference and code samples for all Portkey functionality available through REST APIs and SDKs</td><td></td><td><a href="broken-reference">Broken link</a></td></tr><tr><td><h3>🤝 Integrations</h3></td><td>Find the best integration for you with 0+ models across LLM providers and multiple frameworks. </td><td></td><td><a href="welcome/integration-guides/">integration-guides</a></td></tr></tbody></table>

### Languages Supported

<table><thead><tr><th width="171">Language</th><th>Supported Library</th></tr></thead><tbody><tr><td>Javascript</td><td><a href="https://github.com/Portkey-AI/portkey-node-sdk">portkey-node-sdk</a><br><a href="https://github.com/openai/openai-node">openai-node</a></td></tr><tr><td>Python</td><td><a href="https://github.com/Portkey-AI/portkey-python-sdk">portkey-python-sdk</a><br><a href="https://github.com/openai/openai-python">openai-python</a></td></tr><tr><td>Go</td><td><a href="https://github.com/sashabaranov/go-openai">go-openai</a></td></tr><tr><td>Java</td><td><a href="https://github.com/TheoKanning/openai-java">openai-java</a></td></tr><tr><td>Rust</td><td><a href="https://github.com/64bit/async-openai">async-openai</a></td></tr><tr><td>Ruby</td><td><a href="https://github.com/alexrudall/ruby-openai">ruby-openai</a></td></tr></tbody></table>

### AI Providers Supported

<table><thead><tr><th width="169">AI Provider</th><th data-type="select" data-multiple>Status</th></tr></thead><tbody><tr><td><a href="welcome/integration-guides/openai.md">OpenAI</a></td><td></td></tr><tr><td><a href="welcome/integration-guides/anthropic.md">Anthropic</a></td><td></td></tr><tr><td><a href="welcome/integration-guides/azure-openai.md">Azure OpenAI</a></td><td></td></tr><tr><td><a href="welcome/integration-guides/cohere.md">Cohere</a></td><td></td></tr><tr><td><a href="welcome/integration-guides/anyscale-llama2-mistral-zephyr.md">Anyscale</a></td><td></td></tr><tr><td><a href="welcome/integration-guides/google-palm.md">Google Palm</a></td><td></td></tr><tr><td>Google Gemini</td><td></td></tr><tr><td>Together AI</td><td></td></tr><tr><td>Perplexity</td><td></td></tr><tr><td>Mistral</td><td></td></tr><tr><td>AWS Bedrock</td><td></td></tr><tr><td>AzureML</td><td></td></tr><tr><td>BYOLLM</td><td></td></tr></tbody></table>

### Frameworks Supported

<table><thead><tr><th width="172">Framework</th><th data-type="select" data-multiple>Status</th></tr></thead><tbody><tr><td><a href="welcome/integration-guides/langchain-python.md">Langchain</a></td><td></td></tr><tr><td><a href="welcome/integration-guides/llama-index-python.md">Llamaindex</a></td><td></td></tr></tbody></table>
