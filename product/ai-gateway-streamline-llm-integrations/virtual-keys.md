# Virtual Keys

Portkey's virtual key system allows you to securely store your LLM API keys in our vault, utilizing a unique virtual identifier to streamline API key management.

This feature also provides the following benefits:

* Easier key rotation
* The ability to generate multiple virtual keys for a single API key
* Imposition of restrictions based on cost, request volume, and user access

These can be managed within your account under the "Virtual Keys" tab.

### Creating Virtual Keys:

1. Navigate to the "Virtual Keys" page and click the "Add Key" button in the top right corner.
2. Select your AI provider, name your key uniquely, and note any usage specifics if needed.

{% hint style="info" %}
**Tip:** You can register multiple keys for one provider or use different names for the same key for easy identification.
{% endhint %}

## Using Virtual Keys

### Using the Portkey SDK

Add the virtual key directly to the initialization configuration for Portkey.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Portkey supports a vault for your LLM Keys
})
```
{% endtab %}

{% tab title="Python" %}
```python
# Construct a client with a virtual key
const portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="VIRTUAL_KEY"
)
```
{% endtab %}
{% endtabs %}

Alternatively, you can override the virtual key during the completions call as follows:

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
}, {virtualKey: "OVERRIDING_VIRTUAL_KEY"});
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.with_options(virtual_key="...").chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'gpt-3.5-turbo'
})
```
{% endtab %}
{% endtabs %}

### Prompt Templates

Choose your Virtual Key within Portkey’s prompt templates, and it will be automatically retrieved and ready for use.

<figure><img src="https://3798672042-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FeWEp2XRBGxs7C1jgAdk7%2Fuploads%2FN2vbbUsZw8kGe1uf622M%2Fconfig_prompt.gif?alt=media&#x26;token=98b26d64-8af5-4231-9cf0-a23e045b80fd" alt=""><figcaption></figcaption></figure>

### Langchain / LlamaIndex

Set the virtual key when utilizing Portkey's custom LLM as shown below:

```python
# Example in Langchain
llm = PortkeyLLM(api_key="PORTKEY_API_KEY",virtual_key="VIRTUAL_KEY")
```
