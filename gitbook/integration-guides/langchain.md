# ➡ Langchain

**Open AI**

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
    "x-portkey-api-key": "<PORTKEY_API_KEY>",
    "x-portkey-mode": "proxy openai"
  }
)

print(response.choices[0].text.strip())
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
