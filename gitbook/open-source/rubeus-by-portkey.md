---
description: Blazing-fast AI Gateway
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Rubeus by Portkey

Rubeus is an open-source AI gateway that reflects Portkey's commitment to productionizing LLM systems. Engineered for blazing-fast performance, Rubeus can be either self-hosted or set up on edge, serving as a cohesive interface for all LLM interactions.

Rubeus streamlines 20+ LLMs under a unified interace and makes them interoperable so you can swap out a model or a provider without changing underlying code. On top of this, to make your app production-grade, Rubeus adds features like automated fallbacks & retries and load balacing to your systems with a single line code change.

**For a detailed overview and list of supported providers,** [**please visit the Rubeus Github repo**](https://github.com/portkey-ai/rubeus)**.**

***

## Official SDK

We offer an official SDK for Rubeus with Portkey. This includes extended features around observability and experimentation. [**Access the SDK documentation here.**](../sdk/)

### **Production Features Supported with Portkey SDK**

#### **🚪 AI Gateway:**

* Unified API Signature: If you've used OpenAI, you already know how to use Portkey with any other provider.
* Interoperability: Write once, run with any provider. Switch between _any model_ from _any provider_ seamlessly.
* Automated Fallbacks & Retries: Ensure your application remains functional even if a primary service fails.
* Load Balancing: Efficiently distribute incoming requests among multiple models.
* Semantic Caching: Reduce costs and latency by intelligently caching results.

#### **🔬 Observability:**

* Logging: Keep track of all requests for monitoring and debugging.
* Requests Tracing: Understand the journey of each request for optimization.
* Custom Tags: Segment and categorize requests for better insights.
