# Portkey SDK Client

The Portkey SDK client enables various features of Portkey in an easy to use `config-as-code` paradigm.

### **Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Portkey's gateway.

{% tabs %}
{% tab title="NodeJS" %}
```bash
npm install --save portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```bash
pip install portkey-ai
```
{% endtab %}
{% endtabs %}

### **Export Portkey API Key**

```bash
export PORTKEY_API_KEY=""
```

## Basic Client Setup

The basic Portkey SDK client needs _**2 required parameters**_

1. The Portkey Account's API key to authenticate all your requests
2. The [virtual key](../product/ai-gateway-streamline-llm-integrations/virtual-keys/#using-virtual-keys) of the AI provider you want to use\
   OR\
   The [config](config-object.md) being used

This is achieved through headers when you're using the REST API.

For example,

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai';

// Construct a client with a virtual key
const portkey = new Portkey({
<strong>    apiKey: "PORTKEY_API_KEY",
</strong><strong>    virtualKey: "VIRTUAL_KEY"
</strong>})

// Construct a client with a config id
const portkey = new Portkey({
<strong>    apiKey: "PORTKEY_API_KEY",
</strong><strong>    config: "cf-***" // Supports a string config slug or a config object
</strong>})
</code></pre>

Find more info on what's available through [configs here](config-object.md).
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

# Construct a client with a virtual key
portkey = Portkey(
<strong>    api_key="PORTKEY_API_KEY",
</strong><strong>    virtual_key="VIRTUAL_KEY"
</strong>)

# Construct a client with provider and provider API key
portkey = Portkey(
<strong>    api_key="PORTKEY_API_KEY",
</strong><strong>    config="cf-***" # Supports a string config slug or a config object
</strong>)
</code></pre>

Find more info on what's available through [configs here](config-object.md).
{% endtab %}

{% tab title="cURL" %}
<pre class="language-bash"><code class="lang-bash">curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
<strong>  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
</strong><strong>  -H "x-portkey-virtual-key: $VIRTUAL_KEY" \ 
</strong>  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user","content": "Hello!"}]
  }'

curl https://api.portkey.ai/v1/chat/completions \
  -H 'Content-Type: application/json' \
<strong>  -H 'x-portkey-api-key: $PORTKEY_API_KEY' \
</strong><strong>  -H 'x-portkey-config: cf-***' \ 
</strong>  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user","content": "Hello!"}]
  }'
</code></pre>

Find more info on what's available through [configs here](config-object.md).
{% endtab %}
{% endtabs %}

## Making a Request

You can then use the client to make completion and other calls like this

{% tabs %}
{% tab title="NodeJS" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python" %}
```python
completion = portkey.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'gpt-3.5-turbo'
)
```
{% endtab %}
{% endtabs %}

### Passing Trace ID or Metadata

You can choose to override the configuration in individual requests as well and send trace id or metadata along with each request.&#x20;

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
}, {
<strong>    traceId: "39e2a60c-b47c-45d8", 
</strong><strong>    metadata: {"_user": "432erf6"}
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python"><strong>completion = portkey.with_options(
</strong><strong>    trace_id = "TRACE_ID", 
</strong><strong>    metadata = {"_user": "USER_IDENTIFIER"}
</strong>).chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'gpt-3.5-turbo'
)
</code></pre>
{% endtab %}
{% endtabs %}

***

## Async Usage

Portkey's Python SDK supports **Async** usage - just use **`AsyncPortkey`** instead of **`Portkey`** with **`await`**:

{% tabs %}
{% tab title="Python" %}
```python
import asyncio
from portkey_ai import AsyncPortkey

portkey = AsyncPortkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="VIRTUAL_KEY"
)

async def main():
    chat_completion = await portkey.chat.completions.create(
        messages=[{'role': 'user', 'content': 'Say this is a test'}],
        model='gpt-4'
    )

    print(chat_completion)

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

***

## Parameters

Following are the parameter keys that you can add while creating the Portkey client.&#x20;

Keeping in tune with the most popular language conventions, we use:

* **camelCase** for **Javascript** keys
* **snake\_case** for **Python** keys
* **hyphenated-keys** for the **headers**&#x20;

{% tabs %}
{% tab title="NodeJS" %}
<table><thead><tr><th width="384">Parameter</th><th width="97">Type<select multiple><option value="e8618b5391cc4b0692bd961a5c54befe" label="string" color="blue"></option><option value="ad1bd085879248dfac91455e550b4c62" label="object" color="blue"></option><option value="55c3ea4c5f8843f9af086f6fa3e02091" label="required" color="blue"></option><option value="dZalFSqJXRdc" label="boolean" color="blue"></option><option value="1zQQsMveb7IZ" label="array of string" color="blue"></option></select></th><th>Key</th></tr></thead><tbody><tr><td><strong>API Key</strong><br>Your Portkey account's API Key.</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string, </span><span data-option="55c3ea4c5f8843f9af086f6fa3e02091">required</span></td><td><code>apiKey</code></td></tr><tr><td><strong>Virtual Key</strong><br>The virtual key created from Portkey's vault for a specific provider</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>virtualKey</code></td></tr><tr><td><strong>Config</strong><br>The slug or <a href="config-object.md">config object</a> to use</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string, </span><span data-option="ad1bd085879248dfac91455e550b4c62">object</span></td><td><code>config</code></td></tr><tr><td><strong>Provider</strong><br>The AI provider to use for your calls. (<a href="../welcome/integration-guides/#supported-ai-providers">supported providers</a>).</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>provider</code></td></tr><tr><td><strong>Base URL</strong><br>You can edit the URL of the gateway to use. Needed if you're <a href="https://github.com/Portkey-AI/gateway/blob/main/docs/installation-deployments.md">self-hosting the AI gateway</a></td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>baseURL</code></td></tr><tr><td><strong>Trace ID</strong><br>An ID you can pass to refer to 1 or more requests later on. Generated automatically for every request, if not sent.</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>traceID</code></td></tr><tr><td><strong>Metadata</strong><br>Any metadata to attach to the requests. These can be filtered later on in the analytics and log dashboards<br><br>Can contain <code>_prompt</code>, <code>_user</code>, <code>_organisation</code>, or <code>_environment</code> that are special metadata types in Portkey.<br><br>You can also send any other keys as part of this object.</td><td><span data-option="ad1bd085879248dfac91455e550b4c62">object</span></td><td><code>metadata</code></td></tr><tr><td><strong>Cache Force Refresh</strong><br>Force refresh the cache for your request by making a new call and storing that value.</td><td><span data-option="dZalFSqJXRdc">boolean</span></td><td><code>cacheForceRefresh</code></td></tr><tr><td><strong>Cache Namespace</strong><br>Partition your cache based on custom strings, ignoring metadata and other headers.</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>cacheNamespace</code></td></tr><tr><td><strong>Custom Host</strong><br>Route to locally or privately hosted model by configuring the API URL with custom host</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>customHost</code></td></tr><tr><td><strong>Forward Headers</strong><br>Forward sensitive headers directly to your model's API without any processing from Portkey.</td><td><span data-option="1zQQsMveb7IZ">array of string</span></td><td><code>forwardHeaders</code></td></tr><tr><td><strong>Azure OpenAI Headers</strong><br>Configuration headers for Azure OpenAI that you can send separately</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>azureResourceName</code><br><code>azureDeploymentId</code><br><code>azureApiVersion</code></td></tr><tr><td><strong>Google Vertex AI Headers</strong><br>Configuration headers for Vertex AI that you can send separately</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>vertexProjectId</code><br><code>vertexRegion</code></td></tr><tr><td><strong>AWS Bedrock Headers</strong><br>Configuration headers for Bedrock that you can send separately</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>awsAccessKeyId awsSecretAccessKey awsRegion awsSessionToken</code></td></tr></tbody></table>
{% endtab %}

{% tab title="Python" %}
<table><thead><tr><th width="364">Parameter</th><th width="97">Type<select multiple><option value="e8618b5391cc4b0692bd961a5c54befe" label="string" color="blue"></option><option value="ad1bd085879248dfac91455e550b4c62" label="object" color="blue"></option><option value="55c3ea4c5f8843f9af086f6fa3e02091" label="required" color="blue"></option><option value="2lp2NVspT4yx" label="boolean" color="blue"></option><option value="r2DmNWSIDfq4" label="array of string" color="blue"></option></select></th><th>Key</th></tr></thead><tbody><tr><td><strong>API Key</strong><br>Your Portkey account's API Key.</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string, </span><span data-option="55c3ea4c5f8843f9af086f6fa3e02091">required</span></td><td><code>api_key</code></td></tr><tr><td><strong>Virtual Key</strong><br>The virtual key created from Portkey's vault for a specific provider</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>virtual_key</code></td></tr><tr><td><strong>Config</strong><br>The slug or <a href="config-object.md">config object</a> to use</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string, </span><span data-option="ad1bd085879248dfac91455e550b4c62">object</span></td><td><code>config</code></td></tr><tr><td><strong>Provider</strong><br>The AI provider to use for your calls. (<a href="../welcome/integration-guides/#supported-ai-providers">supported providers</a>).</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>provider</code></td></tr><tr><td><strong>Base URL</strong><br>You can edit the URL of the gateway to use. Needed if you're <a href="https://github.com/Portkey-AI/gateway/blob/main/docs/installation-deployments.md">self-hosting the AI gateway</a></td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>base_url</code></td></tr><tr><td><strong>Trace ID</strong><br>An ID you can pass to refer to 1 or more requests later on. Generated automatically for every request, if not sent.</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>trace_id</code></td></tr><tr><td><strong>Metadata</strong><br>Any metadata to attach to the requests. These can be filtered later on in the analytics and log dashboards<br><br>Can contain <code>_prompt</code>, <code>_user</code>, <code>_organisation</code>, or <code>_environment</code> that are special metadata types in Portkey.<br><br>You can also send any other keys as part of this object.</td><td><span data-option="ad1bd085879248dfac91455e550b4c62">object</span></td><td><code>metadata</code></td></tr><tr><td><strong>Cache Force Refresh</strong><br>Force refresh the cache for your request by making a new call and storing that value.</td><td><span data-option="2lp2NVspT4yx">boolean</span></td><td><code>cache_force_refresh</code></td></tr><tr><td><strong>Cache Namespace</strong><br>Partition your cache based on custom strings, ignoring metadata and other headers.</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>cache_namespace</code></td></tr><tr><td><strong>Custom Host</strong><br>Route to locally or privately hosted model by configuring the API URL with custom host</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>custom_host</code></td></tr><tr><td><strong>Forward Headers</strong><br>Forward sensitive headers directly to your model's API without any processing from Portkey.</td><td><span data-option="r2DmNWSIDfq4">array of string</span></td><td><code>forward_headers</code></td></tr><tr><td><strong>Azure OpenAI Headers</strong><br>Configuration headers for Azure OpenAI that you can send separately</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>azure_resource_name</code><br><code>azure_deployment_id</code><br><code>azure_api_version</code></td></tr><tr><td><strong>Google Vertex AI Headers</strong><br>Configuration headers for Vertex AI that you can send separately</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>vertex_project_id</code><br><code>vertex_region</code></td></tr><tr><td><strong>AWS Bedrock Headers</strong><br>Configuration headers for Bedrock that you can send separately</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>aws_access_key_id</code><br><code>aws_secret_access_key</code><br><code>aws_region</code><br><code>aws_session_token</code></td></tr></tbody></table>


{% endtab %}

{% tab title="REST Headers" %}
<table><thead><tr><th width="364">Parameter</th><th width="97">Type<select multiple><option value="e8618b5391cc4b0692bd961a5c54befe" label="string" color="blue"></option><option value="ad1bd085879248dfac91455e550b4c62" label="object" color="blue"></option><option value="55c3ea4c5f8843f9af086f6fa3e02091" label="required" color="blue"></option><option value="3kNzGLeekH64" label="boolean" color="blue"></option><option value="LYRekkvOxbts" label="array of string" color="blue"></option></select></th><th>Header Key</th></tr></thead><tbody><tr><td><strong>API Key</strong><br>Your Portkey account's API Key.</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string, </span><span data-option="55c3ea4c5f8843f9af086f6fa3e02091">required</span></td><td><code>x-portkey-api-key</code></td></tr><tr><td><strong>Virtual Key</strong><br>The virtual key created from Portkey's vault for a specific provider</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>x-portkey-virtual-key</code></td></tr><tr><td><strong>Config</strong><br>The slug or <a href="config-object.md">config object</a> to use</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>x-portkey-config</code></td></tr><tr><td><strong>Provider</strong><br>The AI provider to use for your calls. (<a href="../welcome/integration-guides/#supported-ai-providers">supported providers</a>).</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>x-portkey-provider</code></td></tr><tr><td><strong>Base URL</strong><br>You can edit the URL of the gateway to use. Needed if you're <a href="https://github.com/Portkey-AI/gateway/blob/main/docs/installation-deployments.md">self-hosting the AI gateway</a></td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td>Change the request URL</td></tr><tr><td><strong>Trace ID</strong><br>An ID you can pass to refer to 1 or more requests later on. Generated automatically for every request, if not sent.</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>x-portkey-trace-id</code></td></tr><tr><td><strong>Metadata</strong><br>Any metadata to attach to the requests. These can be filtered later on in the analytics and log dashboards<br><br>Can contain <code>_prompt</code>, <code>_user</code>, <code>_organisation</code>, or <code>_environment</code> that are special metadata types in Portkey.<br><br>You can also send any other keys as part of this object.</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>x-portkey-metadata</code></td></tr><tr><td><strong>Cache Force Refresh</strong><br>Force refresh the cache for your request by making a new call and storing that value.</td><td><span data-option="3kNzGLeekH64">boolean</span></td><td><code>x-portkey-cache-force-refresh</code></td></tr><tr><td><strong>Cache Namespace</strong><br>Partition your cache based on custom strings, ignoring metadata and other headers</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>x-portkey-cache-namespace</code></td></tr><tr><td><strong>Custom Host</strong><br>Route to locally or privately hosted model by configuring the API URL with custom host</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>x-portkey-custom-host</code></td></tr><tr><td><strong>Forward Headers</strong><br>Forward sensitive headers directly to your model's API without any processing from Portkey.</td><td><span data-option="LYRekkvOxbts">array of string</span></td><td><code>x-portkey-forward-headers</code></td></tr><tr><td><strong>Azure OpenAI Headers</strong><br>Configuration headers for Azure OpenAI that you can send separately</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>x-portkey-azure-resource-name</code><br><code>x-portkey-azure-deployment-id</code><br><code>x-portkey-azure-api-version</code><br><code>api-key</code></td></tr><tr><td><strong>Google Vertex AI Headers</strong><br>Configuration headers for Vertex AI that you can send separately</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>x-portkey-vertex-project-id</code><br><code>x-portkey-vertex-region</code></td></tr><tr><td><strong>AWS Bedrock Headers</strong><br>Configuration headers for Bedrock that you can send separately</td><td><span data-option="e8618b5391cc4b0692bd961a5c54befe">string</span></td><td><code>x-portkey-aws-session-token</code><br><code>x-portkey-aws-secret-access-key</code><br><code>x-portkey-aws-region</code><br><code>x-portkey-aws-session-token</code></td></tr></tbody></table>
{% endtab %}
{% endtabs %}
