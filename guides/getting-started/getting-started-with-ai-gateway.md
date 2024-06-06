# Getting started with AI Gateway

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1nQa-9EYcv9-O6VnwLATnVd9Q2wFtthOA?usp=sharing)

[Portkey](https://app.portkey.ai/) is the Control Panel for AI apps. With it's popular AI Gateway and Observability Suite, hundreds of teams ship reliable, cost-efficient, and fast apps.

With Portkey, you can

* Connect to 150+ models through a unified API,
* View 40+ metrics & logs for all requests,
* Enable semantic cache to reduce latency & costs,
* Implement automatic retries & fallbacks for failed requests,
* Add custom tags to requests for better tracking and analysis and more.

## Quickstart

Since Portkey is fully compatible with the OpenAI signature, you can connect to the Portkey AI Gateway through OpenAI Client.

* Set the `base_url` as `PORTKEY_GATEWAY_URL`
* Add `default_headers` to consume the headers needed by Portkey using the `createHeaders` helper method.

```python
!pip install -qU portkey-ai openai
```

```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
from google.colab import userdata
```

## OpenAI

```python
client = OpenAI(
    api_key=OPENAI_API_KEY,
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key=PORTKEY_API_KEY
    )
)

chat_complete = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user",
               "content": "What's a fractal?"}],
)

print(chat_complete.choices[0].message.content)
```

```
A fractal is a complex geometric shape that can be split into parts, each of which is a reduced-scale copy of the whole. Fractals are typically self-similar and independent of scale, meaning they look similar at any zoom level. They often appear in nature, in things like snowflakes, coastlines, and fern leaves. The term "fractal" was coined by mathematician Benoit Mandelbrot in 1975.
```

## Anthropic

```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    api_key=userdata.get('ANTHROPIC_API_KEY')
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="anthropic",
        api_key=PORTKEY_API_KEY
    ),
)

response = client.chat.completions.create(
    model="claude-3-opus-20240229",
    messages=[{"role": "user",
               "content": "What's a fractal?"}],
    max_tokens= 512
)
```

## Mistral AI

```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    api_key=userdata.get('MISTRAL_API_KEY'),
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="mistral-ai",
        api_key=PORTKEY_API_KEY
    )
)

chat_complete = client.chat.completions.create(
    model="mistral-medium",
    messages=[{"role": "user",
               "content": "What's a fractal?"}],
)

print(chat_complete.choices[0].message.content)
```

```
A fractal is a geometric shape or pattern that exhibits self-similarity at different scales. This means that the shape appears similar or identical when viewed at different levels of magnification. Fractals are often complex and intricate, and they can be generated mathematically using iterative algorithms. They are commonly found in nature, such as in the branching patterns of trees and the shapes of coastlines. Fractals have applications in various fields, including mathematics, physics, and computer graphics. Some famous examples of fractals include the Mandelbrot set and the Sierpinski triangle.
```

## Together AI

```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    api_key=userdata.get('TOGETHER_API_KEY'),
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="together-ai",
        api_key=PORTKEY_API_KEY
    )
)

chat_complete = client.chat.completions.create(
    model="meta-llama/Llama-2-70b-hf",
    messages=[{"role": "user",
               "content": "What's a fractal?"}],
)

print(chat_complete.choices[0].message.content)
```

```
<|im_start|>user
A fractal is a never ending pattern. Fractals are infinitely complex patterns that are self-similar across different scales. They are created by repeating a simple process over and over in an ongoing feedback loop. Driven by recursion, fractals are images of dynamic systems – the pictures of Chaos. Geometrically, they exist in between our familiar dimensions. Fractal patterns are extremely familiar, since nature is full of fractals. For instance: trees, rivers, coastlines, mountains, clouds, seashells, hurricanes, etc
```
