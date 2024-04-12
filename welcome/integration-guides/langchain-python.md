---
description: Portkey adds core production capabilities to any Langchain app.
---

# Langchain (Python)

{% hint style="info" %}
This guide covers the integration for the **Python** flavour of Langchain. Docs for the JS Langchain integration are coming soon.
{% endhint %}

**LangChain** is a framework for developing applications powered by language models. It enables applications that:

* **Are context-aware**: connect a language model to sources of context (prompt instructions, few shot examples, content to ground its response in, etc.)
* **Reason**: rely on a language model to reason (about how to answer based on provided context, what actions to take, etc.)

You can find more information about it [here](https://python.langchain.com/docs/get\_started/quickstart).

When using Langchain, **Portkey can help take it to production** by adding a fast AI gateway, observability, prompt management and more to your Langchain app.

## Quick Start Integration

Install the Portkey and Langchain Python SDKs to get started.

```bash
pip install -U langchain-core portkey_ai langchain-openai
```

We installed `langchain-core` to skip the optional dependencies. You can also install `langchain` if you prefer.

{% hint style="info" %}
Since Portkey is fully compatible with the OpenAI signature, you can connect to the Portkey Ai Gateway through the ChatOpenAI interface.

* Set the `base_url` as `PORTKEY_GATEWAY_URL`
* Add `default_headers` to consume the headers needed by Portkey using the `createHeaders` helper method.
{% endhint %}

We can now initialise the model and update the model to use Portkey's AI gateway

{% code overflow="wrap" %}
````python
```python
from langchain_openai import ChatOpenAI
from portkey_ai import createHeaders, PORTKEY_GATEWAY_URL

PORTKEY_API_KEY = "..."
PROVIDER_API_KEY = "..." # Add the API key of the AI provider being used

portkey_headers = createHeaders(api_key=PORTKEY_API_KEY,provider="openai")

llm = ChatOpenAI(api_key=PROVIDER_API_KEY, base_url=PORTKEY_GATEWAY_URL, default_headers=portkey_headers)

llm.invoke("What is the meaning of life, universe and everything?")
```
````
{% endcode %}

Response

{% code overflow="wrap" %}
```sh
AIMessage(content='The meaning of life, universe, and everything is a question that has puzzled humanity for centuries. In the book "The Hitchhiker\'s Guide to the Galaxy" by Douglas Adams, the answer to this ultimate question is humorously given as "42." This has since become a popular meme and cultural reference.\n\nIn a more philosophical sense, the meaning of life, universe, and everything is subjective and can vary greatly from person to person. Some may find meaning in religious beliefs, others in personal relationships, achievements, or experiences. Ultimately, it is up to each individual to find their own meaning and purpose in life.', response_metadata={'token_usage': {'completion_tokens': 124, 'prompt_tokens': 18, 'total_tokens': 142}, 'model_name': 'gpt-3.5-turbo', 'system_fingerprint': 'fp_c2295e73ad', 'finish_reason': 'stop', 'logprobs': None}, id='run-d2b790be-589b-4c72-add8-a36e098ab277-0')
```
{% endcode %}

The call and the corresponding prompt will also be visible on the Portkey logs tab.

<figure><img src="../../.gitbook/assets/langchain-logs.gif" alt=""><figcaption></figcaption></figure>

### Using Virtual Keys for Multiple Models

Portkey supports [Virtual Keys](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md) which are an easy way to store and manage API keys in a secure vault. Lets try using a Virtual Key to make LLM calls.

**1. Create a Virtual Key in your Portkey account and copy the id**

Let's try creating a new Virtual Key for Mistral like this

<figure><img src="../../.gitbook/assets/create_virtual_key.gif" alt=""><figcaption></figcaption></figure>

**2. Use Virtual Keys in the Portkey Headers**

The `virtual_key` parameter sets the authentication and provider for the AI provider being used. In our case we're using the Mistral Virtual key.

{% hint style="info" %}
Notice that the `api_key` can be left blank as that authentication won't be used.
{% endhint %}

```python
from langchain_openai import ChatOpenAI
from portkey_ai import createHeaders, PORTKEY_GATEWAY_URL

PORTKEY_API_KEY = "..."
VIRTUAL_KEY = "..." # Mistral's virtual key we copied above

portkey_headers = createHeaders(api_key=PORTKEY_API_KEY,virtual_key=VIRTUAL_KEY)

llm = ChatOpenAI(api_key="X", base_url=PORTKEY_GATEWAY_URL, default_headers=portkey_headers, model="mistral-large-latest")

llm.invoke("What is the meaning of life, universe and everything?")
```

The Portkey AI gateway will authenticate the API request to Mistral and get the response back in the OpenAI format for you to consume.

The AI gateway extends Langchain's `ChatOpenAI` class making it a single interface to call any provider and any model.

### Embeddings

Embeddings in Langchain through Portkey work the same way as the Chat Models using the `OpenAIEmbeddings` class. Let's try to create an embedding using OpenAI's embedding model

```python
from langchain_openai import OpenAIEmbeddings  

PORTKEY_API_KEY = "..."
VIRTUAL_KEY = "..." # Add OpenAI's Virtual Key created in Portkey

portkey_headers = createHeaders(api_key=PORTKEY_API_KEY,virtual_key=VIRTUAL_KEY)
  
embeddings_model = OpenAIEmbeddings(api_key="X", base_url=PORTKEY_GATEWAY_URL, default_headers=portkey_headers)
embeddings = embeddings_model.embed_documents(["Hi there!"])
len(embeddings[0])
```

{% hint style="info" %}
Only OpenAI is supported as an embedding provider for Langchain because internally, Langchain converts the texts into tokens which are then sent as input to the API. This method of embedding tokens instead of strings via the API is ONLY supported by OpenAI.

If you plan to use any other embedding model, we recommend using the Portkey SDK directly to make embedding calls.
{% endhint %}

### Chains & Prompts

[Chains](https://python.langchain.com/docs/modules/chains/) enable the integration of various Langchain concepts for simultaneous execution while Langchain supports [Prompt Templates](https://python.langchain.com/docs/modules/model\_io/prompts/) to construct inputs for language models. Lets see how this would work with Portkey

```python
from langchain_openai import ChatOpenAI
from portkey_ai import createHeaders, PORTKEY_GATEWAY_URL
from langchain_core.prompts import ChatPromptTemplate

PORTKEY_API_KEY = "..."
VIRTUAL_KEY = "..." # Add the OpenAI Virtual key

portkey_headers = createHeaders(api_key=PORTKEY_API_KEY,provider="openai")

chat = ChatOpenAI(api_key=PROVIDER_API_KEY, base_url=PORTKEY_GATEWAY_URL, default_headers=portkey_headers)

prompt = ChatPromptTemplate.from_messages([ 
	("system", "You are world class technical documentation writer."), 
	("user", "{input}") 
])
  
chain = prompt | chat 
chain.invoke({"input": "Explain the concept of an API"})
```

We'd be able to view the exact prompt that was used to make the call to OpenAI in the Portkey logs dashboards.

### Using Advanced Routing

The Portkey AI Gateway brings capabilities like load-balancing, fallbacks, experimentation and canary testing to Langchain through a configuration-first approach.

Let's take an **example** where we might want to split traffic between gpt-4 and claude-opus 50:50 to test the two large models. The gateway configuration for this would look like the following:

```python
config = {
	"strategy": {
		"mode": "loadbalance"
	},
	"targets": [{
		"virtual_key": "openai-25654" # OpenAI's virtual key
		"override_params": {"model": "gpt4"},
		"weight": 0.5
	}, {
		"virtual_key": "anthropic-25654" # Anthropic's virtual key
		"override_params": {"model": "claude-opus-20240229"},
		"weight": 0.5
	}]
}
```

We can then use this config in our requests being made from langchain.

```python
portkey_headers = createHeaders(
	api_key=PORTKEY_API_KEY,
	config=config
)

llm = ChatOpenAI(api_key="X", base_url=PORTKEY_GATEWAY_URL, default_headers=portkey_headers)

llm.invoke("What is the meaning of life, universe and everything?")
```

When the LLM is invoked, Portkey will distribute the requests to `gpt-4` and `claude-opus-20240229` in the ratio of the defined weights.

You can find more config examples [here](../../api-reference/config-object.md#examples).

### Agents & Tracing

A powerful capability of Langchain is creating Agents. The challenge with agentic workflows is that prompts are often abstracted out and it's hard to get a visibility into what the agent is doing. This also makes debugging harder.

Portkey's Langchain integration gives you full visibility into the running of an agent. Let's take an example of a [popular agentic workflow](https://python.langchain.com/docs/use\_cases/tool\_use/quickstart/#agents).

```python
from langchain import hub  
from langchain.agents import AgentExecutor, create_openai_tools_agent  
from langchain_openai import ChatOpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
 
prompt = hub.pull("hwchase17/openai-tools-agent")

portkey_headers = createHeaders(
    api_key=PORTKEY_API_KEY,
    virtual_key=OPENAI_VIRTUAL_KEY,
    trace_id="uuid-uuid-uuid-uuid"
)

@tool  
def add(first_int: int, second_int: int) -> int:  
    "Add two integers."  
    return first_int + second_int  
  
  
@tool  
def exponentiate(base: int, exponent: int) -> int:  
    "Exponentiate the base to the exponent power."  
    return base**exponent  
  
  
tools = [multiply, add, exponentiate]

model = ChatOpenAI(api_key="X", base_url=PORTKEY_GATEWAY_URL, default_headers=portkey_headers, temperature=0)
  
# Construct the OpenAI Tools agent  
agent = create_openai_tools_agent(model, tools, prompt)

# Create an agent executor by passing in the agent and tools
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

agent_executor.invoke({
    "input": "Take 3 to the fifth power and multiply that by the sum of twelve and three, then square the whole result"
})
```

Running this would yield the following logs in Portkey.

<figure><img src="../../.gitbook/assets/agent_tracing.gif" alt=""><figcaption></figcaption></figure>

This is extremely powerful since you gain control and visibility over the agent flows so you can identify problems and make updates as needed.
