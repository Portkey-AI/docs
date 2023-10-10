# 📝 Configs

Configs streamline the management and setting of **all** Portkey features, enabling you to programmatically control various aspects like fallbacks, load balancing, retries, caching, and more - all through a single interface. Access and adjust these settings via the Portkey UI and invoke them in your LLM code through the REST API. (SDK support is coming soon.)

## **Setting Configs**

Navigate to the ‘Configs’ page in the Portkey app to set and manage your Configs. Start typing, and you will see type hints and suggestions for the keys.

<figure><img src="../../.gitbook/assets/config_1 (1).gif" alt=""><figcaption></figcaption></figure>

#### There, you can set the following features:

| Key     | Expected Value                                  |
| ------- | ----------------------------------------------- |
| retry   | Integer \[0,5]                                  |
| cache   | "simple", "semantic"                            |
| mode    | "single", "fallback", "loadbalance", "ab\_test" |
| options | Array of LLM options                            |

* **`retry`** - Set the automatic retry count. Minimum retry count is 0, and maximum is 5.
* **`cache`** - Enable caching. Portkey provides two types of caches - simple & semantic. (More on that [here](request-caching.md))
* **`mode`** - Set the orchestrattion method for your reuqests.&#x20;
  * **"fallback"**: Enable model fallback for primary failure scenarios.
  * **"loadbalance"**: Distribute your request load among multiple providers or accounts.
  * **"ab\_test"**: Run A/B tests programmatically on various model parameters.
  * **"single"**: Opt for standard orchestration.
* **`options`** - Required when the mode is other than "single", allowing you to define model logic for fallback, load balance, or A/B test.

#### In **`options`**, you can set,

| Key              | Expected Value                                                                                                   |
| ---------------- | ---------------------------------------------------------------------------------------------------------------- |
| provider         | "anthropic", "openai", "cohere", "anyscale", etc                                                                 |
| api\_key         | Your provider API key                                                                                            |
| virtual\_key     | Virtual key for any provider from your Portkey dashboard                                                         |
| weight           | Required in case of "loadbalance" or "ab\_test" mode to set the weight for that particular LLM. Should sum to 1. |
| override\_params | To set model params like temperature, top\_p, max\_tokens, model name, etc. Expects a JSON                       |

* **`override_params`** accepts JSON, letting you override model parameters set during completion calls, crucial for optimizing outputs when orchestrating between distinct models like Claude-2 & GPT-3.5.

#### In **`override_params`** you can set,

| Key                                                               | Expected Value                                                                   |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| model                                                             | Model name based on the provider, like "gpt-3.5-turbo", "gpt-4", "claude-2", etc |
| All other model params like top\_p, temperature, max\_tokens, etc | Refer to the provider spec for more info on the params                           |
| messages                                                          | Messages array input for chat completion models                                  |
| prompt                                                            | Text string input for completion models                                          |

***

## Example Config

Here's an example Config putting everything together, that sets fallback between gpt 3.5 & gpt 4:

```json
{
	"mode": "fallback",
	"options": [
		{
			"provider": "openai",
			"virtual_key": "open-ai-key-aaa",
			"override_params": {
					"model": "gpt-3.5-turbo"
			}
		},
		{
			"provider": "openai",
			"virtual_key": "open-ai-key-aaa",
			"override_params": {
					"model": "gpt-4"
			}
		}
	]
}
```

***

## **Using Configs**

### Rest API

To integrate Configs in your REST API calls:

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

### Portkey SDK

Configs radically simplifies how you can use Portkey SDK.&#x20;

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
