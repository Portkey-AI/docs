# Authentication

To ensure secure access to Portkey's APIs, authentication is required for all requests. This guide provides the necessary steps to authenticate your requests using the Portkey API key, regardless of whether you are using the SDKs for Python and JavaScript, the OpenAI SDK, or making REST API calls directly.

## Obtaining Your API Key

Your Portkey API key is the primary credential for authentication. To find it:

1. Log in to your Portkey account.
2. Navigate to the dropdown menu in the profile section.
3. Locate and copy your API key.

## Authentication with SDKs

### Portkey SDKs

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'

const portkey = new Portkey({
<strong>    apiKey: "PORTKEY_API_KEY", // Replace with your actual API key
</strong>    virtualKey: "VIRTUAL_KEY"  // Optional: Use for virtual key management
})

const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey import Portkey

client = Portkey(
<strong>    api_key="PORTKEY_API_KEY",  # Replace with your actual API key
</strong>    virtual_key="VIRTUAL_KEY"   # Optional: Use if virtual keys are set up
)

chat_completion = client.chat.complete.create(
    messages=[{"role": "user", "content": "Say this is a test"}],
    model='gpt-3.5-turbo'
)

print(chat_completion.choices[0].message.content)
</code></pre>
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash">curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
<strong>  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
</strong>  -H "x-portkey-virtual-key: $VIRTUAL_KEY" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [
      { "role": "system", "content": "You are a helpful assistant." },
      { "role": "user", "content": "Hello!" }
    ]
  }'

</code></pre>
{% endtab %}
{% endtabs %}

### OpenAI SDK

When integrating Portkey through the OpenAI SDK, modify the base URL and add the `x-portkey-api-key` header for authentication. Here's an example of how to do it:

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import openai from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai';

openai.api_base = PORTKEY_GATEWAY_URL;

const headers = createHeaders({
    mode: "openai",
<strong>    apiKey: "PORTKEY_API_KEY" // Replace with your actual Portkey API key
</strong>});

// Continue with your OpenAI SDK usage, utilizing the headers for authentication

</code></pre>
{% endtab %}

{% tab title="Python" %}

{% endtab %}
{% endtabs %}

Read more [here](../getting-started/integration-guides/openai.md).

