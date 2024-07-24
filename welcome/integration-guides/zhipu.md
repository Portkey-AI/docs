# ZhipuAI / ChatGLM / BigModel

[ZhipuAI](https://open.bigmodel.cn/) has developed the GLM series of open source LLMs that are some of the world's best performing and capable models today. Portkey provides a robust and secure gateway to seamlessly integrate these LLMs into your applications in the familiar OpenAI spec with just 2 LOC change!

With Portkey, you can leverage powerful features like fast AI gateway, caching, observability, prompt management, and more, while securely managing your LLM API keys through a virtual key system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`zhipu`**</mark>
{% endhint %}

## Portkey SDK Integration with ZhipuAI

### **1. Install the Portkey SDK**

Install the Portkey SDK in your project using npm or pip:

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

To use ZhipuAI / ChatGLM / BigModel with Portkey, [get your API key from here](https://bigmodel.cn/usercenter/apikeys), then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
<strong>    virtualKey: "VIRTUAL_KEY" // Your ZhipuAI Virtual Key
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
<strong>    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for ZhipuAI
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
<strong>    virtualKey: "ZHIPUAI_VIRTUAL_KEY",
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
<strong>        virtual_key="ZHIPUAI_VIRTUAL_KEY"
</strong>    )
)
</code></pre>
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions**

{% tabs %}
{% tab title="Portkey OR OpenAI NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Who are you?' }],
<strong>    model: 'glm-4'
</strong>});

console.log(chatCompletion.choices);
</code></pre>

> I am an AI assistant named ZhiPuQingYan（智谱清言）, you can call me Xiaozhi🤖
{% endtab %}

{% tab title="Portkey OR OpenAI Python SDK" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model= 'glm-4'
</strong>)

print(completion)
</code></pre>

> I am an AI assistant named ZhiPuQingYan（智谱清言）, you can call me Xiaozhi🤖
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash"><strong>curl https://api.portkey.ai/v1/chat/completions \
</strong>  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-virtual-key: $ZHIPUAI_VIRTUAL_KEY" \
</strong>  -d '{
    "messages": [{"role": "user","content": "Hello!"}],
<strong>    "model": "glm-4",
</strong>  }'
</code></pre>

> I am an AI assistant named ZhiPuQingYan（智谱清言）, you can call me Xiaozhi🤖
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
2. [Add gateway configs to your ZhipuAI requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing ZhipuAI requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to ZhipuAI](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
