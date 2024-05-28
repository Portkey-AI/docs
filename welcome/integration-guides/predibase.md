# Predibase

Portkey provides a robust and secure gateway to facilitate the integration of various **open source** as well as **fine-tuned LLMs** into your apps, including your [**models on Predibase**](https://predibase.com/).

With Portkey, you can take advantage of features like fast AI gateway, caching, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

## Portkey SDK Integration with Predibase

Using Portkey, you can call your Predibase models in the familar **OpenAI-spec** and try out your existing pipelines on Predibase fine-tuned models with 2 LOC change.

### **1. Install the Portkey SDK**

{% tabs %}
{% tab title="NodeJS" %}
```bash
npm install --save portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```bash
pip install portkey-ai
```
{% endtab %}
{% endtabs %}

### **2. Initialize Portkey with the Virtual Key**

Set up Portkey with your virtual key as part of the initialization configuration. Grab your [Predibase API key](https://app.predibase.com/), and add it to [Portkey vault](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) to get an associated virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
<strong>    virtualKey: "VIRTUAL_KEY" // Your Predibase Virtual Key
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
<strong>    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Predibase
</strong>)
</code></pre>
{% endtab %}

{% tab title="OpenAI Node SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import OpenAI from "openai";
import { PORTKEY_GATEWAY_URL, createHeaders } from "portkey-ai";

const portkey = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
<strong>    virtualKey: "PREDIBASE_VIRTUAL_KEY",
</strong>  }),
});
</code></pre>
{% endtab %}

{% tab title="OpenAI Python SDK" %}
<pre class="language-python"><code class="lang-python">from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

portkey = OpenAI(
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
<strong>        virtual_key="PREDIBASE_VIRTUAL_KEY"
</strong>    )
)
</code></pre>
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions on Predibase Serverless Endpoints**

Predibase offers LLMs like **Llama 3**, **Mistral**, **Gemma**, etc. on its [serverless infra](https://docs.predibase.com/user-guide/inference/models#serverless-endpoints) that you can query instantly.

{% hint style="info" %}
#### **Passing Predibase Tenand ID**

To make a successful Predibase request, you are required to also pass your **account tenant ID**. With Portkey, you can send [**your Tenand ID**](https://app.predibase.com/settings) with the **`user`** param while making your request.
{% endhint %}

{% tabs %}
{% tab title="Portkey OR OpenAI NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    model: 'llama-3-8b	',
</strong><strong>    user: 'PREDIBASE_TENANT_ID'
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Portkey OR OpenAI Python SDK" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model= 'llama-3-8b',
</strong><strong>    user= "PREDIBASE_TENANT_ID"
</strong>)

print(completion)
</code></pre>
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash"><strong>curl https://api.portkey.ai/v1/chat/completions \
</strong>  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-virtual-key: $PREDIBASE_VIRTUAL_KEY" \
</strong>  -d '{
    "messages": [{"role": "user","content": "Hello!"}],
<strong>    "model": "llama-3-8b",
</strong><strong>    "user": "PREDIBASE_TENANT_ID"
</strong>  }'
</code></pre>
{% endtab %}
{% endtabs %}

### 4. Invoke Predibase Fine-Tuned Models

With Portkey, you can send your Predibase fine-tune adapter details with the `model` param directly while making a request.

{% hint style="info" %}
The format is:

> **`model = <base_model>:<adapter-repo-name/adapter-version-number>`**

So, if your base model is **llama-3-8b**, and adapter repo name is **portkey**, you can pass the following directly in the model param to start querying your fine-tuned model:

> **`model = "llama-3-8b:portkey/1"`**
{% endhint %}

{% tabs %}
{% tab title="Portkey OR OpenAI NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    model: 'llama-3-8b:portkey/1',
</strong><strong>    user: 'PREDIBASE_TENANT_ID'
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Portkey OR OpenAI Python SDK" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model= 'llama-3-8b:portkey/1',
</strong><strong>    user= "PREDIBASE_TENANT_ID"
</strong>)

print(completion)
</code></pre>
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash"><strong>curl https://api.portkey.ai/v1/chat/completions \
</strong>  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-virtual-key: $PREDIBASE_VIRTUAL_KEY" \
</strong>  -d '{
    "messages": [{"role": "user","content": "Hello!"}],
<strong>    "model": "llama-3-8b:portkey/1",
</strong><strong>    "user": "PREDIBASE_TENANT_ID"
</strong>  }'
</code></pre>
{% endtab %}
{% endtabs %}

***

## Routing to Dedicated Deployments

Using Portkey, you can easily route to your dedicatedly deployed models as well, without changing anything!

{% hint style="info" %}
Just pass the dedicated deployment in the `model` param

> **`model = "my-dedicated-mistral-deployment-name"`**
{% endhint %}

### JSON Schema Mode

You can enforce JSON schema for all Predibase models - just set the `response_format` to to `json_object` and pass the relevant schema while making your request. Portkey logs will show your JSON output separately

{% tabs %}
{% tab title="Portkey OR OpenAI NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'llama-3-8b	',
    user: 'PREDIBASE_TENANT_ID',
<strong>    response_format: {
</strong><strong>      "type": "json_object",
</strong><strong>      "schema": {"properties": {
</strong><strong>        "name": {"maxLength": 10, "title": "Name", "type": "string"}, 
</strong><strong>        "age": {"title": "Age", "type": "integer"}, 
</strong><strong>        "required": ["name", "age", "strength"], 
</strong><strong>        "title": "Character", 
</strong><strong>        "type": "object"
</strong><strong>      }
</strong><strong>    }
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Portkey OR OpenAI Python SDK" %}
<pre class="language-python"><code class="lang-python"># Using Pydantic to define the schema
<strong>from pydantic import BaseModel, constr
</strong>
<strong># Define JSON Schema
</strong><strong>class Character(BaseModel):
</strong><strong>    name: constr(max_length=10)
</strong><strong>    age: int
</strong><strong>    strength: int
</strong>
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model= 'llama-3-8b',
</strong><strong>    user= "PREDIBASE_TENANT_ID",
</strong><strong>    response_format={
</strong><strong>        "type": "json_object",
</strong><strong>        "schema": Character.schema(),
</strong><strong>    },
</strong>)

print(completion)
</code></pre>
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash"><strong>curl https://api.portkey.ai/v1/chat/completions \
</strong>  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: $PREDIBASE_VIRTUAL_KEY" \
  -d '{
    "messages": [{"role": "user","content": "Hello!"}],
    "model": "llama-3-8b",
    "user": "PREDIBASE_TENANT_ID",
<strong>    "response_format": {
</strong><strong>      "type": "json_object",
</strong><strong>      "schema": {"properties": {
</strong><strong>        "name": {"maxLength": 10, "title": "Name", "type": "string"}, 
</strong><strong>        "age": {"title": "Age", "type": "integer"}, 
</strong><strong>        "required": ["name", "age", "strength"], 
</strong><strong>        "title": "Character", 
</strong><strong>        "type": "object"
</strong><strong>      }
</strong>    }
  }'
</code></pre>
{% endtab %}
{% endtabs %}

***

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Predibase requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Predibase requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Predibase](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
