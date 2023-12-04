# Llama Index

**LlamaIndex** is a data framework designed for Large Language Models (LLMs) to ingest, structure, and access private or domain-specific data. It provides tools for beginners and advanced users to ingest data from various sources such as APIs, databases, PDFs, and more, and then index it into representations optimized for LLMs. This allows natural language querying and conversation with the data via query engines, chat interfaces, and LLM-powered data agents. LlamaIndex is available in Python and offers both high-level and lower-level APIs for ease of use and customization.

The Portkey - LlamaIndex integration also for monitoring, debugging, and testing of the applications, making it easier to build production-ready LlamaIndex apps.

## Quick Start Integration

### ⭐️ Using the Portkey custom models

Portkey is available as a [Custom LLM in LlamaIndex](https://docs.llamaindex.ai/en/stable/getting\_started/customization.html) making the integration simpler while allowing you to connect to multiple LLM providers (Anthropic, Azure, Huggingface, Anyscale, etc) through our powerful AI gateway.

Here's an example:

```python
# Setup a custom service context by passing in the Portkey LLM
from llama_index import ServiceContext
from llama_index.llms.llama_index import PortkeyChat

portkey = PortkeyChat(api_key="PORTKEY_API_KEY", virtual_key="VIRTUAL_KEY")
service_context = ServiceContext.from_defaults(llm=portkey)
```

<pre class="language-python"><code class="lang-python"># Use this service context in the query engine
from llama_index import VectorStoreIndex, SimpleDirectoryReader

documents = SimpleDirectoryReader("data").load_data()
index = VectorStoreIndex.from_documents(documents)
<strong>query_engine = index.as_query_engine(service_context=service_context)
</strong>response = query_engine.query("What did the author do growing up?")
print(response)
</code></pre>

You can read more about custom LLM models in LlamaIndex [here](https://docs.llamaindex.ai/en/stable/module\_guides/models/llms/usage\_custom.html).



### Using LlamaIndex's OpenAI model

To integrate Portkey with your existing models, you need to

* Update OpenAI's baseURL to the Portkey Gateway
* And add Portkey's headers

The same example above but using the OpenAI model would look like this:

<pre><code>from llama_index import (
    KeywordTableIndex,
    SimpleDirectoryReader,
    LLMPredictor,
    ServiceContext,
)
from llama_index.llms import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
documents = SimpleDirectoryReader("data").load_data()

# define LLM with Portkey abstractions
<strong>headers = createHeaders(api_key="PORTKEY_API_KEY", mode="openai")
</strong><strong>llm = OpenAI(api_base=PORTKEY_GATEWAY_URL, headers=headers)
</strong>service_context = ServiceContext.from_defaults(llm=llm)

# build index
index = KeywordTableIndex.from_documents(
    documents, service_context=service_context
)

# get response from query
query_engine = index.as_query_engine()
response = query_engine.query(
    "What did the author do after his time at Y Combinator?"
)
</code></pre>

