# ➡ Anthropic SDK

In the following code snippets, the `base_url` is changed to Portkey's base path.

The Portkey API key and mode are passed in the request headers. You need to replace `<PORTKEY_API_KEY>` with your actual Portkey API key. The mode is set as **`proxy anthropic`** which is the Portkey middleware mode for Anthropic.

{% tabs %}
{% tab title="Python" %}
```python
from anthropic import Anthropic, HUMAN_PROMPT, AI_PROMPT

headers = {
    "x-portkey-api-key": <PORTKEY_API_KEY>,
    "x-portkey-mode": "proxy anthropic",
}

anthropic = Anthropic(
    api_key=<ANTHROPIC_API_KEY>,
    default_headers=headers,
    base_url="https://api.portkey.ai/v1/proxy",
)

completion = anthropic.completions.create(
    model="claude-2",
    max_tokens_to_sample=300,
    prompt=f"{HUMAN_PROMPT} how does a court case get to the Supreme Court? {AI_PROMPT}",
)

print(completion.completion)
```
{% endtab %}

{% tab title="NodeJS" %}
```typescript
import Anthropic from '@anthropic-ai/sdk';

const headers = {
    "x-portkey-api-key": <PORTKEY_API_KEY>,
    "x-portkey-mode": "proxy anthropic",
    "x-portkey-cache": "semantic"
};


const anthropic = new Anthropic({
  apiKey: <ANTHROPIC_API_KEY>,
  defaultHeaders: headers,
  baseURL: 'https://api.portkey.ai/v1/proxy'
});


async function main() {
  const completion = await anthropic.completions.create({
    model: 'claude-2',
    max_tokens_to_sample: 300,
    prompt: `${Anthropic.HUMAN_PROMPT} why did the chicken cross the road? ${Anthropic.AI_PROMPT}`,
  });
  console.log(completion);    
}
  
main().catch(console.error);  
```
{% endtab %}
{% endtabs %}

Remember, you can leverage more Portkey features by including appropriate headers in your requests. For instance, you can enable request caching by adding the `"x-portkey-cache": "simple"` or `"x-portkey-cache": "semantic"` header, depending on the caching mechanism you want to use.

Likewise, you can enable request retries by adding the `"x-portkey-retry-count": <no_of_retries>` header, and so on. Refer to the Portkey Headers document for more details on enabling/disabling different features using headers.
