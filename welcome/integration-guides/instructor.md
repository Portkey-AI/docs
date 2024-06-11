# Instructor

**Instructor** is a framework for extracting structured outputs from LLMs, available in [Python](https://python.useinstructor.com/) & [JS](https://instructor-ai.github.io/instructor-js/).

With Portkey, you can confidently take your Instructor pipelines to production and get complete observability over all of your calls + make them reliable - all with a 2 LOC change!

### Integrating Portkey with Instructor

{% tabs %}
{% tab title="Instructor Python - OpenAI" %}
<pre class="language-python"><code class="lang-python">import instructor
from pydantic import BaseModel
from openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
portkey = OpenAI(
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        virtual_key="OPENAI_VIRTUAL_KEY",
</strong><strong>        api_key="PORTKEY_API_KEY"
</strong><strong>    )
</strong>)

class User(BaseModel):
    name: str
    age: int

<strong>client = instructor.from_openai(portkey)
</strong>
user_info = client.chat.completions.create(
    model="gpt-4-turbo",
    max_tokens=1024,
    response_model=User,
    messages=[{"role": "user", "content": "John Doe is 30 years old."}],
)

print(user_info.name)
print(user_info.age)
</code></pre>
{% endtab %}

{% tab title="Instructor JS - OpenAI" %}
<pre class="language-javascript"><code class="lang-javascript">import Instructor from "@instructor-ai/instructor";
import OpenAI from "openai";
import { z } from "zod";
<strong>import { PORTKEY_GATEWAY_URL, createHeaders } from "portkey-ai";
</strong>
const portkey = new OpenAI({
<strong>  baseURL: PORTKEY_GATEWAY_URL,
</strong><strong>  defaultHeaders: createHeaders({
</strong><strong>    apiKey: "PORTKEY_API_KEY",
</strong><strong>    virtualKey: "OPENAI_API_KEY",
</strong><strong>  }),
</strong>});

const client = Instructor({
<strong>  client: portkey,
</strong>  mode: "TOOLS",
});

const UserSchema = z.object({
  age: z.number().describe("The age of the user"),
  name: z.string(),
});

const user = await client.chat.completions.create({
  messages: [{ role: "user", content: "Jason Liu is 30 years old" }],
  model: "claude-3-sonnet-20240229",
  //  model: "gpt-4",
  max_tokens: 512,
  response_model: {
    schema: UserSchema,
    name: "User",
  },
});

console.log(user);
</code></pre>
{% endtab %}
{% endtabs %}

### Caching Your Requests

Let's now bring down the cost of running your Instructor pipeline with Portkey caching. You can just create a Config object where you define your cache setting:

```json
{
  "cache": {
    "mode": "simple"
  }
}
```

You can write it raw, or use Portkey's Config builder and get a corresponding `config id`. Then, just pass it while instantiating your OpenAI client:

{% tabs %}
{% tab title="Instructor Python - OpenAI" %}
<pre class="language-python"><code class="lang-python">import instructor
from pydantic import BaseModel
from openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
cache_config = {
  "cache": {
    "mode": "simple"
  }
}

portkey = OpenAI(
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        virtual_key="OPENAI_VIRTUAL_KEY",
</strong><strong>        api_key="PORTKEY_API_KEY",
</strong><strong>        config=cache_config # Or pass your Config ID saved from Portkey app
</strong><strong>    )
</strong>)

class User(BaseModel):
    name: str
    age: int

<strong>client = instructor.from_openai(portkey)
</strong>
user_info = client.chat.completions.create(
    model="gpt-4-turbo",
    max_tokens=1024,
    response_model=User,
    messages=[{"role": "user", "content": "John Doe is 30 years old."}],
)

print(user_info.name)
print(user_info.age)
</code></pre>
{% endtab %}

{% tab title="Instructor JS - OpenAI" %}
<pre class="language-javascript"><code class="lang-javascript">import Instructor from "@instructor-ai/instructor";
import OpenAI from "openai";
import { z } from "zod";
<strong>import { PORTKEY_GATEWAY_URL, createHeaders } from "portkey-ai";
</strong>
const cache_config = {
  "cache": {
    "mode": "simple"
  }
}

const portkey = new OpenAI({
<strong>  baseURL: PORTKEY_GATEWAY_URL,
</strong><strong>  defaultHeaders: createHeaders({
</strong><strong>    apiKey: "PORTKEY_API_KEY",
</strong><strong>    virtualKey: "OPENAI_API_KEY",
</strong><strong>    config: cache_config // Or pass your Config ID saved from Portkey app
</strong><strong>  }),
</strong>});

const client = Instructor({
<strong>  client: portkey,
</strong>  mode: "TOOLS",
});

const UserSchema = z.object({
  age: z.number().describe("The age of the user"),
  name: z.string(),
});

const user = await client.chat.completions.create({
  messages: [{ role: "user", content: "Jason Liu is 30 years old" }],
  model: "claude-3-sonnet-20240229",
  //  model: "gpt-4",
  max_tokens: 512,
  response_model: {
    schema: UserSchema,
    name: "User",
  },
});

console.log(user);
</code></pre>
{% endtab %}
{% endtabs %}

Similarly, you can add [Fallback](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md), [Loadbalancing](../../product/ai-gateway-streamline-llm-integrations/load-balancing.md), [Timeout](../../product/ai-gateway-streamline-llm-integrations/request-timeouts.md), or [Retry](../../product/ai-gateway-streamline-llm-integrations/automatic-retries.md) settings to your Configs and make your Instructor requests robust & reliable.
