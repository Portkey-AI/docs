# ➡ Open AI SDK

In the following code snippets, the `api_base` is changed to Portkey's base path.

The Portkey API key and mode are passed in the request headers. You need to replace `<YOUR PORTKEY API KEY>` with your actual Portkey API key. The mode is set as **`proxy openai`** which is the Portkey middleware mode for OpenAI.

{% tabs %}
{% tab title="Python" %}
```python
import openai

# Set Portkey as the base path
openai.api_base = "https://api.portkey.ai/v1/proxy"

response = openai.Completion.create(
  model="text-davinci-003",
  prompt="Translate the following English text to French: '{}'",
  temperature=0.5,
  headers={
    "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
    "x-portkey-mode": "proxy openai"
  }
)

print(response.choices[0].text.strip())
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const { OpenAIAPI } = require('openai');
const configuration = new Configuration({
    organization: "YOUR_ORG_ID",
    apiKey: "<OPENAI_API_KEY>",
    basePath: "https://api.portkey.ai/v1/proxy",
    baseOptions: {
      headers: {
        "x-portkey-api-key": "<YOUR PORTKEY API KEY>",
        "x-portkey-mode": "proxy openai"
      }
    }
});

const openai = new OpenAIAPI(configuration);

const prompt = "Translate the following English text to French: '{}'";

async function getCompletion() {
    const completion = await openai.complete({
        model: "text-davinci-003",
        prompt: prompt,
        temperature: 0.5,
    });
    console.log(completion.data.choices[0].text);
}

getCompletion();
```
{% endtab %}
{% endtabs %}

Each of the above code snippets perform the same operation - generating text based on a prompt using OpenAI's API via Portkey's middleware.

Remember, you can leverage more Portkey features by including appropriate headers in your requests. For instance, you can enable request caching by adding the `"x-portkey-cache": "simple"` or `"x-portkey-cache": "semantic"` header, depending on the caching mechanism you want to use.

Likewise, you can enable request retries by adding the `"x-portkey-retry-count": <no_of_retries>` header, and so on. Refer to the Portkey Headers document for more details on enabling/disabling different features using headers.
