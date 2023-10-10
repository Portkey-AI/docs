# 🔧 How to Use Configs

## Rest API

#### To integrate Configs in your REST API calls:

1. Retrieve your **Config ID** from the Portkey dashboard.
2. Along with your Portkey API key, include it in your API calls using the header **`'x-portkey-config'`**.

{% tabs %}
{% tab title="OpenAI Python SDK" %}
```python
# Set Portkey as the base path
openai.api_base = "https://api.portkey.ai/v1/proxy"

response = openai.Completion.create(
  model="text-davinci-003",
  prompt="Translate the following English text to French: '{}'",
  temperature=0.5,
  headers={
    "x-portkey-api-key": "<PORTKEY_API_KEY>",
    "x-portkey-mode": "proxy openai",
    "x-portkey-config": "<CONFIG_ID>" # Pass your Config ID here
  }
)

print(response.choices[0].text.strip())
```
{% endtab %}

{% tab title="Portkey Prompt Templates" %}
```sh
curl -X POST \
  https://api.portkeydev.com/v1/prompts/<prompt_id>/generate \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-config: $CONFIG_ID"
```
{% endtab %}
{% endtabs %}

## Portkey SDK

#### Configs radically simplify how you can use Portkey SDK.&#x20;

You can use the Portkey UI to set your **fallback, load balancing, caching, logging logic** and then just **pass the config id** while constructing your **Portkey client**.

{% tabs %}
{% tab title="Python" %}
```python
import portkey
os.environ["PORTKEY_API_KEY"] = "PORTKEY_API_KEY"

# Directly pass the config id while constructing the Portkey client
portkey.config = portkey.Config(config = "<CONFIG_ID>")

response = portkey.ChatCompletions.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Hello!"}
  ]
)

print(response.choices[0].message)
```
{% endtab %}

{% tab title="Node" %}
```typescript
import { Portkey } from "portkey-ai";

// Directly pass the config id while constructing the Portkey client
const portkey = new Portkey({ config: "<CONFIG_ID>" })

const response = await portkey.chatCompletions.create({
    messages: [
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Hello!"}
    ]
})

console.log(response.choices[0].message)
```
{% endtab %}
{% endtabs %}
