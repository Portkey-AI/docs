# AWS Bedrock

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including models hosted on AWS Bedrock.&#x20;

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) system.

## Portkey SDK Integration with AWS Bedrock

Portkey provides a consistent API to interact with models from various providers. To integrate Bedrock with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Anthropic's API through Portkey's gateway.

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

### **2. Initialize Portkey with the Virtual Key**

Set up Portkey with your virtual key as part of the initialization configuration. You can create a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) for AWS Bedrock in the UI.

You can navigate to the Virtual Keys tab of your Portkey account and create a Key for AWS Bedrock that'll make it easier to make calls. You'll need the `AWS Secret Access Key` and `AWS Access Key Id` to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Bedrock Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Bedrock
)
```
{% endtab %}
{% endtabs %}

#### **Using Virtual Key with AWS STS**

If you're using [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html), you can pass your `aws_session_token` along with the Virtual key:

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Bedrock Virtual Key,
<strong>    aws_session_token: "&#x3C;aws_session_token>"
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Bedrock,
<strong>    aws_session_token="&#x3C;aws_session_token>"
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

#### Not using Virtual Keys?

[Check out this example on how you can directly use your AWS details to make a Bedrock request through Portkey.](aws-bedrock.md#making-requests-without-virtual-keys)

### **3. Invoke Chat Completions with AWS bedrock**&#x20;

Use the Portkey instance to send requests to Anthropic. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'anthropic.claude-v2:1',
    max_tokens: 250 // Required field for Anthropic
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'anthropic.claude-v2:1',
    max_tokens=250 # Required field for Anthropic
)
    
print(completion.choices)
```
{% endtab %}
{% endtabs %}

## Using Vision Models

Portkey's multimodal Gateway fully supports Bedrock's vision models `anthropic.claude-3-sonnet`, `anthropic.claude-3-haiku`, and `anthropic.claude-3-opus`

For more info, check out this guide:

{% content-ref url="../../product/ai-gateway-streamline-llm-integrations/multimodal-capabilities/vision.md" %}
[vision.md](../../product/ai-gateway-streamline-llm-integrations/multimodal-capabilities/vision.md)
{% endcontent-ref %}

## Managing AWS Bedrock Prompts

You can manage all prompts to AWS bedrock in the [Prompt Library](../../product/prompt-library.md). All the current models of Anthropic are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Making Requests without Virtual Keys

If you do not want to add your AWS details to Portkey vault, you can also directly pass them while instantiating the Portkey client.

### Mapping the Bedrock Details

<table><thead><tr><th width="208">Node SDK</th><th width="209">Python SDK</th><th>REST Headers</th></tr></thead><tbody><tr><td>awsAccessKeyId</td><td>aws_access_key_id</td><td>x-portkey-aws-session-token</td></tr><tr><td>awsSecretAccessKey</td><td>aws_secret_access_key</td><td>x-portkey-aws-secret-access-key</td></tr><tr><td>awsRegion</td><td>aws_region</td><td>x-portkey-aws-region</td></tr><tr><td>awsSessionToken</td><td>aws_session_token</td><td>x-portkey-aws-session-token</td></tr></tbody></table>

### Example

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
<strong>    provider: "bedrock",
</strong><strong>    awsAccessKeyId: "AWS_ACCESS_KEY_ID",
</strong><strong>    awsSecretAccessKey: "AWS_SECRET_ACCESS_KEY",
</strong><strong>    awsRegion: "us-east-1",
</strong><strong>    awsSessionToken: "AWS_SESSION_TOKEN"
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

client = Portkey(
    api_key="PORTKEY_API_KEY",
<strong>    provider="bedrock",
</strong><strong>    aws_access_key_id="&#x3C;aws_acces_key_id>",
</strong><strong>    aws_secret_access_key="&#x3C;aws_secret_acces_key>",
</strong><strong>    aws_region="us-east-1",
</strong><strong>    aws_session_token="&#x3C;aws_session_token>"
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}



### Supported Models

<table><thead><tr><th width="322">Model Name</th><th>Model String to Use in API calls</th></tr></thead><tbody><tr><td>Amazon Titan Text Express</td><td><code>amazon.titan-text-express-v1</code></td></tr><tr><td>Amazon Titan Lite</td><td><code>amazon.titan-text-lite-v1</code></td></tr><tr><td>Amazon Titan Embeddings</td><td><code>amazon.titan-embed-text-v1</code></td></tr><tr><td>Anthropic Claude 1</td><td><code>anthropic.claude-v1</code></td></tr><tr><td>Anthropic Claude 2</td><td><code>anthropic.claude-v2</code></td></tr><tr><td>Anthropic Claude 2.1</td><td><code>anthropic.claude-v2:1</code></td></tr><tr><td>Anthropic Claude Instant 1</td><td><code>anthropic.claude-instant-v1</code></td></tr><tr><td>Anthropic Claude 3 Sonnet</td><td><code>anthropic.claude-3-sonnet-20240229-v1:0</code></td></tr><tr><td>AI21 J2 Mid</td><td><code>ai21.j2-mid-v1</code></td></tr><tr><td>AI21 J2 Ultra</td><td><code>ai21.j2-ultra-v1</code></td></tr><tr><td>Cohere Command</td><td><code>cohere.command-text-v14</code></td></tr><tr><td>Cohere Command Light</td><td><code>cohere.command-light-text-v14</code></td></tr><tr><td>Cohere Embeddings (English)</td><td><code>cohere.embed-english-v3</code></td></tr><tr><td>Cohere Embeddings (multilingual)</td><td><code>cohere.embed-multilingual-v3</code></td></tr><tr><td>Meta Llama2 13B Chat</td><td><code>meta.llama2-13b-chat-v1</code></td></tr><tr><td>Meta Llama2 70B Chat</td><td><code>meta.llama2-70b-chat-v1</code></td></tr><tr><td>Stability Image Generation models</td><td>All</td></tr><tr><td>Mistral 7B Instruct</td><td><code>mistral.mistral-7b-instruct-v0:2</code></td></tr><tr><td>Mixtral 8X7B Instruct</td><td><code>mistral.mixtral-8x7b-instruct-v0:1</code></td></tr><tr><td>Mistral Large</td><td><code>mistral.mistral-large-2402-v1:0</code></td></tr></tbody></table>

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Bedrock requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Bedrock requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Beckrock APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
