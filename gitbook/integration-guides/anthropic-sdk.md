# ➡ Anthropic SDK

In the following code snippets, the `api_base` is changed to Portkey's base path.

The Portkey API key and mode are passed in the request headers. You need to replace `<YOUR PORTKEY API KEY>` with your actual Portkey API key. The mode is set as **`proxy anthropic`** which is the Portkey middleware mode for OpenAI.

{% tabs %}
{% tab title="Python" %}
```python
from anthropic import Anthropic, HUMAN_PROMPT, AI_PROMPT

anthropic_key = "<anthropic-api-key>"
portkey_api = "<portkery-api-key>"
base_url="
https://api.portkey.ai/v1/proxy
"

headers = {
    "x-portkey-api-key": PORTKEY API KEY,
    "x-portkey-mode": "proxy anthropic",
}

anthropic = Anthropic(
    api_key=anthropic_key,
    default_headers=headers,
    base_url=base_url,
)

completion = anthropic.completions.create(
    model="claude-1",
    max_tokens_to_sample=300,
    prompt=f"{HUMAN_PROMPT} how does a court case get to the Supreme Court? {AI_PROMPT}",
)

print(completion.completion)
```
{% endtab %}
{% endtabs %}



Remember, you can leverage more Portkey features by including appropriate headers in your requests. For instance, you can enable request caching by adding the `"x-portkey-cache": "simple"` or `"x-portkey-cache": "semantic"` header, depending on the caching mechanism you want to use.

Likewise, you can enable request retries by adding the `"x-portkey-retry-count": <no_of_retries>` header, and so on. Refer to the Portkey Headers document for more details on enabling/disabling different features using headers.
