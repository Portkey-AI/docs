# AWS Bedrock

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including models hosted on AWS Bedrock.&#x20;

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`bedrock`**</mark>
{% endhint %}

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

To integrate AWS Bedrock with Portkey, you need the `AWS Secret Access Key`, `AWS Access Key Id`, and `AWS Region` with which you can create your Virtual key on Portkey.

[Here's a guide on where to find your AWS credentials on AWS](aws-bedrock.md#how-to-find-your-aws-credentials).

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

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash">curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: bedrock" \
<strong>  -H "x-portkey-aws-session-token: $AWS_SESSION_TOKEN" \
</strong><strong>  -H "x-portkey-aws-secret-access-key: $AWS_SECRET_ACCESS_KEY" \
</strong><strong>  -H "x-portkey-aws-region: $AWS_REGION" \
</strong><strong>  -H "x-portkey-aws-session-token: $AWS_TOKEN" \
</strong>  -d '{
    "model": "gpt-4o",
    "messages": [{"role": "user","content": "Hello!"}]
  }'
</code></pre>
{% endtab %}
{% endtabs %}

***

## Supported Models

{% embed url="https://docs.aws.amazon.com/bedrock/latest/userguide/model-ids.html#model-ids-arns" %}

***

## How to Find Your AWS Credentials

[Navigate here in the AWS Management Console](https://us-east-1.console.aws.amazon.com/iam/home#/security\_credentials) to obtain your **AWS Access Key ID** and **AWS** **Secret Access Key.**&#x20;

* In the console, you'll find the '**Access keys'** section. Click on '**Create access key**'.&#x20;
* Copy the `Secret Access Key` once it is generated, and you can view the `Access Key ID` along with it.&#x20;

<figure><img src="../../.gitbook/assets/Portkey Group (5).png" alt=""><figcaption></figcaption></figure>

* On the same [page](https://us-east-1.console.aws.amazon.com/iam/home#/security\_credentials) under the '**Access keys'** section, where you created your Secret Access key, you will also find your **Access Key ID.**&#x20;

<figure><img src="../../.gitbook/assets/Portkey Group (6).png" alt=""><figcaption></figcaption></figure>

* And lastly, get Your **`AWS Region`** from the Home Page of[ AWS Bedrock](https://us-east-1.console.aws.amazon.com/bedrock/home?region=us-east-1#/overview) as shown in the image below.

<figure><img src="../../.gitbook/assets/Portkey Group (3).png" alt=""><figcaption></figcaption></figure>

***

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
