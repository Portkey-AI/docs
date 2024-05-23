# Deepinfra

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1SiyWV8ER-Gp2GEkMr9aA3KhebdeEJHBK?usp=sharing)

## Portkey + DeepInfra

[Portkey](https://app.portkey.ai/) is the Control Panel for AI apps. With it's popular AI Gateway and Observability Suite, hundreds of teams ship reliable, cost-efficient, and fast apps.

With Portkey, you can

* Connect to 150+ models through a unified API,
* View 40+ metrics & logs for all requests,
* Enable semantic cache to reduce latency & costs,
* Implement automatic retries & fallbacks for failed requests,
* Add custom tags to requests for better tracking and analysis and more.

### Quickstart

Since Portkey is fully compatible with the OpenAI signature, you can connect to the Portkey AI Gateway through OpenAI Client.

* Set the `base_url` as `PORTKEY_GATEWAY_URL`
* Add `default_headers` to consume the headers needed by Portkey using the `createHeaders` helper method.

You will need Portkey and Deepinfra API keys to run this notebook.

* Sign up for Portkey and generate your API key [here](https://app.portkey.ai/).
* Get your Deepinfra key [here](https://deepinfra.com/dash/api\_keys)

```python
!pip install -qU portkey-ai openai
```

### With OpenAI Client

```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
from google.colab import userdata

client = OpenAI(
    api_key= userdata.get('DEEPINFRA_API_KEY'), ## replace it your Mistral API key
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="deepinfra",
        api_key= userdata.get('PORTKEY_API_KEY'), ## replace it your Portkey API key
    )
)

chat_complete = client.chat.completions.create(
    model="meta-llama/Meta-Llama-3-70B-Instruct",
    messages=[{"role": "user",
               "content": "Who are you?"}],
)

print(chat_complete.choices[0].message.content)
```

```
Nice to meet you! I'm LLaMA, a large language model trained by a team of researcher at Meta AI. My primary function is to generate human-like responses to a wide range of questions and topics, from science and history to entertainment and culture.

I'm not a human, but rather an artificial intelligence designed to simulate conversation and answer questions to the best of my knowledge. I've been trained on a massive dataset of text from the internet and can respond in multiple languages.

I can help with things like:

* Answering questions on a variety of topics
* Generating text on a given topic or subject
* Translating text from one language to another
* Summarizing long pieces of text into shorter, more digestible versions
* Offering suggestions or ideas for creative projects
* Even just having a conversation and chatting about your day or interests!

So, what's on your mind? Want to chat about something specific or just see where the conversation takes us?
```

### Observability with Portkey

By routing requests through Portkey you can track a number of metrics like - tokens used, latency, cost, etc.

Here's a screenshot of the dashboard you get with Portkey:

<figure><img src="../../../.gitbook/assets/CleanShot 2024-05-23 at 10.03.58@2x.png" alt=""><figcaption></figcaption></figure>
