# Cohere

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including Cohere's generation, embedding, and other endpoints.

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) system.

## Portkey SDK Integration with Cohere

Portkey provides a consistent API to interact with models from Cohere. To integrate Cohere with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Cohere's models through Portkey's gateway.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
npm install --save portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```python
pip install portkey-ai
```
{% endtab %}
{% endtabs %}

### **2. Initialize Portkey with the Virtual Key**

Set up Portkey with your virtual key as part of the initialization configuration. You can create a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) for Cohere in the UI.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Cohere Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    apiKey="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtualKey="VIRTUAL_KEY"   # Replace with your virtual key for Cohere
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with Cohere**&#x20;

Use the Portkey instance to send requests to Cohere's models. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'command',
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ role: 'user', content: 'Say this is a test' }],
    model= 'claude-2'
})
```
{% endtab %}
{% endtabs %}

## Managing Cohere Prompts

You can manage all prompts to Cohere in the [Prompt Library](../../product/prompt-library.md). All the current models of Cohere are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Other Cohere Endpoints

### Embeddings

Embedding endpoints are natively supported within Portkey like this:

```javascript
const embedding = await portkey.embeddings.create({
    input: 'Name the tallest buildings in Hawaii',
    model: 'command',
});

console.log(embedding);
```

### Re-ranking

You can use cohere reranking the `portkey.post` method with the body expected by [Cohere's reranking API](https://docs.cohere.com/reference/rerank-1).

```python
const response = portkey.post({
  "return_documents": false,
  "max_chunks_per_doc": 10,
  "model": "rerank-english-v2.0",
  "query": "What is the capital of the United States?",
  "documents": [
    "Carson City is the capital city of the American state of Nevada.",
    "The Commonwealth of the Northern Mariana Islands is a group of islands in the Pacific Ocean. Its capital is Saipan.",
    "Washington, D.C. (also known as simply Washington or D.C., and officially as the District of Columbia) is the capital of the United States. It is a federal district.",
    "Capital punishment (the death penalty) has existed in the United States since beforethe United States was a country. As of 2017, capital punishment is legal in 30 of the 50 states."
  ]
})
```
