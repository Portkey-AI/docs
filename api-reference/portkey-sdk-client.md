# Portkey SDK Client

The Portkey SDK client enables various features of Portkey in a easy to use `config-as-code` paradigm.

### **Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Portkey's gateway.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
npm install --save portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```python
pip install portkey-ai
```
{% endtab %}
{% endtabs %}

### **2. Initialize Portkey with the Virtual Key**

## Basic Client Setup

The basic Portkey SDK client needs 2 parameters

1. The Portkey Account's API key to authenticate all your requests _(required)_
2. The [virtual key](../product/ai-gateway-streamline-llm-integrations/virtual-keys.md#using-virtual-keys) of the AI provider you want to use. \
   OR\
   The [config](config-object.md) being used

This is achieved through headers when you're using the REST API.

For example,

{% tabs %}
{% tab title="NodeJS" %}
```javascript
// Construct a client with a virtual key
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "VIRTUAL_KEY"
})

// Construct a client with a config id
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    config: "cf-***" // Supports a string config slug or a config object
})
```

Find more info on what's available through [configs here](config-object.md).
{% endtab %}

{% tab title="Python" %}
```python
# Construct a client with a virtual key
portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="VIRTUAL_KEY"
)

# Construct a client with provider and provider API key
portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    config="cf-***" # Supports a string config slug or a config object
)
```

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
  -H 'x-portkey-api-key: $PORTKEY_API_KEY' \
  -H 'x-portkey-config: cf-***' \ 
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user","content": "Hello!"}]
  }'
</code></pre>

Find more info on what's available through [configs here](config-object.md).
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="NodeJS" %}
```javascript
// Construct a client with a virtual key
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "VIRTUAL_KEY"
})

// Construct a client with a config id
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    config: "cf-***" // Supports a string config slug or a config object
})
```

Find more info on what's available through [configs here](config-object.md).
{% endtab %}

{% tab title="Python" %}
```python
# Construct a client with a virtual key
portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="VIRTUAL_KEY"
)

# Construct a client with provider and provider API key
portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    config="cf-***" # Supports a string config slug or a config object
)
```

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
  -H 'x-portkey-api-key: $PORTKEY_API_KEY' \
  -H 'x-portkey-config: cf-***' \ 
  -d '{
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

You can choose to override the configuration in individual requests as well. This is specifically used when passing trace IDs or metadata.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
}, {
    traceId: "39e2a60c-b47c-45d8", 
    metadata: {"_user": "432erf6"}
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python" %}
```python
completion = portkey.with_options(
    trace_id = "TRACE_ID", 
    metadata = {"_user": "USER_IDENTIFIER"}
).chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'gpt-3.5-turbo'
)
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="NodeJS" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
}, {
    traceId: "39e2a60c-b47c-45d8", 
    metadata: {"_user": "432erf6"}
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python" %}
```python
completion = portkey.with_options(
    trace_id = "TRACE_ID", 
    metadata = {"_user": "USER_IDENTIFIER"}
).chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'gpt-3.5-turbo'
)
```
{% endtab %}
{% endtabs %}

## Parameters

Following are the parameter keys that you can add while creating the Portkey client. We use camelCase for Javascript keys, snake case for Python keys and hyphenated keys for the headers keeping in tune with the most popular language conventions.

{% tabs %}
{% tab title="NodeJS" %}
<table><thead><tr><th width="384">Parameter</th><th width="97" data-type="select" data-multiple>Type</th><th>Key</th></tr></thead><tbody><tr><td><strong>API Key</strong><br>Your Portkey account's API Key.</td><td></td><td><code>apiKey</code></td></tr><tr><td><strong>Virtual Key</strong><br>The virtual key created from Portkey's vault for a specific provider</td><td></td><td><code>virtualKey</code></td></tr><tr><td><strong>Config</strong><br>The slug or <a href="config-object.md">config object</a> to use</td><td></td><td><code>config</code></td></tr><tr><td><strong>Provider</strong><br>The AI provider to use for your calls. (<a href="../getting-started/integration-guides/#supported-ai-providers">supported providers</a>).</td><td></td><td><code>provider</code></td></tr><tr><td><strong>Base URL</strong><br>You can edit the URL of the gateway to use. Needed if you're self-hosting Rubeus</td><td></td><td><code>baseURL</code></td></tr><tr><td><strong>Trace ID</strong><br>An ID you can pass to refer to 1 or more requests later on. Generated automatically for every request, if not sent.</td><td></td><td><code>traceID</code></td></tr><tr><td><strong>Metadata</strong><br>Any metadata to attach to the requests. These can be filtered later on in the analytics and log dashboards<br><br>Can contain <code>_prompt</code>, <code>_user</code>, <code>_organisation</code>, or <code>_environment</code> that are special metadata types in Portkey.<br><br>You can also send any other keys as part of this object.</td><td></td><td><code>metadata</code></td></tr></tbody></table>


{% endtab %}

{% tab title="Python" %}
<table><thead><tr><th width="365">Parameter</th><th width="97" data-type="select" data-multiple>Type</th><th>Key</th></tr></thead><tbody><tr><td><strong>API Key</strong><br>Your Portkey account's API Key.</td><td></td><td><code>api_key</code></td></tr><tr><td><strong>Virtual Key</strong><br>The virtual key created from Portkey's vault for a specific provider</td><td></td><td><code>virtual_key</code></td></tr><tr><td><strong>Config</strong><br>The slug or <a href="config-object.md">config object</a> to use</td><td></td><td><code>config</code></td></tr><tr><td><strong>Provider</strong><br>The AI provider to use for your calls. (<a href="../getting-started/integration-guides/#supported-ai-providers">supported providers</a>).</td><td></td><td><code>provider</code></td></tr><tr><td><strong>Base URL</strong><br>You can edit the URL of the gateway to use. Needed if you're self-hosting Rubeus</td><td></td><td><code>base_url</code></td></tr><tr><td><strong>Trace ID</strong><br>An ID you can pass to refer to 1 or more requests later on. Generated automatically for every request, if not sent.</td><td></td><td><code>trace_id</code></td></tr><tr><td><strong>Metadata</strong><br>Any metadata to attach to the requests. These can be filtered later on in the analytics and log dashboards<br><br>Can contain <code>_prompt</code>, <code>_user</code>, <code>_organisation</code>, or <code>_environment</code> that are special metadata types in Portkey.<br><br>You can also send any other keys as part of this object.</td><td></td><td><code>metadata</code></td></tr></tbody></table>
{% endtab %}

{% tab title="REST Headers" %}
<table><thead><tr><th width="364">Parameter</th><th width="97" data-type="select" data-multiple>Type</th><th>Header Key</th></tr></thead><tbody><tr><td><strong>API Key</strong><br>Your Portkey account's API Key.</td><td></td><td><code>x-portkey-api-key</code></td></tr><tr><td><strong>Virtual Key</strong><br>The virtual key created from Portkey's vault for a specific provider</td><td></td><td><code>x-portkey-virtual-key</code></td></tr><tr><td><strong>Config</strong><br>The slug or <a href="config-object.md">config object</a> to use</td><td></td><td><code>x-portkey-config</code></td></tr><tr><td><strong>Provider</strong><br>The AI provider to use for your calls. (<a href="../getting-started/integration-guides/#supported-ai-providers">supported providers</a>).</td><td></td><td><code>x-portkey-provider</code></td></tr><tr><td><strong>Base URL</strong><br>You can edit the URL of the gateway to use. Needed if you're self-hosting Rubeus</td><td></td><td>Change the request URL</td></tr><tr><td><strong>Trace ID</strong><br>An ID you can pass to refer to 1 or more requests later on. Generated automatically for every request, if not sent.</td><td></td><td><code>x-portkey-trace-id</code></td></tr><tr><td><strong>Metadata</strong><br>Any metadata to attach to the requests. These can be filtered later on in the analytics and log dashboards<br><br>Can contain <code>_prompt</code>, <code>_user</code>, <code>_organisation</code>, or <code>_environment</code> that are special metadata types in Portkey.<br><br>You can also send any other keys as part of this object.</td><td></td><td><code>x-portkey-metadata</code></td></tr></tbody></table>
{% endtab %}
{% endtabs %}

**Config Parameter**

The value of the config parameter can be the slug of the config created in the Portkey UI or an object representing the config to be used. The object has the following JSON schema
