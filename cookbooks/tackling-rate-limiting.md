# Tackling Rate Limiting

LLMs are **costly** to run. , the providers have to balance serving user requests v/s straining their GPU resources too thin. They generally deal with this by putting _rate limits_ on how many requests a user can send in a minute or in a day.&#x20;

For example, for the text-to-speech model `tts-1-hd` from OpenAI, you can not send **more than 7** requests in minute. Any extra request automatically fails.&#x20;

There are many real-world use cases where it's possilbe to run into rate limits:

<table><thead><tr><th>LLM Provider</th><th width="213">Example Model</th><th>Rate Limits</th></tr></thead><tbody><tr><td><a href="https://platform.openai.com/docs/guides/rate-limits/usage-tiers">OpenAI</a> </td><td>gpt-3.5-turbo</td><td><p>Tier 1: </p><ul><li>10,000 Requests Per Day</li></ul></td></tr><tr><td><a href="https://docs.anthropic.com/claude/reference/errors-and-rate-limits">Anthropic</a></td><td></td><td><p></p><ul><li></li></ul></td></tr><tr><td><a href="https://docs.cohere.com/docs/going-live#trial-key-limitations">Cohere</a></td><td></td><td></td></tr><tr><td><a href="https://www.anyscale.com/endpoints">Anyscale</a></td><td></td><td><p>Endpoints</p><ul><li>30 concurrent requests</li></ul></td></tr><tr><td><a href="https://docs.perplexity.ai/docs/rate-limits">Perplexity AI</a></td><td>KTxYu9wCEMaj</td><td><ul><li></li></ul></td></tr><tr><td><a href="https://docs.together.ai/docs/rate-limits">Together AI</a></td><td></td><td><p>Paid:</p><ul><li></li></ul></td></tr></tbody></table>



```javascript
import Portkey from "portkey-ai";

const portkey = new Portkey({
  apiKey: process.env.PORTKEYAI_API_KEY,
  virtualKey: process.env.OPENAI_VIRTUAL_KEY,
});

 response = await portkey.chat.completions.create({
  model: "gpt-3.5-turbo",
});
console.(response.choices[0].message.content);

```

### Fallback to lternative LLMs

```javascript
import Portkey from "portkey-ai";

const portkey = new Portkey({
   apiKey: process.env.PORTKEY_API_KEY,
   config: JSON.stringify({
      strategy: {
      },
      targets: [{
            virtual_key: "opeanai-virtual-",
         },
         {
         }
   ]
```

* The targe array&#x20;

### &#x20;Load alance mong LLMs&#x20;

```javascript
import Portkey from "portkey-ai";

const portkey = new Portkey({
   apiKey: process.env.PORTKEY_API_KEY,
   config: JSON.stringify({
      strategy: {
         mode: "loadbalance",
      },
      targets: [{
            virtual_key: "",
            weight: ,
         },
         {
            virtual_key: "",
            weight:,
   })
});
```
