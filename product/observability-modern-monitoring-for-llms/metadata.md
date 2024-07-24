# Metadata

{% hint style="success" %}
This feature is available on all Portkey plans.
{% endhint %}

You can send custom metadata along with your requests in Portkey, which can later be used for auditing or filtering logs. You can pass **any number** of keys, all values should be of type **`string`** with **max-length** as **128** characters.

### Adding Metadata to Requests

The metadata object accepts a JSON - Portkey will then parse it and let you filter on your metadata.

```python
PORTKEY_METADATA = {
    "_user": "userid123",
    "environment": "production",
    "organisation": "orgid123",
    "prompt": "summarisationPrompt",
    "foo": "abc",
    "bar": "def"
}
```

{% hint style="info" %}
**\_user is a special key**

This key is used to **provide user analytics** in the analytics section of Portkey app.

If you pass the **`user`** key in the OpenAI request body, we will automatically also store it in Portkey's **`_user`** key. \
\
If both the OpenAI request body **`user`** key and the Portkey metadata **`_user`** key are passed, then only the metadata **`_user`** key will be stored.
{% endhint %}

#### Send metadata along with your requests as shown below:

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import {Portkey} from 'portkey-ai'

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "OPENAI_VIRTUAL_KEY"
})

const requestOptions = { 
<strong>    metadata: {
</strong><strong>        "_user": "USER_ID",
</strong><strong>        "organisation": "ORG_ID",
</strong><strong>        "request_id": "1729"
</strong><strong>    }
</strong>}

const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Who was ariadne?' }],
    model: 'gpt-4',
<strong>}, requestOptions);
</strong>
console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="OPENAI_VIRTUAL_KEY"
)

<strong>response = portkey.with_options(
</strong><strong>    metadata = {
</strong><strong>        "_user": "USER_ID",
</strong><strong>        "environment": "production",
</strong><strong>        "prompt": "test_prompt",
</strong><strong>        "session_id": "1729"
</strong><strong>}).chat.completions.create(
</strong>    messages = [{ "role": 'user', "content": 'What is 1729' }],
    model = 'gpt-4'
)

print(response.choices[0].message)
</code></pre>
{% endtab %}

{% tab title="OpenAI NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
<strong>    metadata: {"_user": "USER_ID"}
</strong>  })
});

async function main() {
  const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: 'user', content: 'What is the capital of Uzbekistan?' }],
    model: 'gpt-4',
  });

  console.log(chatCompletion.choices[0].message);
}

main();
</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
<pre class="language-python"><code class="lang-python">from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
<strong>        metadata={"_user": "USER_ID"}
</strong>    )
)

response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
)

print(response.choices[0].message)
</code></pre>
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash">curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: $OPENAI_VIRTUAL_KEY" \
<strong>  -H "x-portkey-metadata: {\"_user\":\"john\"}" \
</strong>  -d '{
    "model": "gpt-4",
    "messages": [{"role": "user","content": "Hello!"}]
  }'
</code></pre>
{% endtab %}
{% endtabs %}

### Using Metadata

#### See metadata summary on the Analytics page:

Just head over to the metadata tab and you should see all of your keys in the filter.

<figure><img src="../../.gitbook/assets/metadata-analytics.gif" alt=""><figcaption></figcaption></figure>

#### Filter requests according to your custom metadata on the Logs page

Select the "Meta" filter and choose the key:value pairs you want to filter the requests on. All the custom keys you have sent with _any_ of your requests should show up here.

<figure><img src="../../.gitbook/assets/metadata-logs.gif" alt=""><figcaption></figcaption></figure>
