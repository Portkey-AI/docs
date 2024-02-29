# Universal API

The Portkey API and SDKs are integrated with all the popular LLM providers. This allows you to make a request to any hosted or private model with the same signature, and we take care of the request and response transformations automatically.

This universal API is powered by Portkey's battle-tested open-source AI Gateway. It converts all incoming requests to the OpenAI signature and is tuned to always return an OpenAI compliant response.

Let's try sending a completion request to OpenAI using Portkey's gateway

{% tabs %}
{% tab title="NodeJS" %}
```javascript
import Portkey from 'portkey-ai';

// Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",  // Replace with your Portkey API key
    virtualKey: "VIRTUAL_KEY"   // The OpenAI virtual key
});

// Function to generate a text completion
async function getTextCompletion() {
    const completion = await portkey.completions.create({
        prompt: "Say this is a test",
        model: "gpt-3.5-turbo-instruct",
    });

    return completion;
}

const response = await getTextCompletion();
console.log(response);
```
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey

# Initialize the Portkey client
portkey = Portkey(
    api_key="/turdjWE+tIUeAzmzGxGEkkJLBQ=",  # Replace with your Portkey API key
    virtual_key="open-ai-key-6317bc"   # The OpenAI virtual key
)

# Generate a text completion
def get_text_completion():
    completion = portkey.completions.create(
        prompt='Say this is a test',
        model='gpt-3.5-turbo-instruct'
    )
    return completion

response = get_text_completion()
print(response)
```
{% endtab %}
{% endtabs %}

When we decide that we just want to try out Anthropic instead, we can just change this to Anthropic's virtual key and update the model.

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai';

// Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",  // Replace with your Portkey API key
<strong>    virtualKey: "VIRTUAL_KEY"   // The Anthropic virtual key
</strong>});

// Function to generate a text completion
async function getTextCompletion() {
    const completion = await portkey.completions.create({
        prompt: "Say this is a test",
<strong>        model: "claude-2",
</strong><strong>        max_tokens: 250 //required parameter for Anthropic
</strong>    });

    return completion;
}

const response = await getTextCompletion();
console.log(response);
</code></pre>
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"# The Anthropic virtual key
)

# Generate a text completion
def get_text_completion():
    completion = portkey.completions.create(
        prompt='Say this is a test',
        model='claude-2',
        max_tokens=250 #required parameter for Anthropic
    )
    return completion

response = get_text_completion()
print(response)
```
{% endtab %}
{% endtabs %}

You can also use [config objects](configs.md) for a whole host of gateway strategies.

The AI Gateway supports APIs outside plain text and chat completions as well. Check out the guides on [Image Generation](capabilities/image-generation.md), [Function Calling](capabilities/function-calling.md), [Using Vision APIs ](capabilities/vision.md)and [Working with Audio](capabilities/audio.md).
