# 🔁 Automatic Retries

Automatically reprocess any unsuccessful API requests **`upto 5`** times. We apply an **`exponential backoff`** strategy, which spaces out retry attempts to prevent network overload.

### How to Use Retries

{% tabs %}
{% tab title="cURL" %}
Add the below header to your API request:

```
'x-portkey-retry-count': '5'
```

Your cURL request with retry count set would look like this:

```bash
curl -L 'https://api.portkey.ai/v1/chatComplete' \
     -H 'x-portkey-api-key: PORTKEY_API_KEY' \
     -H 'x-portkey-retry-count: 5' \
     -H 'Content-Type: application/json' \
     -d '{ 
        "config": { 
            "provider": "openai",
            "apiKey": "OPENAI_API_KEY"
        },
        "params": {
            "messages": [{"role": "user","content":"What are the ten tallest buildings in India?"}],
            "model": "gpt-4"
        }
    }'
```
{% endtab %}

{% tab title="Portkey Python SDK" %}
Set **`retry`** count while constructing the LLM

```python
from portkey import LLMOptions

llm = LLMOptions(
    provider="openai", api_key="OPENAI_API_KEY", 
    retry=5
)
```
{% endtab %}

{% tab title="Portkey Node SDK" %}
Set **`retry`** count while constructing the LLM

```typescript
import { Portkey } from "portkey-ai";

const portkey = new Portkey({
    mode: "single",
    llms: [{
        provider: "openai", virtual_key: "open-ai-xxx",
        retry: 5
    }]
})
```
{% endtab %}
{% endtabs %}
