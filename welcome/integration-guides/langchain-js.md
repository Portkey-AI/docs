---
description: Portkey adds core production capabilities to any Langchain app.
---

# Langchain (JS/TS)

{% hint style="info" %}
This guide covers the integration for the **Javascript / Typescript** flavour of Langchain. Docs for the Python Langchain integration are [here](langchain-python.md).
{% endhint %}

**LangChain** is a framework for developing applications powered by language models. It enables applications that:

* **Are context-aware**: connect a language model to sources of context (prompt instructions, few shot examples, content to ground its response in, etc.)
* **Reason**: rely on a language model to reason (about how to answer based on provided context, what actions to take, etc.)

You can find more information about it [here](https://python.langchain.com/docs/get\_started/quickstart).

When using Langchain, **Portkey can help take it to production** by adding a fast AI gateway, observability, prompt management and more to your Langchain app.

## Quick Start Integration

Install the Portkey and Langchain SDKs to get started.

```bash
npm install langchain portkey-ai @langchain/openai
```

{% hint style="info" %}
Since Portkey is fully compatible with the OpenAI signature, you can connect to the Portkey Ai Gateway through the `ChatOpenAI` interface.

* Set the `baseURL` as `PORTKEY_GATEWAY_URL`
* Add `defaultHeaders` to consume the headers needed by Portkey using the `createHeaders` helper method.
{% endhint %}

We can now initialise the model and update the model to use Portkey's AI gateway

{% code overflow="wrap" %}
```javascript
import { ChatOpenAI } from "@langchain/openai";
import { createHeaders, PORTKEY_GATEWAY_URL} from "portkey-ai"

const PORTKEY_API_KEY = "..."
const PROVIDER_API_KEY = "..." // Add the API key of the AI provider being used

const portkeyConf = {
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({apiKey: PORTKEY_API_KEY, provider: "openai"})
}

const chatModel = new ChatOpenAI({
  apiKey: PROVIDER_API_KEY,
  configuration: portkeyConf
});

await chatModel.invoke("What is the meaning of life, universe and everything?")
```
{% endcode %}

Response

```sh
AIMessage {
  lc_serializable: true,
  lc_kwargs: {
    content: `The phrase "the meaning of life, universe, and everything" is a reference to Douglas Adams' science fiction series, "The Hitchhiker's Guide to the Galaxy." In the series, a supercomputer called Deep Thought was asked to calculate the Answer to the Ultimate Question of Life, the Universe, and Everything. After much deliberation, Deep Thought revealed that the answer was simply the number 42.\n` +
      '\n' +
      'In the context of the series, the number 42 is meant to highlight the absurdity and unknowability of the ultimate meaning of life and the universe. It is a humorous and satirical take on the deep philosophical questions that have puzzled humanity for centuries.\n' +
      '\n' +
      'Ultimately, the meaning of life, universe, and everything is a complex and deeply personal question that each individual must grapple with and find their own answer to. It may be different for each person and can encompass a wide range of beliefs, values, and experiences.',
    tool_calls: [],
    invalid_tool_calls: [],
    additional_kwargs: { function_call: undefined, tool_calls: undefined },
    response_metadata: {}
  },
  lc_namespace: [ 'langchain_core', 'messages' ],
  content: `The phrase "the meaning of life, universe, and everything" is a reference to Douglas Adams' science fiction series, "The Hitchhiker's Guide to the Galaxy." In the series, a supercomputer called Deep Thought was asked to calculate the Answer to the Ultimate Question of Life, the Universe, and Everything. After much deliberation, Deep Thought revealed that the answer was simply the number 42.\n` +
    '\n' +
    'In the context of the series, the number 42 is meant to highlight the absurdity and unknowability of the ultimate meaning of life and the universe. It is a humorous and satirical take on the deep philosophical questions that have puzzled humanity for centuries.\n' +
    '\n' +
    'Ultimately, the meaning of life, universe, and everything is a complex and deeply personal question that each individual must grapple with and find their own answer to. It may be different for each person and can encompass a wide range of beliefs, values, and experiences.',
  name: undefined,
  additional_kwargs: { function_call: undefined, tool_calls: undefined },
  response_metadata: {
    tokenUsage: { completionTokens: 186, promptTokens: 18, totalTokens: 204 },
    finish_reason: 'stop'
  },
  tool_calls: [],
  invalid_tool_calls: []
}
```

The call and the corresponding prompt will also be visible on the Portkey logs tab.

<figure><img src="../../.gitbook/assets/langchain-logs.gif" alt=""><figcaption></figcaption></figure>

## Using Virtual Keys for Multiple Models

Portkey supports [Virtual Keys](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) which are an easy way to store and manage API keys in a secure vault. Lets try using a Virtual Key to make LLM calls.

#### **1. Create a Virtual Key in your Portkey account and copy the id**

Let's try creating a new Virtual Key for Mistral like this

<figure><img src="../../.gitbook/assets/create_virtual_key.gif" alt=""><figcaption></figcaption></figure>

#### **2. Use Virtual Keys in the Portkey Headers**

The `virtualKey` parameter sets the authentication and provider for the AI provider being used. In our case we're using the Mistral Virtual key.

{% hint style="info" %}
Notice that the `apiKey` can be left blank as that authentication won't be used.
{% endhint %}

{% code overflow="wrap" %}
```javascript
import { ChatOpenAI } from "@langchain/openai";
import { createHeaders, PORTKEY_GATEWAY_URL} from "portkey-ai"

const PORTKEY_API_KEY = "..."
const MISTRAL_VK = "..." // Add the virtual key for mistral that we just created

const portkeyConf = {
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({apiKey: PORTKEY_API_KEY, virtualKey: MISTRAL_VK})
}

const chatModel = new ChatOpenAI({
  apiKey: "X",
  configuration: portkeyConf,
  model: "mistral-large-latest"
});

await chatModel.invoke("What is the meaning of life, universe and everything?")
```
{% endcode %}

The Portkey AI gateway will authenticate the API request to Mistral and get the response back in the OpenAI format for you to consume.

The AI gateway extends Langchain's `ChatOpenAI` class making it a single interface to call any provider and any model.

## Embeddings

Embeddings in Langchain through Portkey work the same way as the Chat Models using the `OpenAIEmbeddings` class. Let's try to create an embedding using OpenAI's embedding model

```javascript
import { OpenAIEmbeddings } from "@langchain/openai";
import { createHeaders, PORTKEY_GATEWAY_URL} from "portkey-ai"

const PORTKEY_API_KEY = "...";
const OPENAI_VK = "..." // Add OpenAI's Virtual Key created in Portkey

const portkeyConf = {
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({apiKey: PORTKEY_API_KEY, virtualKey: OPENAI_VK})
}

/* Create instance */
const embeddings = new OpenAIEmbeddings({
  apiKey: "X",
  configuration: portkeyConf,
});

/* Embed queries */
const res = await embeddings.embedQuery("Hello world");
```

## Chains & Prompts

[Chains](https://python.langchain.com/docs/modules/chains/) enable the integration of various Langchain concepts for simultaneous execution while Langchain supports [Prompt Templates](https://python.langchain.com/docs/modules/model\_io/prompts/) to construct inputs for language models. Lets see how this would work with Portkey

{% code overflow="wrap" %}
```javascript
import { ChatOpenAI } from "@langchain/openai";
import { ChatPromptTemplate } from "@langchain/core/prompts";
import { createHeaders, PORTKEY_GATEWAY_URL} from "portkey-ai"

const PORTKEY_API_KEY = "...";
const OPENAI_VK = "..." // Add OpenAI's Virtual Key created in Portkey

const portkeyConf = {
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({apiKey: PORTKEY_API_KEY, virtualKey: OPENAI_VK})
}

// Initialise the chat model
const chatModel = new ChatOpenAI({
  apiKey: "X",
  configuration: portkeyConf
});

// Define the chat prompt template
const prompt = ChatPromptTemplate.fromMessages([
  ["human", "Tell me a short joke about {topic}"],
]);

// Invoke the chain with the prompt and chat model
const chain = prompt.pipe(chatModel);
const res = await chain.invoke({ topic: "ice cream" });

console.log(res)
```
{% endcode %}

We'd be able to view the exact prompt that was used to make the call to OpenAI in the Portkey logs dashboards.

## Using Advanced Routing

The Portkey AI Gateway brings capabilities like load-balancing, fallbacks, experimentation and canary testing to Langchain through a configuration-first approach.

Let's take an **example** where we might want to split traffic between gpt-4 and claude-opus 50:50 to test the two large models. The gateway configuration for this would look like the following:

```javascript
const config = {
    "strategy": {
         "mode": "loadbalance"
    },
    "targets": [{
        "virtual_key": OPENAI_VK, // OpenAI's virtual key
        "override_params": {"model": "gpt4"},
        "weight": 0.5
    }, {
        "virtual_key": ANTHROPIC_VK, // Anthropic's virtual key
        "override_params": {"model": "claude-3-opus-20240229"},
        "weight": 0.5
    }]
}
```

We can then use this `config` in our requests being made from langchain.

{% code overflow="wrap" %}
```javascript
const portkeyConf = {
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({apiKey: PORTKEY_API_KEY, config: config})
}
  
const chatModel = new ChatOpenAI({
  apiKey: "X",
  configuration: portkeyConf
});
  
const res = await chatModel.invoke("What is the meaning of life, universe and everything?")
```
{% endcode %}

When the LLM is invoked, Portkey will distribute the requests to `gpt-4` and `claude-3-opus-20240229` in the ratio of the defined weights.

You can find more config examples [here](../../api-reference/config-object.md#examples).

## Agents & Tracing

A powerful capability of Langchain is creating Agents. The challenge with agentic workflows is that prompts are often abstracted out and it's hard to get a visibility into what the agent is doing. This also makes debugging harder.

Connect the Portkey configuration to the `ChatOpenAI` model and we'd be able to use all the benefits of the AI gateway as shown above.

Also, Portkey would capture the logs from the agent API calls giving us full visibility.

<figure><img src="../../.gitbook/assets/agent_tracing.gif" alt=""><figcaption></figcaption></figure>

This is extremely powerful since we gain control and visibility over the agent flows so we can identify problems and make updates as needed.
