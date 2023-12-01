# Universal API

The Portkey API and SDKs are integrated with all the popular LLM providers. This allows you to make a request to any hosted or private model with the same signature.

Portkey uses the fast open-source AI gateway, [Rubeus](https://github.com/Portkey-AI/Rubeus) to handle request and response transformations automatically. Rubeus by default accepts all requests with a similar signature to OpenAI and is tuned to always return an OpenAI compliant response.

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

// Generate a text completion
async function getTextCompletion() {
    const completion = await portkey.completions.create({
        prompt: "Say this is a test",
        model: "gpt-3.5-turbo-instruct",
    });

    console.log(completion);
}
getTextCompletion();
```
{% endtab %}

{% tab title="Python" %}
```python
from portkey import Portkey

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # The OpenAI virtual key
)

# Generate a text completion
def get_text_completion():
    completion = portkey.completions.create(
        prompt='Say this is a test',
        model='gpt-3.5-turbo-instruct'
    )
    print(completion)

get_text_completion()
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

// Generate a text completion
async function getTextCompletion() {
    const completion = await portkey.completions.create({
        prompt: "Say this is a test",
<strong>        model: "claude-2",
</strong>    });

    console.log(completion);
}
getTextCompletion();
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey import Portkey

# Initialize the Portkey client
portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
<strong>    virtual_key="VIRTUAL_KEY"   # The Anthropic virtual key
</strong>)

# Generate a text completion
def get_text_completion():
    completion = portkey.completions.create(
        prompt='Say this is a test',
<strong>        model='claude-2'
</strong>    )
    print(completion)

get_text_completion()
</code></pre>
{% endtab %}
{% endtabs %}

You can also use [config objects](configs.md) for a whole host of gateway strategies.
