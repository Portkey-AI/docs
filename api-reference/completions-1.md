---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Image Generation

## Create Image

`POST /images/generations`&#x20;

Generate images using the selected provider and model

{% swagger src="../.gitbook/assets/openapi.yaml" path="/images/generations" method="post" %}
[openapi.yaml](../.gitbook/assets/openapi.yaml)
{% endswagger %}



Pass the config parameters for the request in the headers as defined [here](../product/ai-gateway-streamline-llm-integrations/configs.md).

Portkey automatically transforms the parameters for image models according the parameters accepted by them.

## SDK Usage

The `images.create` method in the Portkey or OpenAI SDK allows you to generate images using various image models. This method provides a straightforward interface for requesting image generations.

### Method Signature

{% tabs %}
{% tab title="OpenAI NodeJS" %}
```js
client.images.create(requestParams[configParams]);
```
{% endtab %}

{% tab title="OpenAI Python" %}
```py
# with only request params
client.images.create(requestParams);
```
{% endtab %}
{% endtabs %}



### Parameters

1. **requestParams (Object):** Parameters for the completion request. These parameters should include the prompt and model, and are transformed automatically by Portkey for image models.&#x20;
2. **configParams (Object):** Additional configuration options for the request. This is an optional parameter that can include custom config options for this specific request. These will override the configs set in the Portkey Client.

### Example Usage

<details>

<summary>REST API Example</summary>

In REST calls, `x-portkey-api-key` is a compulsory header, it can be paired with the following options for sending provider details:

1. `x-portkey-provider` & `Authorization` (or similar auth headers)
2. `x-portkey-virtual-key`&#x20;
3. `x-portkey-config`

**Example request using Provider + Auth:**

<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/images/generations" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-provider: openai" \
</strong><strong>  -H "Authorization: Bearer $OPENAI_API_KEY" \
</strong>  -d '{
    "prompt": "A cute baby sea otter",
    "model": "dall-e-3",
    "n": 1
  }'
</code></pre>

**Example request using Virtual Key:**&#x20;

<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/images/generations" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-virtual-key: openai-virtual-key" \
</strong>  -d '{
    "prompt": "A cute baby sea otter",
    "model": "dall-e-3",
    "n": 1
  }'
</code></pre>

**Example request using Config:**

<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/images/generations" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
<strong>  -H "x-portkey-config: config-key" \
</strong>  -d '{
    "prompt": "A cute baby sea otter",
    "model": "dall-e-3",
    "n": 1
  }'
</code></pre>

**You can send 3 other headers in your Portkey requests**

* `x-portkey-trace-id`: Send trace id&#x20;
* `x-portkey-metadata`: Send custom metadata
* `x-portkey-cache-force-refresh`: Force refresh cache for this request

**Example request using these 3:**

```bash
curl "https://api.portkey.ai/v1/images/generations" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-config: config-key" \
  -H "x-portkey-trace-id: $UNIQUE_TRACE_ID" \
  -H "x-portkey-metadata: {\"_user\":\"john\"}" \
  -H "x-portkey-cache-force-refresh: True" \
  -d '{
    "prompt": "A cute baby sea otter",
    "model": "dall-e-3",
    "n": 1
  }'
```

</details>

### Response Format

The response will conform to the Image Generation Object schema from the Portkey API, typically including the generated image based on the prompt and the selected model.
