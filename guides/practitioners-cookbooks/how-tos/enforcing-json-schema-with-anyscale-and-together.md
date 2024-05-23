---
description: >-
  Get the LLM to adhere to your JSON schema using Anyscale & Together AI's newly
  introduced JSON modes
---

# Enforcing JSON Schema with Anyscale & Together

LLMs excel at generating creative text, but production applications demand structured outputs for seamless integration. Instructing LLMs to only generate the output in a specified syntax can help make their behaviour a bit more predictable. JSON is the format of choice here - it is versatile enough and is widely used as a standard data exchange format.&#x20;

Several LLM providers offer features that help enforce JSON outputs:

* OpenAI has a feature called [JSON mode](https://platform.openai.com/docs/guides/text-generation/json-mode) that ensures that the output is a valid JSON object.&#x20;
* While this is great, it doesn't guarantee adherence to your custom JSON schemas, but only that the output IS a JSON.
* Anyscale and Together AI go further - they not only enforce that the output is in JSON but also ensure that the output follows any given JSON schema.

Using Portkey, you can easily experiment with models from Anyscale & Together AI and explore the power of their JSON modes:

{% tabs %}
{% tab title="Node" %}
<pre class="language-typescript" data-full-width="true"><code class="lang-typescript">import Portkey from 'portkey-ai';

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    <a data-footnote-ref href="#user-content-fn-1">virtualKey: "ANYSCALE_VIRTUAL_KEY"</a>// OR "TOGETHER_VIRTUAL_KEY"
})

asyn function main(){
  const json_response = await portkey.chat.completions.create({
    messages: [{role: "user",content: `Give me a recipe for making Ramen, in JSON format`}],
    model: "mistralai/Mistral-7B-Instruct-v0.1",
<strong>    response_format: {
</strong><strong>      type: "json_object",
</strong><strong>      schema: {
</strong><strong>        type: "object",
</strong><strong>        properties: {
</strong><strong>          title: { type: "string" },
</strong><strong>          description: { type: "string" },
</strong><strong>          steps: { type: "array" }
</strong><strong>        }
</strong><strong>      }
</strong><strong>    }
</strong>  });
}

console.log(json_response.choices[0].message.content);

main()
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey
from pydantic import BaseModel, Field

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="ANYSCALE_VIRTUAL_KEY" # OR #TOGETHER_VIRTUAL_KEY
)

class Recipe(BaseModel):
    title: str
    description: str
    steps: List[str]
    
json_response = portkey.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Give me a recipe for making Ramen, in JSON format' }],
    model = 'mistralai/Mistral-7B-Instruct-v0.1',
<strong>    response_format = {
</strong><strong>        "type":"json_object",
</strong><strong>        "schema": Recipe.schema_json()
</strong><strong>    }
</strong>)

print(json_response.choices[0].message.content)
</code></pre>
{% endtab %}
{% endtabs %}

### Output JSON:

```json
{
  "title": "Fried Rice with Chicken and Vegetables",
  "description": "A delicious recipe for fried rice that includes chicken and a mix of colorful vegetables. Perfect for a healthy and satisfying meal. yum yum yum yum yum",
  "steps": [
    "1. Cook rice and set aside",
    "2. In a large skillet or wok, sauté sliced chicken in oil until browned",
    "3. ..."
  ]
}  
```

As you can see - it's pretty simple. Just define the JSON schema, and pass it at the time of making your request using the `response_format` param. The `response_format`'s `type` is `json_object` and the `schema` contains all keys and their expected type.&#x20;

## Supporting Models

<table><thead><tr><th width="431">Model/Provider</th><th width="157">Ensure JSON</th><th>Ensure Schema</th></tr></thead><tbody><tr><td><code>mistralai/Mistral-7B-Instruct-v0.1</code> <br>Anyscale</td><td>✅</td><td>✅</td></tr><tr><td><code>mistralai/Mixtral-8x7B-Instruct-v0.1</code><br>Anyscale</td><td>✅</td><td>✅</td></tr><tr><td><code>mistralai/Mixtral-8x7B-Instruct-v0.1</code><br>Together AI</td><td>✅</td><td>✅</td></tr><tr><td><code>mistralai/Mistral-7B-Instruct-v0.1</code><br>Together AI</td><td>✅</td><td>✅</td></tr><tr><td><code>togethercomputer/CodeLlama-34b-Instruct</code><br>Together AI</td><td>✅</td><td>✅</td></tr><tr><td><code>gpt-4</code> and previous releases<br>OpenAI / Azure OpenAI</td><td>✅</td><td>❌</td></tr><tr><td><code>gpt-3.5-turbo</code> and previous releases<br>OpenAI / Azure OpenAI</td><td>✅</td><td>❌</td></tr><tr><td>Ollama models</td><td>✅</td><td>❌</td></tr></tbody></table>

### Creating Nested JSON Object Schema

Here's an example showing how you can also create nested JSON schema and get the LLM to enforce it:

<pre class="language-python"><code class="lang-python"><strong>class Ingredient(BaseModel):
</strong><strong>    name: str
</strong><strong>    quantity: str
</strong>
class Recipe(BaseModel):
    title: str
    description: str
<strong>    ingredients: List[Ingredient]
</strong>    steps: List[str]
    
json_response = portkey.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Give me a recipe for making Ramen, in JSON format' }],
    model="mistralai/Mistral-7B-Instruct-v0.1",
<strong>    response_format={
</strong><strong>        "type": "json_object", 
</strong><strong>        "schema": Recipe.schema_json()
</strong><strong>    }
</strong>)
</code></pre>

Add your Anyscale or Together AI virtual keys to Portkey vault, and get started!

[^1]: Add Anyscale API key to Portkey vault to generate your disposable virtual key.
