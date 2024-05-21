# Config Object

The `config` object is used to configure API interactions with various providers. It supports multiple modes such as single provider access, load balancing between providers, and fallback strategies.

**The following JSON schema is used to validate the config object:**

<details>

<summary>JSON Schema</summary>

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "strategy": {
      "type": "object",
      "properties": {
        "mode": {
          "type": "string",
          "enum": [
            "single",
            "loadbalance",
            "fallback"
          ]
        },
        "on_status_codes": {
          "type": "array",
          "items": {
            "type": "integer"
          },
          "optional": true
        }
      }
    },
    "provider": {
      "type": "string",
      "enum": [
        "openai",
        "anthropic",
        "azure-openai",
        "anyscale",
        "cohere",
        "palm"
      ]
    },
    "resource_name": {
      "type": "string",
      "optional": true
    },
    "deployment_id": {
      "type": "string",
      "optional": true
    },
    "api_version": {
      "type": "string",
      "optional": true
    },
    "override_params": {
      "type": "object"
    },
    "api_key": {
      "type": "string"
    },
    "virtual_key": {
      "type": "string"
    },
    "cache": {
      "type": "object",
      "properties": {
        "mode": {
          "type": "string",
          "enum": [
            "simple",
            "semantic"
          ]
        },
        "max_age": {
          "type": "integer",
          "optional": true
        }
      },
      "required": [
        "mode"
      ]
    },
    "retry": {
      "type": "object",
      "properties": {
        "attempts": {
          "type": "integer"
        },
        "on_status_codes": {
          "type": "array",
          "items": {
            "type": "number"
          },
          "optional": true
        }
      },
      "required": [
        "attempts"
      ]
    },
    "weight": {
      "type": "number"
    },
    "on_status_codes": {
      "type": "array",
      "items": {
        "type": "integer"
      }
    },
    "targets": {
      "type": "array",
      "items": {
        "$ref": "#"
      }
    }
  },
  "anyOf": [
    {
      "required": [
        "provider",
        "api_key"
      ]
    },
    {
      "required": [
        "virtual_key"
      ]
    },
    {
      "required": [
        "strategy",
        "targets"
      ]
    },
    {
      "required": [
        "cache"
      ]
    },
    {
      "required": [
        "retry"
      ]
    }
  ],
  "additionalProperties": false
}
```

</details>

## Example Configs

```json
// Simple config with cache and retry
{
  "virtual_key": "***", // Your Virtual Key
  "cache": { // Optional
    "mode": "semantic",
    "max_age": 10000
  },
  "retry": { // Optional
    "attempts": 5,
    "on_status_codes": []
  }
}

// Load balancing with 2 OpenAI keys
{
  "strategy": {
      "mode": "loadbalance"
    },
  "targets": [
    {
      "provider": "openai",
      "api_key": "sk-***"
    },
    {
      "provider": "openai",
      "api_key": "sk-***"
    }
  ]
}
```

You can find more examples of schemas [below](config-object.md#examples).

## Schema Details

<table><thead><tr><th width="203">Key Name</th><th width="225">Description</th><th width="157">Type</th><th width="169">Required</th><th width="190">Enum Values</th><th>Additional Info</th></tr></thead><tbody><tr><td><code>strategy</code></td><td>Operational strategy for the config or any individual target</td><td>object</td><td>Yes (if no <code>provider</code> or <code>virtual_key</code>)</td><td>-</td><td>See Strategy Object Details</td></tr><tr><td><code>provider</code></td><td>Name of the service provider</td><td>string</td><td>Yes (if no <code>mode</code> or <code>virtual_key</code>)</td><td>"openai", "anthropic", "azure-openai", "anyscale", "cohere"</td><td>-</td></tr><tr><td><code>api_key</code></td><td>API key for the service provider</td><td>string</td><td>Yes (if <code>provider</code> is specified)</td><td>-</td><td>-</td></tr><tr><td><code>virtual_key</code></td><td>Virtual key identifier</td><td>string</td><td>Yes (if no <code>mode</code> or <code>provider</code>)</td><td>-</td><td>-</td></tr><tr><td><code>cache</code></td><td>Caching configuration</td><td>object</td><td>No</td><td>-</td><td>See Cache Object Details</td></tr><tr><td><code>retry</code></td><td>Retry configuration</td><td>object</td><td>No</td><td>-</td><td>See Retry Object Details</td></tr><tr><td><code>weight</code></td><td>Weight for load balancing</td><td>number</td><td>No</td><td>-</td><td>Used in <code>loadbalance</code> mode</td></tr><tr><td><code>on_status_codes</code></td><td>Status codes triggering fallback</td><td>array of strings</td><td>No</td><td>-</td><td>Used in <code>fallback</code> mode</td></tr><tr><td><code>targets</code></td><td>List of target configurations</td><td>array</td><td>Yes (if <code>mode</code> is specified)</td><td>-</td><td>Each item follows the config schema</td></tr><tr><td><code>request_timeout</code></td><td>Request timeout configuration</td><td>number</td><td>No</td><td>-</td><td>-</td></tr><tr><td><code>custom_host</code></td><td>Route to privately hosted model</td><td>string</td><td>No</td><td>-</td><td>Used in combination with <code>provider</code> + <code>api_key</code></td></tr><tr><td><code>forward_headers</code></td><td>Forward sensitive headers directly</td><td>array of strings</td><td>No</td><td>-</td><td>-</td></tr><tr><td><code>override_params</code></td><td>Pass model name and other hyper parameters</td><td>object</td><td>No</td><td>"model", "temperature", "frequency_penalty", "logit_bias", "logprobs", "top_logprobs", "max_tokens", "n", "presence_penalty", "response_format", "seed", "stop", "top_p", etc.</td><td>Pass everything that's typically part of the payload</td></tr></tbody></table>

### Strategy Object Details

<table><thead><tr><th width="215">Key Name</th><th width="242">Description</th><th width="107">Type</th><th>Required</th><th width="133">Enum Values</th><th>Additional Info</th></tr></thead><tbody><tr><td><code>mode</code></td><td>strategy mode for the config</td><td>string</td><td>Yes</td><td>"loadbalance", "fallback"</td><td></td></tr><tr><td><code>on_status_codes</code></td><td>status codes to apply the strategy. This field is only used when strategy mode is "fallback"</td><td>array of numbers</td><td>No</td><td></td><td>Optional</td></tr></tbody></table>

### Cache Object Details

| Key Name  | Description                   | Type    | Required | Enum Values          | Additional Info |
| --------- | ----------------------------- | ------- | -------- | -------------------- | --------------- |
| `mode`    | Cache mode                    | string  | Yes      | "simple", "semantic" | -               |
| `max_age` | Maximum age for cache entries | integer | No       | -                    | Optional        |

### Retry Object Details

| Key Name          | Description                     | Type             | Required | Enum Values | Additional Info |
| ----------------- | ------------------------------- | ---------------- | -------- | ----------- | --------------- |
| `attempts`        | Number of retry attempts        | integer          | Yes      | -           | -               |
| `on_status_codes` | Status codes to trigger retries | array of strings | No       | -           | Optional        |

### Notes

* The strategy `mode` key determines the operational mode of the config. If strategy `mode` is not specified, a single provider mode is assumed, requiring either `provider` and `api_key` or `virtual_key`.
* In `loadbalance` and `fallback` modes, the `targets` array specifies the configurations for each target.
* The `cache` and `retry` objects provide additional configurations for caching and retry policies, respectively.

## Examples

<details>

<summary>Single Provider with API Key</summary>

```json
{
  "provider": "openai",
  "api_key": "sk-***"
}
```

</details>

<details>

<summary>Passing Model &#x26; Hyperparameters with Override Option</summary>

<pre class="language-json"><code class="lang-json">{
  "provider": "anthropic",
  "api_key": "xxx",
<strong>  "override_params": {
</strong><strong>    "model": "claude-3-sonnet-20240229",
</strong><strong>    "max_tokens": 512,
</strong><strong>    "temperature": 0
</strong><strong>  }
</strong>}
</code></pre>

</details>

<details>

<summary>Single Provider with Virtual Key</summary>

```json
{
  "virtual_key": "***"
}
```

</details>

<details>

<summary>Single Provider with Virtual Key, Cache and Retry</summary>

```json
{
  "virtual_key": "***",
  "cache": {
    "mode": "semantic",
    "max_age": 10000
  },
  "retry": {
    "attempts": 5,
    "on_status_codes": [429]
  }
}
```

</details>

<details>

<summary>Load Balancing with Two OpenAI API Keys</summary>

```json
{
  "strategy": {
      "mode": "loadbalance"
    },
  "targets": [
    {
      "provider": "openai",
      "api_key": "sk-***"
    },
    {
      "provider": "openai",
      "api_key": "sk-***"
    }
  ]
}
```

</details>

<details>

<summary>Load Balancing and Fallback Combination</summary>

```json
{
  "strategy": {
      "mode": "loadbalance"
    },
  "targets": [
    {
      "provider": "openai",
      "api_key": "sk-***"
    },
    {
      "strategy": {
          "mode": "fallback",
          "on_status_codes": [429, 241]
        },
      "targets": [
        {
          "virtual_key": "***"
        },
        {
          "virtual_key": "***"
        }
      ]
    }
  ]
}
```

</details>
