---
description: Use Portkey with Autogen to take your AI Agents to production
---

# Autogen

## Getting Started

### 1. Install the required packages:

```python
!pip install -qU pyautogen portkey-ai
```

### **2.** Configure your Autogen configs:

```python
from autogen import AssistantAgent, UserProxyAgent, config_list_from_json
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

config = [
    {
        "api_key": "OPENAI_API_KEY",
        "model": "gpt-3.5-turbo",
        "base_url": PORTKEY_GATEWAY_URL,
        "api_type": "openai",
        "default_headers": createHeaders(
            api_key ="PORTKEY_API_KEY", #Replace with Your Portkey API key
            provider = "openai",
        )
    }
]
```

## Integration Guide

Here's a simple Google Colab notebook that demonstrates Autogen with Portkey integration

[![Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://dub.sh/Autogen-docs)

## Make your agents Production-ready with Portkey

Portkey makes your Autogen agents reliable, robust, and production-grade with its observability suite and AI Gateway. Seamlessly integrate 200+ LLMs with your Autogen agents using Portkey. Implement fallbacks, gain granular insights into agent performance and costs, and continuously optimize your AI operations—all with just 2 lines of code.

Let's dive deep! Let's go through each of the use cases!

### 1.[ Interoperability](../../product/ai-gateway-streamline-llm-integrations/universal-api.md)

Easily switch between 200+ LLMs. Call various LLMs such as Anthropic, Gemini, Mistral, Azure OpenAI, Google Vertex AI,  AWS Bedrock, and many more by simply changing the  `provider` and `API key` in the `ChatOpenAI` object.

{% tabs %}
{% tab title="OpenAI to Azure OpenAI" %}
If you are using OpenAI with autogen, your code would look like this:

```python
config = [
    {
        "api_key": "OPENAI_API_KEY",
        "model": "gpt-3.5-turbo",
        "base_url": PORTKEY_GATEWAY_URL,
        "api_type": "openai",
        "default_headers": createHeaders(
            api_key ="PORTKEY_API_KEY", #Replace with Your Portkey API key
            provider = "openai",
        )
    }
]
```

To switch to Azure as your provider, add your Azure details to Portley vault ([here's how](../integration-guides/azure-openai.md)) and use Azure OpenAI using virtual keys

```python
config = [
    {
        "api_key": "api-key",
        "model": "gpt-3.5-turbo",
        "base_url": PORTKEY_GATEWAY_URL,
        "api_type": "openai",
        "default_headers": createHeaders(
            api_key ="PORTKEY_API_KEY", #Replace with Your Portkey API key
            provider = "azure-openai",
            virtual_key="AZURE_VIRTUAL_KEY"   # Replace with Azure Virtual Key
        )
    }
]

```
{% endtab %}

{% tab title="Anthropic to AWS Bedrock" %}
If you are using Anthropic with CrewAI, your code would look like this:

```
llm = ChatOpenAI(
    api_key="OpenAI_API_Key",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai", #choose your provider
        api_key="PORTKEY_API_KEY"
    )
)
```

To switch to AWS Bedrock as your provider, add your AWS Bedrock details to Portley vault ([here's how](../integration-guides/aws-bedrock.md)) and use AWS Bedrock using virtual keys,

```
llm = ChatOpenAI(
    api_key="api-key",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="azure-openai", #choose your provider
        api_key="PORTKEY_API_KEY",  
        virtual_key="AZURE_VIRTUAL_KEY"   # Replace with your virtual key for Azure
    )
)
```
{% endtab %}
{% endtabs %}

### 2. [Reliability](../../product/ai-gateway-streamline-llm-integrations/)

Agents are _brittle_. Long agentic pipelines with multiple steps can fail at any stage, disrupting the entire process. Portkey solves this by offering built-in **fallbacks** between different LLMs or providers, **load-balancing** across multiple instances or API keys, and implementing automatic **retries** and request **timeouts**. This makes your agents more reliable and resilient.

Here's how you can implement these features using Portkey's config

```json
{
  "retry": {
    "attempts": 5
  },
  "strategy": {
    "mode": "loadbalance" // Choose between "loadbalance" or "fallback"
  },
  "targets": [
    {
      "provider": "openai",
      "api_key": "OpenAI_API_Key"
    },
    {
      "provider": "anthropic",
      "api_key": "Anthropic_API_Key"
    }
  ]
}
```

### 4. [Metrics](../../product/observability-modern-monitoring-for-llms/)

Agent runs can be costly. Tracking agent metrics is crucial for understanding the performance and reliability of your AI agents. Metrics help identify issues, optimize runs, and ensure that your agents meet their intended goals.

Portkey automatically logs comprehensive metrics for your AI agents, including **cost**, **tokens used**, **latency**, etc. Whether you need a broad overview or granular insights into your agent runs, Portkey's customizable filters provide the metrics you need.\
For agent-specific observability, add `Trace-id` to the request headers for each agent.&#x20;

```python
config = [
    {
        "api_key": "OPENAI_API_KEY",
        "model": "gpt-3.5-turbo",
        "base_url": PORTKEY_GATEWAY_URL,
        "api_type": "openai",
        "default_headers": createHeaders(
            api_key ="PORTKEY_API_KEY", #Replace with your Portkey API key
            provider = "openai",
            trace_id="research_agent1" #Add individual trace-id for your agent
        )
    }
]
```

### 5. [Logs](../../product/observability-modern-monitoring-for-llms/logs.md)

Agent runs are complex. Logs are essential for diagnosing issues, understanding agent behavior, and improving performance. They provide a detailed record of agent activities and tool use, which is crucial for debugging and optimizing processes.

Portkey offers comprehensive logging features that capture detailed information about every action and decision made by your AI agents. Access a dedicated section to view records of agent executions, including parameters, outcomes, function calls, and errors. Filter logs based on multiple parameters such as trace ID, model, tokens used, and metadata.

<figure><img src="../../.gitbook/assets/222.gif" alt=""><figcaption></figcaption></figure>

### 7. [Continuous Improvement](../../product/observability-modern-monitoring-for-llms/feedback.md)

Improve your Agent runs by capturing qualitative & quantitative user feedback on your requests.\
Portkey's Feedback APIs provide a simple way to get weighted feedback from customers on any request you served, at any stage in your app. You can capture this feedback on a request or conversation level and analyze it by adding meta data to the relevant request.

### 8. [Caching](../../product/ai-gateway-streamline-llm-integrations/cache-simple-and-semantic.md)

Agent runs are time-consuming and expensive due to their complex pipelines. Caching can significantly reduce these costs by storing frequently used data and responses.\
Portkey offers a built-in caching system that stores past responses, reducing the need for agent calls saving both time and money.

```json
{
 "cache": {
    "mode": "semantic" // Choose between "simple" or "semantic"
 }
}
```

### 9. [Security & Compliance](../../product/enterprise-offering/security-portkey.md)

Set budget limits on provider API keys and implement fine-grained user roles and permissions for both the app and the Portkey APIs.

***

## [Portkey Config](../../product/ai-gateway-streamline-llm-integrations/configs.md)

Many of these features are driven by Portkey's Config architecture. The Portkey app simplifies creating, managing, and versioning your Configs.

For more information on using these features and setting up your Config, please refer to the [Portkey documentation](https://docs.portkey.ai).
