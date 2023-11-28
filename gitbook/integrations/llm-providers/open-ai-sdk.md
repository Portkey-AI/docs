# ➡ Open AI SDK

In the following code snippets, the `base_url` is changed to Portkey's base path.

The Portkey API key and mode are passed in the request headers. You need to replace `<PORTKEY_API_KEY>` with your actual Portkey API key. The mode is set as **`proxy openai`**.

{% tabs %}
{% tab title="Python" %}
```python
from openai import OpenAI

client = OpenAI(
    api_key="OPENAI_API_KEY", # defaults to os.environ.get("OPENAI_API_KEY")
    base_url="https://api.portkey.ai/v1/proxy",
    default_headers= {
        "x-portkey-api-key": "PORTKEY_API_KEY",
        "x-portkey-mode": "proxy openai"
    }
)

chat_complete = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
)

print(chat_complete.choices[0].message.content)
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
import OpenAI from 'openai';

const openai = new OpenAI({
  apiKey: 'OPENAI_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: "https://api.portkey.ai/v1/proxy",
  defaultHeaders:{
    "x-portkey-api-key": "PORTKEY_API_KEY",
    "x-portkey-mode": "proxy openai"
  }
});

async function main() {
  const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
  });

  console.log(chatCompletion.choices);
}

main();
```
{% endtab %}
{% endtabs %}

Each of the above code snippets perform the same operation - generating text based on a prompt using OpenAI's API via Portkey's middleware.

Remember, you can leverage more Portkey features by including appropriate headers in your requests. For instance, you can enable request caching by adding the `"x-portkey-cache": "simple"` or `"x-portkey-cache": "semantic"` header, depending on the caching mechanism you want to use.

Likewise, you can enable request retries by adding the `"x-portkey-retry-count": <no_of_retries>` header, and so on. Refer to the Portkey Headers document for more details on enabling/disabling different features using headers.
