# ➡ Langchain

**Open AI**

Using Portkey with Langchain is as simple as just choosing which Portkey features you want, enabling them via `headers=Portkey.Config` and passing it in your LLM calls.

{% tabs %}
{% tab title="Python" %}
```python
# Tracing agent calls across different requests

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

**Check out the list of config keys you can set with `Portkey.Config` on Langchain:**

<table><thead><tr><th width="191">Feature</th><th width="225">Config Key</th><th width="129">Value (Type)</th><th>Required/Optional</th></tr></thead><tbody><tr><td>API Key</td><td><code>api_key</code></td><td>API Key (<code>string</code>)</td><td>✅ Required</td></tr><tr><td><a href="https://docs.portkey.ai/key-features/request-tracing">Tracing Requests</a></td><td><code>trace_id</code></td><td>Custom <code>string</code></td><td>❔ Optional</td></tr><tr><td><a href="https://docs.portkey.ai/key-features/automatic-retries">Automatic Retries</a></td><td><code>retry_count</code></td><td><code>integer</code> [1,2,3,4,5]</td><td>❔ Optional</td></tr><tr><td><a href="https://docs.portkey.ai/key-features/request-caching">Enabling Cache</a></td><td><code>cache</code></td><td><code>simple</code> OR <code>semantic</code></td><td>❔ Optional</td></tr><tr><td>Cache Force Refresh</td><td><code>cache_force_refresh</code></td><td><code>True</code></td><td>❔ Optional</td></tr><tr><td>Set Cache Expiry</td><td><code>cache_age</code></td><td><code>integer</code> (in seconds)</td><td>❔ Optional</td></tr><tr><td><a href="https://docs.portkey.ai/key-features/custom-metadata">Add User</a></td><td><code>user</code></td><td><code>string</code></td><td>❔ Optional</td></tr><tr><td><a href="https://docs.portkey.ai/key-features/custom-metadata">Add Organisation</a></td><td><code>organisation</code></td><td><code>string</code></td><td>❔ Optional</td></tr><tr><td><a href="https://docs.portkey.ai/key-features/custom-metadata">Add Environment</a></td><td><code>environment</code></td><td><code>string</code></td><td>❔ Optional</td></tr><tr><td><a href="https://docs.portkey.ai/key-features/custom-metadata">Add Prompt (version/id/string)</a></td><td><code>prompt</code></td><td><code>string</code></td><td>❔ Optional</td></tr></tbody></table>

**Azure Open AI**

{% tabs %}
{% tab title="Python" %}
```python
import os

os.environ["OPENAI_API_TYPE"] = "azure"
os.environ["OPENAI_API_VERSION"] = "2023-03-15-preview"
os.environ["OPENAI_API_BASE"] = "https://api.portkey.ai/v1/proxy/RESOURCE_NAME.openai.azure.com/"
os.environ["OPENAI_API_KEY"] = "AZURE_API_KEY"

from langchain.llms import AzureOpenAI

llm = AzureOpenAI(
    headers = {
        "x-portkey-api-key": "<PORTKEY_API_KEY>",
        "x-portkey-mode": "proxy azure-openai"
    },
    deployment_name="DEPLOYMENT_NAME",
    model_name="MODEL_NAME",
)

llm("Tell me a joke")
```
{% endtab %}

{% tab title="NodeJS" %}
```typescript
import { ChatOpenAI } from "langchain/chat_models/openai";

const model = new ChatOpenAI({
  azureOpenAIApiKey: process.env.AZURE_OPENAI_API_KEY
  azureOpenAIApiVersion: process.env.AZURE_OPENAI_API_VERSION
  azureOpenAIApiInstanceName: process.env.AZURE_OPENAI_API_INSTANCE_NAME
  azureOpenAIApiDeploymentName: process.env.AZURE_OPENAI_API_DEPLOYMENT_NAME
  azureOpenAIBasePath: "https://api.portkey.ai/v1/proxy/${process.env.AZURE_OPENAI_API_INSTANCE_NAME}.openai.azure.com/openai/deployments",
},
{
    baseOptions: {
        headers: {
            "x-portkey-api-key": "<PORTKEY_API_KEY>",
            "x-portkey-mode": "proxy azure-openai"
        },
      },
}
);

async function main() {
  const message = await model.invoke("Tell me a joke");
  console.log(message);
}

main();
```
{% endtab %}
{% endtabs %}

For more, checkout [Portkey's Langchain documentation for Python](https://python.langchain.com/docs/integrations/providers/portkey).
