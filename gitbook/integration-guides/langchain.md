# ➡ Langchain

**Open AI**



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
        "x-portkey-api-key": "PORTKEY_API_KEY",
        "x-portkey-mode": "proxy azure-openai"
    },
    deployment_name="DEPLOYMENT_NAME",
    model_name="MODEL_NAME",
)

llm("Tell me a joke")
```
{% endtab %}
{% endtabs %}



**Cohere**



**Anthropic**
