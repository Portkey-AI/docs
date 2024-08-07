---
description: Portkey helps bring your agents to production!
---

# Agents

<table data-column-title-hidden data-view="cards"><thead><tr><th align="center"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td align="center"><p><img src="../../.gitbook/assets/Autogen Image.jpeg" alt=""></p><p><strong>Autogen</strong></p></td><td><a href="autogen.md">autogen.md</a></td></tr><tr><td align="center"><p><img src="../../.gitbook/assets/CrewAI logo.jpg" alt=""></p><p><strong>CrewAI</strong></p></td><td><a href="crewai.md">crewai.md</a></td></tr><tr><td align="center"><p><img src="../../.gitbook/assets/Phidata Square Image.jpeg" alt=""></p><p><strong>Phidata</strong></p></td><td><a href="phidata.md">phidata.md</a></td></tr><tr><td align="center"><p><img src="../../.gitbook/assets/Llamaindex.ai image.jpeg" alt=""></p><p><strong>Llama Index</strong></p></td><td><a href="llama-agents.md">llama-agents.md</a></td></tr><tr><td align="center"><p><img src="../../.gitbook/assets/Prefect Community Logo.png" alt=""></p><p><strong>Control Flow</strong></p></td><td><a href="control-flow.md">control-flow.md</a></td></tr><tr><td align="center"><p><img src="../../.gitbook/assets/Robot Emoji Icon large.webp" alt=""></p><p><strong>Bring Your Agent</strong></p></td><td><a href="bring-your-own-agents.md">bring-your-own-agents.md</a></td></tr></tbody></table>

## Integrate Portkey with your agents with just 2 lines of code

{% tabs %}
{% tab title="Langchain" %}
<pre class="language-python"><code class="lang-python">from langchain_openai import ChatOpenAI
from portkey_ai import createHeaders, PORTKEY_GATEWAY_URL

llm = ChatOpenAI(
    api_key="OpenAI_API_Key",
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        provider="openai", #choose your provider
</strong><strong>        api_key="PORTKEY_API_KEY"
</strong><strong>    )
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

### Get Started with Portkey x Agent Cookbooks

* [Autogen](https://dub.sh/Autogen-docs)
* [CrewAI](https://git.new/crewAI-docs)
* [Phidata](https://dub.sh/Phidata-docs)
* [Llama Index ](https://git.new/llama-agents)
* [Control Flow](https://dub.sh/Control-Flow-docs)

***

## Key Production Features

By routing your agent's requests through Portkey, you make your agents production-grade with the following features.

### 1. [Interoperability](../../product/ai-gateway-streamline-llm-integrations/universal-api.md)

Easily switch between LLM providers. Call various LLMs such as Anthropic, Gemini, Mistral, Azure OpenAI, Google Vertex AI,  AWS Bedrock and much more by simply changing the  `provider` and `API key` in the LLM object.

### 2. [Caching](../../product/ai-gateway-streamline-llm-integrations/cache-simple-and-semantic.md)

Improve performance and reduce costs on your Agent's LLM calls by storing past responses in the Portkey cache. Choose between Simple and Semantic cache modes in your Portkey's gateway config.

```json
{
 "cache": {
    "mode": "semantic" // Choose between "simple" or "semantic"
 }
}
```

### 3. [Reliability](../../product/ai-gateway-streamline-llm-integrations/)

Set up **fallbacks** between different LLMs or providers, **load balance** your requests across multiple instances or API keys, set **automatic retries**, and **request timeouts.** Ensure your agents' resilience with advanced reliability features.

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

### 4. [Observability](../../product/observability-modern-monitoring-for-llms/)

Portkey automatically logs key details about your agent runs, including cost, tokens used, response time, etc. For agent-specific observability, add Trace IDs to the request headers for each agent. This enables filtering analytics by Trace IDs, ensuring deeper monitoring and analysis.

### 5. [Logs](../../product/observability-modern-monitoring-for-llms/logs.md)

Access a dedicated section to view records of action executions, including parameters, outcomes, and errors. Filter logs of your agent run based on multiple parameters such as trace ID, model, tokens used, metadata, etc.

<figure><img src="../../.gitbook/assets/222.gif" alt=""><figcaption></figcaption></figure>

### 6. [Prompt Management](../../product/prompt-library.md)

Use Portkey as a centralized hub to store, version, and experiment with your agent's prompts across multiple LLMs. Easily modify your prompts and run A/B tests without worrying about the breaking prod.

### 7. [Continuous Improvement](../../product/observability-modern-monitoring-for-llms/feedback.md)

Improve your Agent runs by capturing qualitative & quantitative user feedback on your requests, and then using that feedback to make your prompts AND LLMs themselves better.

### 8. [Security & Compliance](../../product/enterprise-offering/security-portkey.md)

Set budget limits on provider API keys and implement fine-grained user roles and permissions for both the app and the Portkey APIs.
