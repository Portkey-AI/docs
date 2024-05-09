# Instructor

**Instructor** is a framework for extracting structured outputs from LLMs, available in [Python](https://python.useinstructor.com/) & [JS](https://instructor-ai.github.io/instructor-js/).

With Portkey, you can confidently take your Instructor pipelines to production and get complete observability over all of your calls + make them reliable and interoperable.

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

{% tab title="Instructor Python - Anthropic" %}
<pre class="language-python"><code class="lang-python">import instructor
from pydantic import BaseModel
from anthropic import Anthropic
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
portkey = Anthropic(
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        virtual_key="ANTHROPIC_VIRTUAL_KEY",
</strong><strong>        api_key="PORTKEY_API_KEY"
</strong><strong>    )
</strong>)

class User(BaseModel):
    name: str
    age: int

<strong>client = instructor.from_anthropic(portkey)
</strong>
user_info = client.messages.create(
    model="claude-3-opus-20240229",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Extract Jason is 25 years old."}],
    response_model=User,
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

\
