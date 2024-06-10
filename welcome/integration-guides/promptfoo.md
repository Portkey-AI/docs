# Promptfoo

[**`promptfoo`**](https://promptfoo.dev/docs/intro) is an open source library (and CLI) for evaluating LLM output quality.

Portkey brings advanced **AI gateway** capabilities, full-stack **observability**, and **prompt management** + **versioning** to your **Promptfoo** projects. This document provides an overview of how to leverage the strengths of both the platforms to streamline your AI development workflow.

#### By using Portkey with Promptfoo you can:

1. [Manage, version, and collaborate on various prompts with Portkey and easily call them in Promptfoo](promptfoo.md#id-1.-reference-prompts-from-portkey-in-promptfoo)
2. [Run Promptfoo on 200+ LLMs, including locally or privately hosted LLMs](promptfoo.md#id-2.-route-to-anthropic-google-groq-and-more)
3. [Log all requests, segment them as needed with custom metadata, and get granular cost, performance metrics for all Promptfoo runs](promptfoo.md#id-3.-segment-requests-view-cost-and-performance-metrics)
4. [Avoid Promptfoo rate limits & leverage cache](promptfoo.md#id-4.-avoid-promptfoo-rate-limits-and-leverage-cache)

Let’s see how these work!

***

## 1. Reference Prompts from Portkey in Promptfoo

1. ​​Set the `PORTKEY_API_KEY` environment variable in your Promptfoo project
2. In your configuration YAML, use the `portkey://` prefix for your prompts, followed by your Portkey prompt ID.&#x20;

For example:

<pre class="language-yaml"><code class="lang-yaml">prompts:
<strong>  - portkey://pp-test-promp-669f48
</strong>
providers:
  - openai:gpt-3.5-turbo-0613

tests:
  - vars:
      topic: ...
</code></pre>

Variables from your Promptfoo test cases will be automatically plugged into the Portkey prompt as variables. The resulting prompt will be rendered and returned to promptfoo, and used as the prompt for the test case.

{% hint style="warning" %}
Note that promptfoo does not follow the temperature, model, and other parameters set in Portkey. You must set them in the providers configuration yourself.
{% endhint %}

***

## 2. Route to Anthropic, Google, Groq, and More

1. ​​Set the **`PORTKEY_API_KEY`** environment variable
2. While adding the provider in your config YAML, set the model name with **`portkey`** prefix (like `portkey:gpt-4o`)
3. And in the **`config`** param, set the relevant provider for the above chosen model with **`portkeyProvider`** (like `portkeyProvider:openai`)

### For Example, to Call OpenAI

<pre class="language-yaml"><code class="lang-yaml">providers:
<strong>  id: portkey:gpt-4o
</strong>  config:
<strong>    portkeyProvider: openai
</strong></code></pre>

That's it! With this, all your Promptfoo calls will now start showing up on your Portkey dashboard.

### Let's now call `Anthropic`, `Google`, `Groq`, `Ollama`

{% tabs %}
{% tab title="Anthropic" %}
<pre class="language-yaml"><code class="lang-yaml">providers:
<strong>  id: portkey:claude-3-opus20240229
</strong>  config:
<strong>    portkeyProvider: anthropic
</strong>    # You can also add your Anthropic API key to Portkey and pass the virtual key here
<strong>    portkeyVirtualKey: ANTHROPIC_VIRTUAL_KEY
</strong></code></pre>
{% endtab %}

{% tab title="Google Gemini" %}
<pre class="language-yaml"><code class="lang-yaml">providers:
<strong>  id: portkey:gemini-1.5-flash-latest
</strong>  config:
<strong>    portkeyProvider: google
</strong>    # You can also add your Gemini API key to Portkey and pass the virtual key here
<strong>    portkeyVirtualKey: GEMINI_VIRTUAL_KEY
</strong></code></pre>
{% endtab %}

{% tab title="Groq" %}
<pre class="language-yaml"><code class="lang-yaml">providers:
<strong>  id: portkey:llama3-8b-8192
</strong>  config:
<strong>    portkeyProvider: groq
</strong>    # You can also add your Groq API key to Portkey and pass the virtual key here
<strong>    portkeyVirtualKey: GROQ_VIRTUAL_KEY
</strong></code></pre>
{% endtab %}

{% tab title="Ollama" %}
<pre class="language-yaml"><code class="lang-yaml">providers:
<strong>  id: portkey:llama3
</strong>  config:
<strong>    portkeyProvider: ollama
</strong><strong>    portkeyCustomHost: YOUR_OLLAMA_NGROK_URL
</strong></code></pre>
{% endtab %}
{% endtabs %}

### Examples for `Azure OpenAI`, `AWS Bedrock`, `Google Vertex AI`

{% tabs %}
{% tab title="Azure OpenAI" %}
#### Using [Virtual Keys](azure-openai.md#id-2.-initialize-portkey-with-the-virtual-key)

<pre class="language-yaml"><code class="lang-yaml">providers:
  id: portkey:xxx
  config:
<strong>    portkeyVirtualKey: YOUR_PORTKEY_AZURE_OPENAI_VIRTUAL_KEY
</strong></code></pre>

#### Without Using Virtual Keys

First, set the `AZURE_OPENAI_API_KEY` environment variable.

<pre class="language-yaml"><code class="lang-yaml">providers:
  id: portkey:xxx
  config:
<strong>    portkeyProvider: azure-openai
</strong><strong>    portkeyAzureResourceName: AZURE_RESOURCE_NAME
</strong><strong>    portkeyAzureDeploymentId: AZURE_DEPLOYMENT_NAME
</strong><strong>    portkeyAzureApiVersion: AZURE_API_VERSION
</strong></code></pre>

#### Using Client Credentials (JSON Web Token)

You can generate a JSON web token for your client creds, and add it to the `AZURE_OPENAI_API_KEY` environment variable.

```yaml
providers:
  id: portkey:xxx
  config:
    portkeyProvider: azure-openai
    portkeyAzureResourceName: AZURE_RESOURCE_NAME
    portkeyAzureDeploymentId: AZURE_DEPLOYMENT_NAME
    portkeyAzureApiVersion: AZURE_API_VERSION
    # Pass your JSON Web Token with AZURE_OPENAI_API_KEY env var
    portkeyForwardHeaders: [ "authorization" ]
```
{% endtab %}

{% tab title="AWS Bedrock" %}
#### Using [Virtual Keys](aws-bedrock.md#id-2.-initialize-portkey-with-the-virtual-key)

<pre class="language-yaml"><code class="lang-yaml">providers:
  id: portkey:anthropic.claude-3-sonnet-20240229-v1:0
  config:
<strong>    portkeyVirtualKey: YOUR_PORTKEY_AWS_BEDROCK_VIRTUAL_KEY
</strong>    # If you're using AWS Security Token Service, you can set it here
<strong>    awsSessionToken: "AWS_SESSION_TOKEN"
</strong></code></pre>

#### Without Using Virtual Keys

First, set the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` env vars.

<pre class="language-yaml"><code class="lang-yaml">providers:
  id: portkey:anthropic.claude-3-sonnet-20240229-v1:0
  config:
<strong>    portkeyProvider: bedrock
</strong><strong>    portkeyAwsRegion: "us-east-1"
</strong><strong>    portkeyAwsSecretAccessKey: ${AWS_SECRET_ACCESS_KEY}
</strong><strong>    portkeyAwsAccessKeyId: ${AWS_ACCESS_KEY_ID}
</strong>    # You can also set AWS STS (Security Token Service)
<strong>    awsSessionToken: "AWS_SESSION_TOKEN"
</strong></code></pre>
{% endtab %}

{% tab title="Google Vertex AI" %}
#### Using [Virtual Keys](vertex-ai.md#id-2.-initialize-portkey-with-the-virtual-key)

Set your Vertex AI access token with the `VERTEX_API_KEY` env var, and pass the rest of your Vertex AI details with Portkey virtual key.

<pre class="language-yaml"><code class="lang-yaml">providers:
  id: portkey:gemini-1.5-flash-latest
  config:
<strong>    portkeyVirtualKey: YOUR_PORTKEY_GOOGLE_VERTEX_AI_VIRTUAL_KEY
</strong></code></pre>

#### Without Using Virtual Keys

First, set the `VERTEX_API_KEY`, `VERTEX_PROJECT_ID`, `VERTEX_REGION` env vars.

<pre class="language-yaml"><code class="lang-yaml">providers:
  id: portkey:xxx
  config:
<strong>    portkeyProvider: vertex-ai
</strong><strong>    portkeyVertexProjectId: ${VERTEX_PROJECT_ID}
</strong><strong>    portkeyVertexRegion: ${VERTEX_REGION}
</strong></code></pre>
{% endtab %}
{% endtabs %}

***

## 3. Segment Requests, View Cost & Performance Metrics

Portkey automatically logs all the key details about your requests, including cost, tokens used, response time, request and response bodies, and more.

Using Portkey, you can also send custom metadata with each of your requests to further segment your logs for better analytics. Similarly, you can also trace multiple requests to a single trace ID and filter or view them separately in Portkey logs.

```yaml
providers:
  id: portkey:claude-3-opus20240229
  config:
    portkeyVirtualKey: ANTHROPIC_VIRTUAL_KEY
    portkeyMetadata:
      team: alpha9
      prompt: classification
    portkeyTraceId: run_1
```

You can filter or group data by these metadata keys on Portkey dashboards.

<figure><img src="../../.gitbook/assets/promptfoo.gif" alt=""><figcaption></figcaption></figure>

***

## 4. Avoid Promptfoo Rate Limits & Leverage Cache

Since promptfoo can make a lot of calls very quickly, you can use a loadbalanced config in Portkey with cache enabled. You can pass the config header similar to virtual keys in promptfoo.

Here's a sample Config that you can save in the Portkey UI and get a respective config slug:

```json
{
	"cache": { "mode": "simple" },
	"strategy": { "mode": "loadbalance" },
	"targets": [
		{ "virtual_key": "ACCOUNT_ONE" },
		{ "virtual_key": "ACCOUNT_TWO" },
		{ "virtual_key": "ACCOUNT_THREE" }
	]
}
```

And then we can just add the saved Config's slug in the YAML:

```yaml
providers:
  id: portkey:claude-3-opus20240229
  config:
    portkeyVirtualKey: ANTHROPIC_VIRTUAL_KEY
    portkeyConfig: PORTKEY_CONFIG_SLUG
```

***

## 🚧 \[Roadmap] View the Results of Promptfoo Evals in Portkey

We’re building support to view the eval results of promptfoo in Portkey that will let you view the results of promptfoo evals within the feedback section of Portkey.\
