---
description: Production-grade features to bring your LLMs to production, easily!
---

# ✨ Features

Portkey serves as a strategic gateway between your app and LLM providers, enhancing existing workflows with production capabilities without compromising on speed. With Portkey, we've natively built:

1. An **AI Gateway**
2. An **Observability** layer
3. A **Live Feedback** layer
4. A **Prompt Manager**
5. An **Experimentation & Evals** framework
6. And **Security & Compliance** protocols

Here’s a detailed overview of how each feature contributes to productionizing your app:

#### **🚪 AI Gateway**:

* [**Automated Fallbacks & Retries**](../portkey-features/ai-gateway/fallbacks-on-llms.md): Ensure your application remains functional even if a primary service fails.
* [**Load Balancing**](../portkey-features/ai-gateway/load-balancing.md): Efficiently distribute incoming requests among multiple models.
* [**Semantic Caching**](../portkey-features/ai-gateway/simple-and-semantic-cache.md): Reduce costs and latency by intelligently caching results.

#### **🔬 Observability**:

* [**Logging**](../portkey-features/observability/logs-and-analytics.md): Keep track of all requests for monitoring and debugging.
* [**Requests Tracing**](../portkey-features/observability/request-tracing.md): Understand the journey of each request for optimization.
* [**Custom Tags**](../portkey-features/observability/custom-metadata.md): Segment and categorize requests for better insights.

#### **📝 Continuous Improvement with User Feedback**:

* [**Feedback Collection**](../portkey-features/feedback.md): Seamlessly gather feedback on any served request, be it on a generation or conversation level.
* **Weighted Feedback**: Obtain nuanced information by attaching weights to user feedback values.
* [**Feedback Metadata**](../portkey-features/feedback.md): Incorporate custom metadata with the feedback to provide context, allowing for richer insights and analyses.

#### **💻 Dynamic Prompt Management**

* [**Prompt Creation & Management**](../portkey-features/model-management.md): Seamlessly create and manage your prompts within Portkey's user-friendly UI. Deploy these with just an API call.
* **Multi-provider Support:** Portkey supports an extensive range of AI providers, including OpenAI, Anthropic, Cohere, Azure, and Huggingface.
* **Prompt Versioning:** Track modifications, make adjustments, and deploy different versions to your production environment as needed.
* **Raw Mode**: Define prompts with full JSON flexibility, catering to the most complex and specific requirements.

#### 🧪 Experimentation & Evals

* **A/B Test:** Execute A/B tests programmatically, utilizing custom weights to meticulously compare any model parameters, whether it be model name, provider, temperature, top\_p, or any other aspect!

#### 🔐 **Security & Compliance**

* [**Virtual Keys**](../portkey-features/security-and-compliance/virtual-keys.md): Portkey transforms original provider keys into virtual keys, ensuring your primary credentials remain untouched.
* **Redact Sensitive Data**: Automatically remove sensitive data from your requests to prevent indavertent exposure.
* **Access Control & Inbound Rules**: Control which IPs and Geos can connect to your Portkey deployments and stay compliant with your SOC2 policies.
* **Compliant with Global Standards**: Portkey complies with security and privacy best practices with standard certifications.
