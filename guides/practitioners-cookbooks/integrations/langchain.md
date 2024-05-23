# Langchain

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1-EETdhw2RrOCrsmHZP6P7LzSDMsvsJeu?usp=sharing)

## Portkey + Langchain

[Portkey](https://app.portkey.ai/) is the Control Panel for AI apps. With it's popular AI Gateway and Observability Suite, hundreds of teams ship reliable, cost-efficient, and fast apps.

Portkey brings production readiness to Langchain. With Portkey, you can

* Connect to 150+ models through a unified API,
* View 42+ metrics & logs for all requests,
* Enable semantic cache to reduce latency & costs,
* Implement automatic retries & fallbacks for failed requests,
* Add custom tags to requests for better tracking and analysis and more.

### Quickstart

Since Portkey is fully compatible with the OpenAI signature, you can connect to the Portkey AI Gateway through the ChatOpenAI interface.

* Set the `base_url` as `PORTKEY_GATEWAY_URL`
* Add `default_headers` to consume the headers needed by Portkey using the `createHeaders` helper method.

To start, get your Portkey API key by signing up [here](https://app.portkey.ai/). (Click the profile icon on the bottom left, then click on "Copy API Key")

```python
!pip install -qU portkey-ai langchain-openai
```

We can now connect to the Portkey AI Gateway by updating the `ChatOpenAI` model in Langchain

#### Using OpenAI models with Portkey + ChatOpenAI

```python
from langchain_openai import ChatOpenAI
from portkey_ai import createHeaders, PORTKEY_GATEWAY_URL
from google.colab import userdata

portkey_headers = createHeaders(api_key= userdata.get("PORTKEY_API_KEY"), ## Grab from https://app.portkey.ai/
                                provider="openai"
                                )

llm = ChatOpenAI(api_key= userdata.get("OPENAI_API_KEY"),
                 base_url=PORTKEY_GATEWAY_URL,
                 default_headers=portkey_headers)

llm.invoke("What is the meaning of life, universe and everything?")
```

#### Using Together AI models with Portkey + ChatOpenAI

```python
from langchain_openai import ChatOpenAI
from portkey_ai import createHeaders, PORTKEY_GATEWAY_URL
from google.colab import userdata

portkey_headers = createHeaders(api_key= userdata.get("PORTKEY_API_KEY"), ## Grab from https://app.portkey.ai/
                                provider="together-ai"
                                )

llm = ChatOpenAI(model = "meta-llama/Llama-3-8b-chat-hf",
                 api_key= userdata.get("TOGETHER_API_KEY"), ## Replace it with your provider key
                 base_url=PORTKEY_GATEWAY_URL,
                 default_headers=portkey_headers)

llm.invoke("What is the meaning of life, universe and everything?")
```

### Advanced Routing - Load Balancing, Fallbacks, Retries

The Portkey AI Gateway brings capabilities like load-balancing, fallbacks, experimentation and canary testing to Langchain through a configuration-first approach.

Let's take an example where we might want to split traffic between `llama-3-70b` and `gpt-3.5` 50:50 to test the two large models. The gateway configuration for this would look like the following:

```python
config = {
    "strategy": {
         "mode": "loadbalance"
    },
    "targets": [{
        "virtual_key": "gpt3-8070a6", # OpenAI's virtual key
        "override_params": {"model": "gpt-3.5-turbo"},
        "weight": 0.5
    }, {
        "virtual_key": "together-1c20e9", # Together's virtual key
        "override_params": {"model": "meta-llama/Llama-3-8b-chat-hf"},
        "weight": 0.5
    }]
}
```

```python
from langchain_openai import ChatOpenAI
from portkey_ai import createHeaders, PORTKEY_GATEWAY_URL
from google.colab import userdata

portkey_headers = createHeaders(
    api_key= userdata.get("PORTKEY_API_KEY"),
    config=config
)

llm = ChatOpenAI(api_key="X", base_url=PORTKEY_GATEWAY_URL, default_headers=portkey_headers)

llm.invoke("What is the meaning of life, universe and everything?")
```

```python
```
