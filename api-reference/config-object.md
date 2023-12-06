# Config Object

The `config` object is used to configure API interactions with various providers. It supports multiple modes such as single provider access, load balancing between providers, and fallback strategies.

Here are some sample config objects:

```json
// Simple config with cache and retry
{
  "virtual_key": "***", // Your Virtual Key
  "cache": { // Optional
    "mode": "semantic",
    "max_age": 10000
  },
  "retry": { // Optional
    "count": 5,
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

| Key Name          | Description                                                  | Type             | Required                                | Enum Values                                                 | Additional Info                     |
| ----------------- | ------------------------------------------------------------ | ---------------- | --------------------------------------- | ----------------------------------------------------------- | ----------------------------------- |
| `strategy`        | Operational strategy for the config or any individual target | object           | Yes (if no `provider` or `virtual_key`) | -                                                           | See Strategy Object Details         |
| `provider`        | Name of the service provider                                 | string           | Yes (if no `mode` or `virtual_key`)     | "openai", "anthropic", "azure-openai", "anyscale", "cohere" | -                                   |
| `api_key`         | API key for the service provider                             | string           | Yes (if `provider` is specified)        | -                                                           | -                                   |
| `virtual_key`     | Virtual key identifier                                       | string           | Yes (if no `mode` or `provider`)        | -                                                           | -                                   |
| `cache`           | Caching configuration                                        | object           | No                                      | -                                                           | See Cache Object Details            |
| `retry`           | Retry configuration                                          | object           | No                                      | -                                                           | See Retry Object Details            |
| `weight`          | Weight for load balancing                                    | number           | No                                      | -                                                           | Used in `loadbalance` mode          |
| `on_status_codes` | Status codes triggering fallback                             | array of strings | No                                      | -                                                           | Used in `fallback` mode             |
| `targets`         | List of target configurations                                | array            | Yes (if `mode` is specified)            | -                                                           | Each item follows the config schema |

### Strategy Object Details

<table><thead><tr><th width="137">Key Name</th><th width="130">Description</th><th width="107">Type</th><th>Required</th><th width="133">Enum Values</th><th>Additional Info</th></tr></thead><tbody><tr><td><code>mode</code></td><td>strategy mode for the config</td><td>string</td><td>Yes</td><td>"loadbalance", "fallback"</td><td></td></tr><tr><td><code>on_status_codes</code></td><td>status codes to apply the strategy. This field is only used when strategy mode is "fallback"</td><td>array of numbers</td><td>No</td><td></td><td>Optional</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td></tr></tbody></table>

### Cache Object Details

| Key Name  | Description                   | Type    | Required | Enum Values          | Additional Info |
| --------- | ----------------------------- | ------- | -------- | -------------------- | --------------- |
| `mode`    | Cache mode                    | string  | Yes      | "simple", "semantic" | -               |
| `max_age` | Maximum age for cache entries | integer | No       | -                    | Optional        |

### Retry Object Details

| Key Name          | Description                     | Type             | Required | Enum Values | Additional Info |
| ----------------- | ------------------------------- | ---------------- | -------- | ----------- | --------------- |
| `count`           | Number of retry attempts        | integer          | Yes      | -           | -               |
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
    "count": 5,
    "on_status_codes": ["429"]
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
          "on_status_codes": ["429", "241"]
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

These examples demonstrate different use cases of the configuration object, helping users to understand how they can structure their own configurations based on the schema. Feel free to add more examples or adjust these as necessary to match your specific use cases and requirements.

## JSON Schema

The following JSON schema is used to validate the config object

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

