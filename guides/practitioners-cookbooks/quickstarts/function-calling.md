---
description: Get the LLM to interact with external APIs!
---

# Function Calling

As described in the [Enforcing JSON Schema cookbook](../how-tos/enforcing-json-schema-with-anyscale-and-together.md), LLMs are now good at generating outputs that follow a specified syntax. We can combine this LLM ability with their reasoning ability to let LLMs interact with external APIs. **This is called Function (or Tool) calling.** In simple terms, function calling:&#x20;

1. Informs the user when a question can be answered using an external API
2. Generates a valid request in the API's format
3. Converts the API's response to a natural language answer

Function calling is currently supported on select models on **Anyscale**, **Together AI**, **Fireworks AI**, **Google Gemini**, and **OpenAI**. Using Portkey, you can easily experiment with function calling across various providers and gain confidence to ship it to production.

**Let's understand how it works with an example**:&#x20;

We want the  LLM to tell what's the temperature in Delhi today. We'll use a "Weather API" to fetch the weather:

{% tabs %}
{% tab title="Node" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from "portkey-ai";

const portkey = new Portkey({
  apiKey: "PORTKEY_API_KEY",
  virtualKey: "ANYSCALE_VIRTUAL_KEY",
});

// Describing what the Weather API does and expects
let tools = [
    {
        "type": "function",
        "function": {
<strong>            "name": "getWeather",
</strong>            "description": "Get the current weather in a given location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The city and state",
                    },
                    "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
                },
                "required": ["location"],
            },
        },
    }
];

let response = await portkey.chat.completions.create({
    model: "mistralai/Mixtral-8x7B-Instruct-v0.1",
    messages: [
        {"role": "system", "content": "You are helpful assistant."},
<strong>        {"role": "user", "content": "What's the weather like in Delhi - respond in JSON"}
</strong>    ],
<strong>    tools,
</strong><strong>    tool_choice: "auto", // auto is default, yet explicit
</strong>});

console.log(response.choices[0].finish_reason) 
</code></pre>
{% endtab %}
{% endtabs %}

Here, we've defined what the Weather API expects for its requests in the `tool` param, and set `tool_choice` to auto. So, based on the user messages, the LLM will decide if it should do a function call to fulfill the request. Here, it will choose to do that, and we'll see the following output:

<pre class="language-json"><code class="lang-json">{
    "role": "assistant",
    "content": null,
    "tool_calls": [
        "id": "call_x8we3xx", 
        "type": "function", 
<strong>        "function": {
</strong><strong>            "name": "getWeather", 
</strong><strong>            "arguments": '{\n  "location": "Delhi, India",\n  "format": "celsius"\n}'
</strong><strong>        }
</strong>    ],
}
</code></pre>

We can just take the `tool_call` made by the LLM, and pass it to our `getWeather` function - it should return a proper response to our query. We then take that response and send it to our LLM to complete the loop:

```javascript
/**
* getWeather(..) is a utility to call external weather service APIs
* Responds with: {"temperature": 20, "unit": "celsius"}
**/

let weatherData = await getWeather(JSON.parse(arguments));
let content = JSON.stringify(weatherData);

// Push assistant and tool message from earlier generated function arguments
messages.push(assistantMessage); //
messages.push({
    role: "tool", 
    content: content, 
    toolCallId: "call_x8we3xx"
    name: "getWeather"
});

let response = await portkey.chat.completions.create({
  model: "mistralai/Mixtral-8x7B-Instruct-v0.1",
  tools:tools,
  messages:messages,
  tool_choice: "auto",
});

```

We should see this final output:

```
{
    "role": "assistant",
    "content": "It's 30 degrees celsius in Delhi, India.",
}
```

## Function Calling Workflow

Recapping, there are 4 key steps to doing function calling, as illustrated below:

<div align="center">

<figure><img src="../../../.gitbook/assets/image (3) (1).png" alt=""><figcaption><p>Function Calling Workflow</p></figcaption></figure>

</div>

## Supporting Models

While most providers havve standard function calling as illustrated above, models on Together AI & select new models on OpenAI (`gpt-4-turbo-preview`, `gpt-4-0125-preview`, `gpt-4-1106-preview`, `gpt-3.5-turbo-0125`, and `gpt-3.5-turbo-1106`) also support **parallel function calling** - here, you can pass multiple requests in a single query, the model will pick the relevant tool for each query, and return an array of `tool_calls` each with a unique ID. ([Read here for more info](https://platform.openai.com/docs/guides/function-calling/parallel-function-calling))

<table><thead><tr><th width="291">Model/Provider</th><th width="235">Standard Function Calling</th><th>Parallel Function Calling</th></tr></thead><tbody><tr><td><code>mistralai/Mistral-7B-Instruct-v0.1</code> <br>Anyscale</td><td>✅</td><td>❌</td></tr><tr><td><code>mistralai/Mixtral-8x7B-Instruct-v0.1</code><br>Anyscale</td><td>✅</td><td>❌</td></tr><tr><td><code>mistralai/Mixtral-8x7B-Instruct-v0.1</code><br>Together AI</td><td>✅</td><td>✅</td></tr><tr><td><code>mistralai/Mistral-7B-Instruct-v0.1</code><br>Together AI</td><td>✅</td><td>✅</td></tr><tr><td><code>togethercomputer/CodeLlama-34b-Instruct</code><br>Together AI</td><td>✅</td><td>✅</td></tr><tr><td><code>gpt-4</code> and previous releases<br>OpenAI / Azure OpenAI</td><td>✅</td><td>✅ (some)</td></tr><tr><td><code>gpt-3.5-turbo</code> and previous releases<br>OpenAI / Azure OpenAI</td><td>✅</td><td>✅ (some)</td></tr><tr><td><code>firefunction-v1</code><br>Fireworks</td><td>✅</td><td>❌</td></tr><tr><td><code>fw-function-call-34b-v0</code><br>Fireworks</td><td><a href="https://github.com/Portkey-AI/gateway/issues/335">✅</a></td><td>❌</td></tr><tr><td><p><code>gemini-1.0-pro</code></p><p><code>gemini-1.0-pro-001</code></p><p><code>gemini-1.5-pro-lates</code><br><strong>Google</strong></p></td><td><a href="https://github.com/Portkey-AI/gateway/issues/335">✅</a></td><td>❌</td></tr></tbody></table>

