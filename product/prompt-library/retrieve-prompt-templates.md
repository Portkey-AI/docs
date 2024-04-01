---
description: >-
  See how to use Portkey's prompt templates with OpenAI (or any other provider)
  SDKs
---

# Retrieve Prompt Templates

You can retrieve your saved prompts on Portkey using the `/prompts/$PROMPT_ID/render` endpoint. Portkey returns a JSON containing your prompt or messages body along with all the saved parameters that you can directly use in any request.

This is helpful if you are required to use provider SDKs and can not use the Portkey SDK in production. ([Example of how to use Portkey prompt templates with OpenAI SDK](retrieve-prompt-templates.md#using-the-render-output-in-a-new-request))

## Using the `Render` Endpoint/Method

1. Make a request to `https://api.portkey.ai/v1/prompts/$PROMPT_ID/render` with your prompt ID
2. Pass your Portkey API key with `x-portkey-api-key` in the header
3. Send up the variables in your payload with `{ "variables": { "VARIABLE_NAME": "VARIABLE_VALUE" } }`

That's it! See it in action:

{% tabs %}
{% tab title="REST" %}
<pre class="language-bash" data-full-width="true"><code class="lang-bash"><strong>curl -X POST "https://api.portkey.ai/v1/prompts/$PROMPT_ID/render" \
</strong>-H "Content-Type: application/json" \
-H "x-portkey-api-key: $PORTKEY_API_KEY" \
-d '{
  "variables": {"movie":"Dune 2"}
}'
</code></pre>

The Output:

```json
{
    "success": true,
    "data": {
        "model": "gpt-4",
        "n": 1,
        "top_p": 1,
        "max_tokens": 256,
        "temperature": 0,
        "presence_penalty": 0,
        "frequency_penalty": 0,
        "messages": [
            {
                "role": "system",
                "content": "You're a helpful assistant."
            },
            {
                "role": "user",
                "content": "Who directed Dune 2?"
            }
        ]
    }
}
```
{% endtab %}

{% tab title="Portkey Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
  api_key="PORTKEY_API_KEY"
)

render = portkey.prompts.render(
  prompt_id="PROMPT_ID",
  variables={ "movie":"Dune 2" }
)

print(render.data)
```

The Output:

```json
{
    "model": "gpt-4",
    "n": 1,
    "top_p": 1,
    "max_tokens": 256,
    "temperature": 0,
    "presence_penalty": 0,
    "frequency_penalty": 0,
    "messages": [
        {
            "role": "system",
            "content": "You're a helpful assistant."
        },
        {
            "role": "user",
            "content": "Who directed Dune 2?"
        }
    ]
}
```
{% endtab %}

{% tab title="Portkey Node SDK" %}
```typescript
import Portkey from 'portkey-ai'

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY"
})

async function getRender(){
    const render = await portkey.prompts.render({
        promptID: "PROMPT_ID",
        variables: { "movie":"Dune 2" }
    })
    console.log(render.data)
}

getRender()
```

The Output:

```json
{
    "model": "gpt-4",
    "n": 1,
    "top_p": 1,
    "max_tokens": 256,
    "temperature": 0,
    "presence_penalty": 0,
    "frequency_penalty": 0,
    "messages": [
        {
            "role": "system",
            "content": "You're a helpful assistant."
        },
        {
            "role": "user",
            "content": "Who directed Dune 2?"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

## Updating Prompt Params While Retrieving the Prompt

If you want to change any model params (like `temperature`, `messages body` etc) while retrieving your prompt from Portkey, you can send the override params in your `render` payload.&#x20;

Portkey will send back your prompt with overridden params, **without** making any changes to the saved prompt on Portkey.

{% tabs %}
{% tab title="REST" %}
<pre class="language-bash"><code class="lang-bash">curl -X POST "https://api.portkey.ai/v1/prompts/$PROMPT_ID/render" \
-H "Content-Type: application/json" \
-H "x-portkey-api-key: $PORTKEY_API_KEY" \
-d '{
  "variables": {"movie":"Dune 2"},
<strong>  "model": "gpt-3.5-turbo",
</strong><strong>  "temperature: 2
</strong>}'
</code></pre>

Based on the above snippet, `model` and `temperature` params in the retrieved prompt will be **overridden** with the newly passed values

The New Output:

<pre class="language-json"><code class="lang-json">{
    "success": true,
    "data": {
<strong>        "model": "gpt-3.5-turbo",
</strong>        "n": 1,
        "top_p": 1,
        "max_tokens": 256,
<strong>        "temperature": 2,
</strong>        "presence_penalty": 0,
        "frequency_penalty": 0,
        "messages": [
            {
                "role": "system",
                "content": "You're a helpful assistant."
            },
            {
                "role": "user",
                "content": "Who directed Dune 2?"
            }
        ]
    }
}
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
  api_key="PORTKEY_API_KEY"
)

render = portkey.prompts.render(
  prompt_id="PROMPT_ID",
  variables={ "movie":"Dune 2" },
<strong>  model="gpt-3.5-turbo",
</strong><strong>  temperature=2
</strong>)

print(render.data)
</code></pre>

Based on the above snippet, `model` and `temperature` params in the retrieved prompt will be **overridden** with the newly passed values.

**The New Output:**

```json
{
    "model": "gpt-3.5-turbo",
    "n": 1,
    "top_p": 1,
    "max_tokens": 256,
    "temperature": 2,
    "presence_penalty": 0,
    "frequency_penalty": 0,
    "messages": [
        {
            "role": "system",
            "content": "You're a helpful assistant."
        },
        {
            "role": "user",
            "content": "Who directed Dune 2?"
        }
    ]
}
```
{% endtab %}

{% tab title="Node SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai'

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY"
})

async function getRender(){
    const render = await portkey.prompts.render({
        promptID: "PROMPT_ID",
        variables: { "movie":"Dune 2" },
<strong>        model: "gpt-3.5-turbo",
</strong><strong>        temperature: 2
</strong>    })
    console.log(render.data)
}

getRender()
</code></pre>

Based on the above snippet, `model` and `temperature` params in the retrieved prompt will be **overridden** with the newly passed values.

**The New Output:**

```json
{
    "model": "gpt-3.5-turbo",
    "n": 1,
    "top_p": 1,
    "max_tokens": 256,
    "temperature": 2,
    "presence_penalty": 0,
    "frequency_penalty": 0,
    "messages": [
        {
            "role": "system",
            "content": "You're a helpful assistant."
        },
        {
            "role": "user",
            "content": "Who directed Dune 2?"
        }
    ]
}
```
{% endtab %}
{% endtabs %}

## Using the `render` Output in a New Request

Here's how you can take the output from the `render` API and use it for making a call. We'll take example of OpenAI SDKs, but you can use it simlarly for any other provider SDK as well.

{% tabs %}
{% tab title="OpenAI Node" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai';
import OpenAI from 'openai';

// Retrieving the Prompt from Portkey

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY"
})

async function getPromptTemplate() {
    const render_response = await portkey.prompts.render({
        promptID: "PROMPT_ID",
        variables: { "movie":"Dune 2" }
    })
    return render_response.data;
}

// Making a Call to OpenAI with the Retrieved Prompt

const openai = new OpenAI({
    apiKey: 'OPENAI_API_KEY',
    baseURL: 'https://api.portkey.ai/v1',
    defaultHeaders: {
      'x-portkey-provider': 'openai',
      'x-portkey-api-key': 'PORTKEY_API_KEY',
      'Content-Type': 'application/json',
    }
});

async function main() {
    const PROMPT_TEMPLATE = await getPromptTemplate();
<strong>    const chatCompletion = await openai.chat.completions.create(PROMPT_TEMPLATE);
</strong>    console.log(chatCompletion.choices[0]);
}

main();
</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from portkey_ai import Portkey
from openai import OpenAI

# Retrieving the Prompt from Portkey

portkey = Portkey(
  api_key="PORTKEY_API_KEY"
)

render_response = portkey.prompts.render(
  prompt_id="PROMPT_ID",
  variables={ "movie":"Dune 2" }
)

PROMPT_TEMPLATE = render_response.data

# Making a Call to OpenAI with the Retrieved Prompt

openai = OpenAI(
    api_key = "OPENAI_API_KEY",
    base_url = "https://api.portkey.ai/v1",
    default_headers = {
      'x-portkey-provider': 'openai',
      'x-portkey-api-key': 'PORTKEY_API_KEY',
      'Content-Type': 'application/json',
    }
)

chat_complete = openai.chat.completions.create(**PROMPT_TEMPLATE)

print(chat_complete.choices[0].message.content)
```
{% endtab %}
{% endtabs %}

