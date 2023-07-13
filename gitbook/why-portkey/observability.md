# 👀 Observability

Generative AI applications often have numerous moving parts and dependencies, making it crucial to have a way to monitor and understand system behavior and performance. Observability allows you to get insights into what is happening inside your application and can help identify issues or opportunities for optimization.

Here's how Portkey can enhance the observability of your AI applications:

### Detailed Analytics

Portkey offers a comprehensive analytics dashboard with detailed metrics on various aspects of your application. These metrics include:

* **Request count:** Monitor the total number of requests made to different Large Language Models (LLMs).
* **Cost:** Keep track of the expenses associated with each LLM.
* **Latency:** Understand the time taken to process requests, which can help identify performance bottlenecks.
* **Tokens:** Monitor the token consumption per request.
* **Cache hits:** Understand the efficiency of caching strategies by monitoring the cache hit rate.

These metrics provide a wealth of information that allows you to gain valuable insights into your application's behavior and performance, enabling you to make data-driven decisions.

### Comprehensive Logging

Portkey offers robust logging, which can be incredibly useful for debugging and optimizing your AI applications. The logging feature keeps track of all requests passed through Portkey in chronological order. It acts as a managed data store, storing all raw request data which can exported anytime.

### Segmenting and Analyzing LLM Spends

Portkey's custom metadata properties allow you to segment, filter, and analyze your LLM spends in a granular way. This is particularly useful for multi-tenant systems or SaaS companies, which often need to understand usage and costs on a per-customer basis.

For instance, if you run a SaaS company with multiple customers, you can pass customer-specific identifiers as metadata in your Portkey requests. These identifiers then appear in the Portkey dashboard, where you can filter and segment costs, tokens used, request count, and latency by customer. This feature gives you insights into the usage patterns of individual customers and allows you to attribute costs accurately.

\
