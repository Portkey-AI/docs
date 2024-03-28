# Tackling Rate Limiting

LLMs are **costly** to run. As their usage increaases, the providers have to balance serving user requests v/s straining their GPU resources too thin. They generally deal with this by putting _rate limits_ on how many requests a user can send in a minute or in a day.&#x20;

For example, for the text-to-speech model `tts-1-hd` from OpenAI, you can not send **more than 7** requests in minute. Any extra request automatically fails.&#x20;

There are many real-world use cases where it's possilbe to run into rate limits:

* When your requests have very high input-tokens count or a very long context, you can hit token thresholds
* When you are running a complex and long prompts pipeline that fires hundreds of requests at once, you can hit both token & request limits

### Here's an overview of rate limits imposed by various providers:

<table><thead><tr><th>LLM Provider</th><th width="213">Example Model</th><th>Rate Limits</th></tr></thead><tbody><tr><td><a href="https://platform.openai.com/docs/guides/rate-limits/usage-tiers">OpenAI</a> </td><td>gpt-4</td><td><p>Tier 1:</p><ul><li>500 Requests per Minute</li><li>10,000 Tokens per Minute</li><li>10,000 Requests per Day</li></ul></td></tr><tr><td><a href="https://docs.anthropic.com/claude/reference/errors-and-rate-limits">Anthropic</a></td><td>All models</td><td><p>Tier 1:</p><ul><li>50 RPM</li><li>50,000 TPM</li><li>1 Million Tokens per Day</li></ul></td></tr><tr><td><a href="https://docs.cohere.com/docs/going-live#trial-key-limitations">Cohere</a></td><td>Co.Generate models</td><td><p>Production Key:</p><ul><li>10,000 RPM</li></ul></td></tr><tr><td><a href="https://www.anyscale.com/endpoints">Anyscale</a></td><td>All models</td><td><p>Endpoints:</p><ul><li>30 concurrent requests</li></ul></td></tr><tr><td><a href="https://docs.perplexity.ai/docs/rate-limits">Perplexity AI</a></td><td>mixtral-8x7b-instruct</td><td><ul><li>24 RPM</li><li>16,000 TPM</li></ul></td></tr><tr><td><a href="https://docs.together.ai/docs/rate-limits">Together AI</a></td><td>All models</td><td><p>Paid:</p><ul><li>100 RPM</li></ul></td></tr></tbody></table>

Generally, developers tackle getting rate limiting with a few tricks: caching common responses, queuing the requests, reducing the number of requests sent, etc.

**Using Portkey,** you can solve this by just adding a few lines of code to your app once.&#x20;

In this cookbook, we'll show you **2 ways of ensuring** that your app **never gets rate limited** again:

## 1. Install Portkey SDK

```bash
npm i portkey-ai
```

Let's start by making a generic `chat.completions` call using the Portkey SDK:

```javascript
import Portkey from "portkey-ai";

const portkey = new Portkey({
  apiKey: process.env.PORTKEYAI_API_KEY,
  virtualKey: process.env.OPENAI_VIRTUAL_KEY,
});

response = await portkey.chat.completions.create({
  messages: [ {role: "user", content: "Hello!"} ],
  model: "gpt-4",
});
console.log(response.choices[0].message.content);
```

To ensure your reuqest doesn't get rate limited, we'll utilise Portkey's **fallback** & **loadbalance** features:

## 2. Fallback to Alternative LLMs

With Portkey, you can write a call routing strategy that helps you fallback from one provider to another provider in case of rate limit errors. This is done by passing a Config object while instantiating your Portkey client:

```javascript
import Portkey from "portkey-ai";

const portkey = new Portkey({
   apiKey: process.env.PORTKEY_API_KEY,
   config: JSON.stringify({
      strategy: {
         mode: "fallback",
         on_status_codes:[429]
      },
      targets: [
         {
            provider: "opeanai",
            api_key: "OPENAI_API_KEY"
         },
         {
            provider: "anthropic",
            api_key: "ANTHROPIC_API_KEY",
            override_params: { "max_tokens":254 }
         }
      ],
   }),
});
```

#### **In this Config object,**

* The routing `strategy` is set as `fallback`
* `on_status_codes` param ensures that the fallback is only triggered on the `429` error code, which is generated for rate limit errors
* `targets` array contains the details of the LLMs and the order of the fallback
* The `override_params` in the second target lets you add more params for the specific provider. (`max_tokens` for Anthropic in this case)

That's it! The rest of your code remains the same. Based on the Config, Portkey will do the orchestration and ensure that Anthropic is called as a fallback option whenever you get rate limit errors on OpenAI.

Fallback is still reactive - it only gets triggered once you actually get an error. Portkey also lets you tackle rate limiting proactively:

## 3. Load Balance Among Multiple LLMs

Instead of sending all your requests to a single provider on a single account, you can split your traffic across multiple provider accounts using Portkey - this ensures that a single account does not get overburdened with requests and thus avoids rate limits. It is very easy to setup this "loadbalancing" using Portkey - just write the relevant loadbalance Config and pass it while instantiating your Portkey client once:

```javascript
import Portkey from "portkey-ai";

const portkey = new Portkey({
   apiKey: process.env.PORTKEY_API_KEY,
   config: JSON.stringify({
      strategy: {
         mode: "loadbalance",
      },
      targets: [
         {
            provider: "openai", api_key: "OPENAI_KEY_1",
            weight: 1,
         },
         {
            provider: "openai", api_key: "OPENAI_KEY_2",
            weight: 1,
         },
         {
            provider: "openai", api_key: "OPENAI_KEY_3",
            weight: 1,
         },         
      ],
   }),
});

```

#### **In this Config object:**

* The routing `strategy` is set as `loadbalance`
* `targets` contain 3 different OpenAI API keys from 3 different accounts, all with equal weight - which means Portkey will split the traffic 1/3rd equally among the 3 keys

With Loadbalance on different accounts, you can effectively multiply your rate limits and make your app much more robust.

## That's it!&#x20;

You can read more on [fallbacks here](../product/ai-gateway-streamline-llm-integrations/fallbacks.md), and on [loadbalance here](../product/ai-gateway-streamline-llm-integrations/load-balancing.md). If you want a deep dive on how Configs work on Portkey, [check out the docs here](../product/ai-gateway-streamline-llm-integrations/configs.md).
