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

If you're using Langchain for your app, **Portkey can help take it to production** by adding a fast AI gateway, observability, prompt management and more to your app.

## Quick Start Integration

### ⭐️ Using the Portkey custom models

Portkey is available as a Custom LLM integration in Langchain. Using this, you can connect to multiple LLM providers (Anthropic, Azure, Huggingface, Anyscale, etc) through Portkey's powerful AI gateway.

<pre class="language-python"><code class="lang-python">from portkey_ai.llms.langchain import PortkeyChat, PortkeyLLM
from langchain.schema.messages import HumanMessage, SystemMessage

# The messages to be sent to the LLM
messages = [
    SystemMessage(content="You're a helpful assistant"),
    HumanMessage(content="What is the purpose of model regularization?"),
]

<strong>llm = PortkeyLLM(api_key="PORTKEY_API_KEY",virtual_key="VIRTUAL_KEY")
</strong>chat_model = PortkeyChat(api_key="PORTKEY_API_KEY",virtual_key="VIRTUAL_KEY")

llm.predict_messages(messages)
>>> The purpose of mode...

chat.invoke(messages)
>>> AIMessage(content="The purpose of mode...
</code></pre>

You can now incorporate the `llm` or `chat_model` in any of your chains and Portkey will log all your requests + offer enhanced production features.

### Using Langchain's OpenAI model

If you prefer to use Langchain's OpenAI model,&#x20;

* Update OpenAI's \`base\_url\` to the Portkey Gateway
* And add Portkey's headers to the `default_headers` parameter

The same example using the OpenAI model in Langchain would look like this

<pre class="language-python"><code class="lang-python">from langchain.llms import OpenAI
from langchain.chat_models import ChatOpenAI
from portkey_ai import createHeaders, PORTKEY_GATEWAY_URL

<strong>portkeyHeaders = createHeaders(
</strong><strong>    mode="openai",
</strong><strong>    api_key="PORTKEY_API_KEY"
</strong><strong>)
</strong>
<strong>llm = OpenAI(base_url=PORTKEY_GATEWAY_URL,default_headers=portkeyHeaders)
</strong><strong>chat = ChatOpenAI(base_url=PORTKEY_GATEWAY_URL,default_headers=portkeyHeaders)
</strong>
llm.predict("hi!")
>>> "Hi"

chat.predict("hi!")
>>> "Hi"
</code></pre>

## Langchain Concepts

### Prompt Templates & Output Parsers

Langchain supports [Prompt Templates](https://python.langchain.com/docs/modules/model\_io/prompts/) for constructing inputs for language models and [Output Parsers](https://python.langchain.com/docs/modules/model\_io/output\_parsers/) for customizing the models' outputs to your preferred format.

```python
from portkey import PortkeyChat
from langchain.prompts.chat import ChatPromptTemplate
from langchain.schema import BaseOutputParser

portkeyChatModel = PortkeyChat(api_key="...", virtual_key="...)

class CommaSeparatedListOutputParser(BaseOutputParser):
    """Parse the output of an LLM call to a comma-separated list."""


    def parse(self, text: str):
        """Parse the output of an LLM call."""
        return text.strip().split(", ")

template = """You are a helpful assistant who generates comma separated lists.
A user will pass in a category, and you should generate 5 objects in that category in a comma separated list.
ONLY return a comma separated list, and nothing more."""
human_template = "{text}"

chat_prompt = ChatPromptTemplate.from_messages([
    ("system", template),
    ("human", human_template),
])
chain = chat_prompt | portkeyChatModel | CommaSeparatedListOutputParser()
chain.invoke({"text": "colors"})
# >> ['red', 'blue', 'green', 'yellow', 'orange']
```

### Using Chains

[Chains](https://python.langchain.com/docs/modules/chains/) enable the integration of various Langchain concepts for simultaneous execution. Incorporating Portkey is straightforward: simply use the `PortkeyLLM` or `PortkeyChat` model interfaces in your chain.

The Portkey custom models are supported fully across Langchain chains. Additionally, they offer the flexibility to select different models within a chain by swapping out the [virtual key ](../../product/ai-gateway-streamline-llm-integrations/virtual-keys.md)as needed.. Using Portkey [configs](../../product/ai-gateway-streamline-llm-integrations/configs.md) also facilitates load balancing, fallback mechanisms, and more within your Langchain chains.
