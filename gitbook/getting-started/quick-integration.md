# 📎 Quick Integration

### Quick Integration

In this section, we'll show you how to perform a quick integration with Portkey using Middleware Mode. We'll take an example of an OpenAI request, and show you how to route it through Portkey using different methods.

* **OpenAI SDK**

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

* **Langchain**

{% tabs %}
{% tab title="Python" %}
```python
from langchain.llms import OpenAI
import openai

# Set Portkey as the base path
openai.api_base = "https://api.portkey.ai/v1/proxy"

llm = OpenAI(temperature=0, headers={
    "x-portkey-api-key": "<PORTKEY API KEY>",
    "x-portkey-mode": "proxy openai"
}, user="demo_user")

response = llm.generate(prompt="Write an essay on global warming", tokens=500)
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const { OpenAI } = require('langchain');

const openai = new OpenAI({
  apiKey: '<OPENAI API KEY>',
  basePath: 'https://api.portkey.ai/v1/proxy',
  headers: {
    'x-portkey-api-key': '<PORTKEY API KEY>',
    'x-portkey-mode': 'proxy openai'
  }
});

const prompt = 'Write an essay on global warming';
const tokens = 500;

const response = await openai.generate({ prompt, tokens });
```
{% endtab %}
{% endtabs %}

* **Rest API**

{% tabs %}
{% tab title="cURL" %}
```bash
curl --location 'http://192.168.1.91:49875/v1/proxy/completions' \
--header 'x-portkey-api-key: <PORTKEY API KEY>' \
--header 'x-portkey-mode: proxy openai' \
--header 'Authorization: OPENAI API KEY' \
--header 'Content-Type: application/json' \
--data '{
    "n": 1,
    "model": "text-davinci-003",
    "prompt": "Top 20 tallest buildings in the world"
}'
```
{% endtab %}
{% endtabs %}



