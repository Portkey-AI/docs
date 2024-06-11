# Introduction

This documentation provides detailed information about the various ways you can access and interact with Portkey - **a robust AI gateway** designed to simplify and enhance your experience with Large Language Models (LLMs) like OpenAI's GPT models.&#x20;

Whether you're integrating directly with OpenAI, using a framework like Langchain or LlamaIndex, or building standalone applications, Portkey offers a flexible, secure, and efficient way to manage and deploy AI-powered features.

## 3 Ways to Integrate Portkey

Portkey can be accessed through three primary methods, each catering to different use cases and integration requirements:

### **1. Portkey SDKs (Python and JavaScript)**

**Ideal for:** standalone applications or when you're not already using the OpenAI integration. The SDKs are also highly recommended for seamless integration with frameworks like Langchain and LlamaIndex.

The Portkey SDKs are available in Python and JavaScript, designed to provide a streamlined, code-first approach to integrating LLMs into your applications.

#### Installing the SDK

Choose the SDK that matches your development environment:

{% tabs %}
{% tab title="NodeJS" %}
```bash
npm install portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```javascript
pip install portkey_ai
```
{% endtab %}
{% endtabs %}

#### Usage

Once installed, you can use the SDK to make calls to LLMs, manage prompts, handle keys, and more, all through a simple and intuitive API.

Learn more [here](portkey-sdk-client.md).

### **2. OpenAI SDK through the Portkey Gateway**

**Ideal for:** if you're currently utilizing OpenAI's Python or Node.js SDKs. By changing the base URL and adding Portkey-specific headers, you can quickly integrate Portkey's features into your existing setup.

Learn more [here](../welcome/integration-guides/openai.md).

### **3. REST API**

**Ideal for:** applications that prefer RESTful services. The base URL for all REST API requests is `https://api.portkey.ai/v1`, with an [authentication](authentication.md) header.

Learn more [here](../provider-endpoints/chat.md).
