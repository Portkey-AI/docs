---
description: Integrate Portkey and analyze your first LLM call in 2 minutes!
---

# Make Your First Request

## 1. Get your Portkey API Key

[Create](https://app.portkey.ai/signup) or [log in](https://app.portkey.ai/login) to your Portkey account. Grab your account's API key in the dropdown menu on your profile icon.

<div align="left" data-full-width="false">

<figure><img src="https://3798672042-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FeWEp2XRBGxs7C1jgAdk7%2Fuploads%2Fa7JLqhFhjaOinZsj1oxq%2Fportkey_api.gif?alt=media&#x26;token=e2025311-62ba-479f-859c-de5d55de9536" alt="" width="563"><figcaption><p>Copy your Portkey account API key</p></figcaption></figure>

</div>

## 2. Integrate Portkey

Portkey offers a variety of integration options, including SDKs, REST APIs, and native connections with platforms like OpenAI, Langchain, and LlamaIndex, among others.&#x20;

### Through the OpenAI SDK

If you're using the **OpenAI SDK**, import the Portkey SDK and configure it within your OpenAI client object:

{% content-ref url="integration-guides/openai.md" %}
[openai.md](integration-guides/openai.md)
{% endcontent-ref %}

### Portkey SDK

You can also use the **Portkey SDK / REST APIs** directly to make the chat completion calls. This is a more versatile way to make LLM calls across any provider:

{% content-ref url="../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

Once, the integration is ready, you can view the requests reflect on your Portkey dashboard.

<figure><img src="../.gitbook/assets/analytics_logs (1).gif" alt=""><figcaption></figcaption></figure>

### Other Integration Guides

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><h4>Azure OpenAI</h4></td><td><a href="integration-guides/azure-openai.md">azure-openai.md</a></td></tr><tr><td><h4>Anthropic</h4></td><td><a href="integration-guides/anthropic.md">anthropic.md</a></td></tr><tr><td><h4>Langchain</h4></td><td><a href="integration-guides/langchain-python.md">langchain-python.md</a></td></tr><tr><td><h4>LlamaIndex</h4></td><td><a href="integration-guides/llama-index-python.md">llama-index-python.md</a></td></tr><tr><td><h4>Cohere</h4></td><td><a href="integration-guides/cohere.md">cohere.md</a></td></tr><tr><td><h4>Others</h4></td><td><a href="../api-reference/gateway-for-other-apis.md">gateway-for-other-apis.md</a></td></tr></tbody></table>

## 3. Next Steps

Now that you're up and running with Portkey, you can dive into the various Portkey features to learn about all of the supported functionalities:

<table data-view="cards"><thead><tr><th></th><th data-hidden data-type="content-ref"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>Observability</td><td><a href="../product/observability-modern-monitoring-for-llms/">observability-modern-monitoring-for-llms</a></td><td></td></tr><tr><td>AI Gateway</td><td></td><td><a href="../product/ai-gateway-streamline-llm-integrations/">ai-gateway-streamline-llm-integrations</a></td></tr><tr><td>Prompt Library</td><td></td><td><a href="../product/prompt-library.md">prompt-library.md</a></td></tr><tr><td>Autonomous Fine-Tuning</td><td></td><td><a href="../product/autonomous-fine-tuning.md">autonomous-fine-tuning.md</a></td></tr></tbody></table>

***

## FAQs

### Will Portkey increase the latency of my API requests?

Portkey is hosted on edge workers throughout the world and our servers ensure the least latency roundtrips. Our benchmarks estimate a total latency addition between 20-40ms.

Our edge worker locations:

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt="" width="375"><figcaption></figcaption></figure>

### Is my data secure?

Portkey is ISO:27001 and SOC 2 certified. We're also GDPR compliant. This is proof that we maintain the best practices involving security of our services, data storage and retrieval. All your data is encrypted in transit and at rest.

If you're still worried about your data passing through Portkey, we recommend one of the below options:

1. On request, we can enable a feature that does NOT store any of your request and response boby objects in the Portkey datastores or our logs.
2. For enterprises, we offer managed hosting to deploy Portkey inside private clouds.

If you need to talk about these options, feel free to drop us a note on hello@portkey.ai

### Will Portkey scale if my app explodes?

Portkey has been tested to handle millions of requests per second. We serve over 10M requests everyday with a 99.99% uptime. We're built of top of scalable infrastructure and can handle huge loads without breaking a sweat.

[**View our Status Page**](https://status.portkey.ai)

### Does Portkey impose timeouts on requests?

We do not impose **any explicit timeout** for our free OR paid plans currently. In the past, we have had users experience timeouts from various other frameworks, but Portkey does not time out requests on our end.

### Do you support sign ups from non-google/gmail accounts?

Yes! We support registrations with Microsoft accounts - this is currently in beta. Please reach out on support@portkey.ai or [request on Discord](https://discord.gg/kXYKpPGasJ) for access to MS login.

### Where can I reach you you?

We're available all the time on [Discord](https://discord.gg/DD7vgKK299), or on our support email - support@portkey.ai
