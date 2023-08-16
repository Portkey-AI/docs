---
description: Let's connect your LLM providers to Portkey, fast.
---

# 📎 Quick Integration

You can integrate Portkey to your LLM provider via REST APIs, the provider's SDKs and even via Langchain. We'll take an example of an OpenAI request, and show you how to route it through Portkey using different methods. ([Click here](broken-reference) for integration examples with other providers like Anthropic, Cohere etc.)



### **Rest API**

{% tabs %}
{% tab title="cURL - Chat Completion" %}
```powershell
curl --location 'https://api.portkey.ai/v1/complete' \
    --header 'x-portkey-api-key: PORTKEY_API_KEY' \
    --header 'Content-Type: application/json' \
    --data '{ 
        "config": { 
            provider: "openai",
            apiKey: "OPENAI_API_KEY", # Needs apiKey or providerKey
            providerKey: "PROVIDER_KEY"
        }, 
        "params": {
            "messages": [{"role": "user","content":"What are the ten tallest buildings in India?"}],
            "max_tokens": 100,
            "user": "jbu3470",
            "model": "gpt-3.5-turbo"
        }
    }'
```
{% endtab %}

{% tab title="cURL - Completion" %}
{% code overflow="wrap" %}
```powershell
curl --location 'https://api.portkey.ai/v1/complete' \
    --header 'x-portkey-api-key: PORTKEY_API_KEY' \
    --header 'Content-Type: application/json' \
    --data '{ 
        "config": { 
            provider: "openai",
            apiKey: "OPENAI_API_KEY", # Needs apiKey or providerKey
            providerKey: "PROVIDER_KEY"
        }, 
        "params": { 
            "prompt": "What are the top 10 tallest buildings in Bhutan?",                 
            "max_tokens": 50, 
            "model": "text-davinci-003", 
            "user": "jbu749" 
        } 
    }'
```
{% endcode %}
{% endtab %}

{% tab title="curl - Proxy" %}
```powershell
curl --location 'http://api.portkey.ai/v1/proxy/completions' \
    --header 'x-portkey-api-key: <PORTKEY_API_KEY>' \
    --header 'x-portkey-mode: proxy openai' \
    --header 'Authorization: <OPENAI_API_KEY>' \
    --header 'Content-Type: application/json' \
    --data '{
        "n": 1,
        "model": "text-davinci-003",
        "prompt": "Top 20 tallest buildings in the world"
    }'
```
{% endtab %}
{% endtabs %}

In the above cURL requests replace&#x20;

1. `PORTKEY_API_KEY` with Portkey's API Key (Found in your account dashboard)
2. `OPENAI_API_KEY` with your OpenAI API key (Optional, not recommended)
3. `PROVIDER_KEY` with the [AI provider's key](https://app.portkey.ai/organisation/472d2804-d054-4226-b4ae-9d4e2e61e69e/settings/ai-providers) created through Portkey (Optional, recommended)

### **OpenAI SDK**

{% tabs %}
{% tab title="Python" %}
<pre class="language-python"><code class="lang-python"><strong>import openai
</strong>
# Set Portkey as the base path
openai.api_base = "https://api.portkey.ai/v1/proxy"

response = openai.Completion.create(
  model="text-davinci-003",
  prompt="Translate the following English text to French: '{}'",
  temperature=0.5,
  headers={
    "x-portkey-api-key": "&#x3C;PORTKEY_API_KEY>",
    "x-portkey-mode": "proxy openai"
  }
)

print(response.choices[0].text.strip())
</code></pre>
{% endtab %}

{% tab title="NodeJS" %}
```javascript
const { Configuration, OpenAIApi } = require("openai");
const configuration = new Configuration({
  apiKey: "<OPENAI_API_KEY>",
  basePath: "https://api.portkey.ai/v1/proxy",
    baseOptions: {
      headers: {
        "x-portkey-api-key": "<PORTKEY_API_KEY>",
        "x-portkey-mode": "proxy openai"
      }
    }
});

const openai = new OpenAIApi(configuration);

async function generateCompletion() {
  const response = await openai.createCompletion({
    model: "text-davinci-003",
    prompt: "Two roads diverged in the yellow woods",
    max_tokens: 512,
    temperature: 0,
  });

  console.log(response.data.choices[0].text);
}

generateCompletion();
```
{% endtab %}
{% endtabs %}

### **Langchain**

{% tabs %}
{% tab title="Python" %}
```python
import os
from langchain.llms import OpenAI
from langchain.utilities import Portkey

os.environ["OPENAI_API_KEY"] = "<OPENAI_API_KEY>"
PORTKEY_API_KEY = "<PORTKEY_API_KEY>" # Set Portkey API key here
TRACE_ID = "portkey_langchain_demo"  # Set trace id here

# Since Portkey is integrated with Langchain, Portkey.Config() takes care of defining headers

headers = Portkey.Config(
    api_key=PORTKEY_API_KEY,
    trace_id=TRACE_ID,
)

# Now, let's pass these headers

llm = OpenAI(headers=headers)

tools = load_tools(["serpapi", "llm-math"], llm=llm)
agent = initialize_agent(
    tools, llm, agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, verbose=True
)

# Let's test it out!
agent.run(
    "What was the high temperature in SF yesterday in Fahrenheit? What is that number raised to the .023 power?"
)
```
{% endtab %}

{% tab title="NodeJS" %}
```typescript
import { OpenAI } from "langchain/llms/openai";

const model = new OpenAI({
  modelName: "text-davinci-003", 
  temperature: 0.9,
  openAIApiKey: "<OPENAI_API_KEY>",
  configuration: {
    basePath: "https://api.portkey.ai/v1/proxy",
    baseOptions: {
      headers: {
        'x-portkey-api-key': '<PORTKEY_API_KEY>',
        'x-portkey-mode': 'proxy openai',
        'x-portkey-trace-id' : 'langchain_demo'
      }
    }
  }
});

async function main() {
  const res = await model.call("Describe the world as written by Herodotus.");
  console.log(res);
}
main();
```
{% endtab %}
{% endtabs %}



