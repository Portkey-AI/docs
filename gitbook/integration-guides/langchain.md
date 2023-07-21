# ➡ Langchain

**Open AI**

Using Portkey with Langchain is as simple as just choosing which Portkey features you want, enabling them via `headers=Portkey.Config` and passing it in your LLM calls.&#x20;

For more, checkout [Portkey's Langchain documentation](https://python.langchain.com/docs/ecosystem/integrations/portkey/).

{% tabs %}
{% tab title="Python" %}
```python
# Tracing agent calls across different requests

import os
from langchain.llms import OpenAI
import langchain.utilities import Portkey

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
{% endtabs %}
