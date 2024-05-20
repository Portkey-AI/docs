# Portkey API Headers

When making requests to the Portkey API, you need to include specific headers to authenticate your requests, specify the provider, and configure various features. This guide explains the essential and optional headers you can use to build applications on top of Portkey.

## Essential Headers

### 1. API Key (`x-portkey-api-key`)

The `x-portkey-api-key` header is required to authenticate your requests and ensure secure access to the Portkey API. You can obtain your API key from the [Portkey dashboard](https://app.portkey.ai/).

Example:

```bash
"x-portkey-api-key: $PORTKEY_API_KEY" \
```

### 2. Provider Information

In addition to the API key, you must provide information about the AI provider you're using. There are four ways to do this:

**2.1. Provider (`x-portkey-provider`) + Authentication Header**

* `x-portkey-provider`: Specifies the provider you're using (e.g., "openai", "anthropic"). See the list of [Portkey-integrated providers](../welcome/integration-guides/).
* `Authorization` (or another appropriate auth header like `x-api-key` or `api-key`): Includes the necessary authentication details for the specified provider.

Example:

```bash
"x-portkey-provider: openai" \
"Authorization: Bearer $OPENAI_API_KEY" \
```

**2.2. Virtual Key (`x-portkey-virtual-key`)**

* `x-portkey-virtual-key`: Allows you to use a pre-configured virtual key that encapsulates the provider and authentication details.&#x20;
* Virtual keys can be created and managed through the Portkey dashboard. ([Docs](../product/ai-gateway-streamline-llm-integrations/virtual-keys/))

Example:

```bash
"x-portkey-virtual-key: your_virtual_key_here" \
```

**2.3. Config (`x-portkey-config`)**

* `x-portkey-config`: Accepts a JSON object or a config ID that contains the provider details and other configuration settings. Using a config object provides flexibility and allows for dynamic configuration of your requests.&#x20;
* Configs can be saved in the Portkey UI and referenced by their ID as well. ([Docs](../product/ai-gateway-streamline-llm-integrations/configs.md))
* Configs also enable other optional features like Caching, Load Balancing, Fallback, Retries, and Timeouts.&#x20;

Example with JSON Object:

```bash
'x-portkey-config: {"provider": "openai", "api_key": "OPENAI_API_KEY"}' \
```

Example with Config ID:

```bash
'x-portkey-config: pp-config-xx' \
```

**2.4. Custom Host (`x-portkey-custom-host`) + Provider (x-portkey-provider) + Authentication Header**

* Use this combination when connecting to a custom-hosted provider endpoint.
* `x-portkey-custom-host` specifies the URL of the custom endpoint. ([Docs](../welcome/integration-guides/byollm.md))
* `x-portkey-provider` indicates the provider type.
* `Authorization` (or the appropriate auth header) includes the authentication details for the custom endpoint.

***

## Optional Headers

There are additional optional Portkey headers that enable various features and enhancements:

**1. Trace ID (`x-portkey-trace-id`)**

* `x-portkey-trace-id`: An ID you can pass to refer to one or more requests later on. If not provided, Portkey generates a trace ID automatically for each request.  ([Docs](../product/observability-modern-monitoring-for-llms/traces.md))

Example:

```bash
"x-portkey-trace-id: your_custom_trace_id" \
```

#### 2. Metadata (`x-portkey-metadata`)

* `x-portkey-metadata`: Allows you to attach custom metadata to your requests, which can be filtered later in the analytics and log dashboards. You can include the special metadata type `_user` to associate requests with specific users. ([Docs](../product/observability-modern-monitoring-for-llms/metadata.md))

Example:

```bash
"x-portkey-metadata: {"_user": "user_id_123", "foo": "bar"}"
```

#### 3. Cache Force Refresh (`x-portkey-cache-force-refresh`)

* `x-portkey-cache-force-refresh`: Forces a cache refresh for your request by making a new API call and storing the updated value. See the caching documentation for more information.  ([Docs](../product/ai-gateway-streamline-llm-integrations/cache-simple-and-semantic.md))

Example:

```bash
"x-portkey-cache-force-refresh: true" \
```

#### 4. Forward Headers (`x-portkey-forward-headers`)

* `x-portkey-forward-headers`: Allows you to forward sensitive headers directly to your model's API without any processing by Portkey. ([Docs](https://portkey.ai/docs/welcome/integration-guides/byollm#forward-sensitive-headers-securely))

Example:

```bash
"x-portkey-forward-headers: ["X-Custom-Header", "Another-Header"]" \
```

***

## Using Headers in SDKs

You can send these headers through REST API calls as well as by using the OpenAI or Portkey SDKs. With the Portkey SDK, Other than `cacheForceRefresh`, `traceID`, and `metadata`, rest of the headers are passed while instantiating the Portkey client.

{% tabs %}
{% tab title="Portkey Node SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai';

const portkey = new Portkey({
<strong>    apiKey: "PORTKEY_API_KEY",
</strong><strong>//  authorization: "Bearer PROVIDER_API_KEY",
</strong><strong>//  provider: "anthropic",
</strong><strong>//  customHost: "CUSTOM_URL",
</strong><strong>//  forwardHeaders: ["authorization"],
</strong><strong>    virtualKey: "VIRTUAL_KEY",
</strong><strong>    config: "CONFIG_ID",   
</strong>})

const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-4o',
},{
<strong>    traceId: "your_trace_id", 
</strong><strong>    metadata: {"_user": "432erf6"}
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Portkey Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
<strong>    api_key="PORTKEY_API_KEY",
</strong><strong>##  authorization="Bearer PROVIDER_API_KEY",
</strong><strong>##  provider="openai",
</strong><strong>##  custom_host="CUSTOM_URL",
</strong><strong>##  forward_headers=["authorization"],
</strong><strong>    virtual_key="VIRTUAL_KEY",
</strong><strong>    config="CONFIG_ID"
</strong>)

completion = portkey.with_options(
<strong>    trace_id = "TRACE_ID", 
</strong><strong>    metadata = {"_user": "user_12345"}
</strong>)chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'gpt-4o'
)
</code></pre>
{% endtab %}
{% endtabs %}

For a comprehensive list of all available parameters and their detailed descriptions, please refer to the Portkey SDK Client documentation.

{% content-ref url="portkey-sdk-client.md" %}
[portkey-sdk-client.md](portkey-sdk-client.md)
{% endcontent-ref %}
