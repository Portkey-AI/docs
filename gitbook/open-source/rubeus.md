# Rubeus

**Rubeus** streamlines API requests to 20+ LLMs. It provides a unified API signature for interacting with all LLMs alongwith powerful LLM Gateway features like load balancing, fallbacks, retries and more.

[**For more details, head over to the Github repo.**](https://github.com/portkey-ai/rubeus)

***

### Features

* 🌐 **Interoperability:** Write once, run with any provider. Switch between \_\_ models from \_\_ providers seamlessly.
* 🔀 **Fallback Strategies:** Don't let failures stop you. If one provider fails, Rubeus can automatically switch to another.
* 🔄 **Retry Strategies:** Temporary issues shouldn't mean manual re-runs. Rubeus can automatically retry failed requests.
* ⚖️ **Load Balancing:** Distribute load effectively across multiple API keys or providers based on custom weights.
* 📝 **Unified API Signature:** If you've used OpenAI, you already know how to use Rubeus with any other provider.\
  \


### Supported Providers

|                                                                                                                                                                                         | Provider     | Support Status | Supported Endpoints     |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | -------------- | ----------------------- |
| [<img src="https://github.com/Portkey-AI/Rubeus/raw/main/docs/images/openai.png" alt="" data-size="line">](https://github.com/Portkey-AI/Rubeus/blob/main/docs/images/openai.png)       | OpenAI       | ✅ Supported    | `/completion`, `/embed` |
| [<img src="https://github.com/Portkey-AI/Rubeus/raw/main/docs/images/azure.png" alt="" data-size="line">](https://github.com/Portkey-AI/Rubeus/blob/main/docs/images/azure.png)         | Azure OpenAI | ✅ Supported    | `/completion`, `/embed` |
| [<img src="https://github.com/Portkey-AI/Rubeus/raw/main/docs/images/anthropic.png" alt="" data-size="line">](https://github.com/Portkey-AI/Rubeus/blob/main/docs/images/anthropic.png) | Anthropic    | ✅ Supported    | `/complete`             |
| [<img src="https://github.com/Portkey-AI/Rubeus/raw/main/docs/images/cohere.png" alt="" data-size="line">](https://github.com/Portkey-AI/Rubeus/blob/main/docs/images/cohere.png)       | Cohere       | ✅ Supported    | `generate`, `embed`     |
| [<img src="https://github.com/Portkey-AI/Rubeus/raw/main/docs/images/localai.png" alt="" data-size="line">](https://github.com/Portkey-AI/Rubeus/blob/main/docs/images/localai.png)     | LocalAI      | 🚧 Coming Soon |                         |

### Getting Started

```bash
npm install
npm run dev # To run locally
npm run deploy # To deploy to cloudflare
```

### Usage

#### 🌐 Interoperability

Rubeus allows you to switch between different language learning models from various providers, making it a highly flexible tool. The following example shows a request to `openai`, but you could change the provider name to `cohere`, `anthropic` or others and Rubeus will automatically handle everything else.

```bash
curl --location 'http://127.0.0.1:8787/v1/complete' \
--header 'Content-Type: application/json' \
--data-raw '{
    "config": {
        "provider": "openai",
        "api_key: "<open-ai-api-key-here>"
    },
    "params": {
        "prompt": "What are the top 10 happiest countries in the world?",
        "max_tokens": 50,
        "model": "text-davinci-003",
        "user": "jbu3470"
    }
}'
```

#### 🔀 Fallback Strategies

In case one provider fails, Rubeus is designed to automatically switch to another, ensuring uninterrupted service.

```sh
# Fallback to anthropic, if openai fails (This API will use the default text-davinci-003 and claude-v1 models)
curl --location 'http://127.0.0.1:8787/v1/complete' \
--header 'Content-Type: application/json' \
--data-raw '{
    "config": {
        "mode": "fallback",
        "options": [
          {
            "provider": "openai",
            "api_key": "<open-ai-api-key-here>"
          }, 
          {
            "provider": "anthropic",
            "api_key": "<anthropic-api-key-here>"
          }
        ]
    },
    "params": {
        "prompt": "What are the top 10 happiest countries in the world?",
        "max_tokens": 50,
        "user": "jbu3470"
    }
}'

# Fallback to gpt-3.5-turbo when gpt-4 fails
curl --location 'http://127.0.0.1:8787/v1/chatComplete' \
--header 'Content-Type: application/json' \
--data-raw '{
    "config": {
        "mode": "fallback",
        "options": [
          {
            "provider": "openai", 
            "override_params": {"model": "gpt-4"},
            "api_key": "<open-ai-api-key-here>" 
          }, 
          {
            "provider": "openai", 
            "override_params": {"model": "gpt-3.5-turbo"},
            "api_key": "<open-ai-api-key-here>"
          }
        ]
    },
    "params": {
        "messages": [{"role": "user", "content": "What are the top 10 happiest countries in the world?"}],
        "max_tokens": 50,
        "user": "jbu3470"
    }
}'
```

#### 🔄 Retry Strategies

Rubeus has a built-in mechanism to retry failed requests, eliminating the need for manual re-runs.

```sh
# Add the retry configuration to enable exponential back-off retries
curl --location 'http://127.0.0.1:8787/v1/complete' \
--header 'Content-Type: application/json' \
--data-raw '{
    "config": {
        "mode": "single",
        "options": [{
            "provider": "openai",
            "retry": {
                "attempts": 3,
                "on_status_codes": [429,500,504,524]
            },
            "api_key": "<open-ai-api-key-here>"
        }]
    },
    "params": {
        "prompt": "What are the top 10 happiest countries in the world?",
        "max_tokens": 50,
        "model": "text-davinci-003",
        "user": "jbu3470"
    }
}'
```

#### ⚖️ Load Balancing

Manage your workload effectively with Rubeus's custom weight-based distribution across multiple API keys or providers.

```sh
# Load balance 50-50 between gpt-3.5-turbo and claude-v1
curl --location 'http://127.0.0.1:8787/v1/chatComplete' \
--header 'Content-Type: application/json' \
--data '{
    "config": {
        "mode": "loadbalance",
        "options": [{
            "provider": "openai",
            "weight": 0.5,
            "override_params": { "model": "gpt-3.5-turbo" },
            "api_key": "<open-ai-api-key-here>"
        }, {
            "provider": "anthropic",
            "weight": 0.5,
            "override_params": { "model": "claude-v1" },
            "api_key": "<anthropic-api-key-here>"
        }]
    },
    "params": {
        "messages": [{"role": "user","content":"What are the top 10 happiest countries in the world?"}],
        "max_tokens": 50,
        "user": "jbu3470"
    }
}'
```

#### 📝 Unified API Signature

If you're familiar with OpenAI's API, you'll find Rubeus's API easy to use due to its unified signature.

```sh
# OpenAI query
curl --location 'http://127.0.0.1:8787/v1/complete' \
--header 'Content-Type: application/json' \
--data-raw '{
    "config": {
        "provider": "openai",
        "api_key": "<open-ai-api-key-here>"
    },
    "params": {
        "prompt": "What are the top 10 happiest countries in the world?",
        "max_tokens": 50,
        "user": "jbu3470"
    }
}'

# Anthropic Query
curl --location 'http://127.0.0.1:8787/v1/complete' \
--header 'Content-Type: application/json' \
--data-raw '{
    "config": {
        "provider": "anthropic",
        "api_key": "<anthropic-api-key-here>"
    },
    "params": {
        "prompt": "What are the top 10 happiest countries in the world?",
        "max_tokens": 50,
        "user": "jbu3470"
    }
}'
```
