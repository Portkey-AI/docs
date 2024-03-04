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

You can navigate to the Virtual Keys tab of your Portkey account and create a Key for AWS Bedrock that'll make it easier to make calls. You'll need the `AWS Secret Access Key` and `AWS Access Key Id`

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

## Managing AWS Bedrock Prompts

You can manage all prompts to AWS bedrock in the [Prompt Library](../../product/prompt-library.md). All the current models of Anthropic are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

### Supported Models

| Model Name                        | Model String to Use in API calls |
| --------------------------------- | -------------------------------- |
| Amazon Titan Text Express         | `amazon.titan-text-express-v1`   |
| Amazon Titan Lite                 | `amazon.titan-text-lite-v1`      |
| Amazon Titan Embeddings           | `amazon.titan-embed-text-v1`     |
| Anthropic Claude 1                | `anthropic.claude-v1`            |
| Anthropic Claude 2                | `anthropic.claude-v2`            |
| Anthropic Claude 2.1              | `anthropic.claude-v2:1`          |
| Anthropic Claude Instant 1        | `anthropic.claude-instant-v1`    |
| AI21 J2 Mid                       | `ai21.j2-mid-v1`                 |
| AI21 J2 Ultra                     | `ai21.j2-ultra-v1`               |
| Cohere Command                    | `cohere.command-text-v14`        |
| Cohere Command Light              | `cohere.command-light-text-v14`  |
| Cohere Embeddings (English)       | `cohere.embed-english-v3`        |
| Cohere Embeddings (multilingual)  | `cohere.embed-multilingual-v3`   |
| Meta Llama2 13B Chat              | `meta.llama2-13b-chat-v1`        |
| Meta Llama2 70B Chat              | `meta.llama2-70b-chat-v1`        |
| Stability Image Generation models | All                              |

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
