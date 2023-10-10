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

{% content-ref url="configs/how-to-use-configs.md" %}
[how-to-use-configs.md](configs/how-to-use-configs.md)
{% endcontent-ref %}
