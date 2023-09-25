# 🌈 How to Use

### First, obtain **your Portkey API Key:**

Log into [Portkey here](https://app.portkey.ai/), then click on the profile icon on top left and “Copy API Key”.

## There are 3 ways of integrating Portkey in your app:&#x20;

1. Using the official Portkey **SDK**
2. Using our **Rest API**
3. By leveraging Portkey's **Integrations** with projects like Langhchain, Llamaindex and providers like OpenAI, Anthropic, etc.

### 1️⃣ Official [SDK](../sdk/) (recommended)

```python
# Install the SDK:
# pip install -U portkey-ai

# Doing a simple GPT-4 call:

import os
import portkey
from portkey import Config, LLMOptions

os.environ["PORTKEY_API_KEY"] = "YOUR_PORTKEY_API_KEY"

# Construct the Portkey Config
llm = LLMOptions(provider="openai", api_key="YOUR_OPENAI_API_KEY")
portkey.config = Config(mode="single", llms=[llm])

# Your Prompt
messages = [{
    "role": "user", 
    "content": "What is the meaning of life, universe and everything?"
}]

response = portkey.ChatCompletions.create(model="gpt-4", messages=messages)

print(response)
```

Here are detailed docs on the Portkey SDK:

{% content-ref url="../sdk/" %}
[sdk](../sdk/)
{% endcontent-ref %}

### 2️⃣ [Rest API](../integrations/rest-api/)

```bash
curl --location 'https://api.portkey.ai/v1/proxy' \
    --header 'x-portkey-api-key: PORTKEY_API_KEY' \
    --header 'Content-Type: application/json' \
    --data '{ 
        "config": { 
            provider: "openai",
            apiKey: "OPENAI_API_KEY",
        },
        "params": {
            "messages": [{"role": "user","content":"What are the ten tallest buildings in India?"}],
            "model": "gpt-4"
        }
    }'
```

Here are detailed docs on the API:

{% content-ref url="../integrations/rest-api/" %}
[rest-api](../integrations/rest-api/)
{% endcontent-ref %}

### 3️⃣ [Integrations](../integrations/)

Select your integration method and follow the instructions :point\_down:

{% content-ref url="../integrations/llms/open-ai-sdk.md" %}
[open-ai-sdk.md](../integrations/llms/open-ai-sdk.md)
{% endcontent-ref %}

{% content-ref url="../integrations/llms/anthropic-sdk.md" %}
[anthropic-sdk.md](../integrations/llms/anthropic-sdk.md)
{% endcontent-ref %}

{% content-ref url="../integrations/llamaindex/" %}
[llamaindex](../integrations/llamaindex/)
{% endcontent-ref %}

{% content-ref url="../integrations/langchain.md" %}
[langchain.md](../integrations/langchain.md)
{% endcontent-ref %}
